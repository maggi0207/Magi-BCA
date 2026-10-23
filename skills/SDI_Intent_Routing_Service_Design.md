# SDI Intent Routing Service — Simple Design

**Status:** Proposed design for review  
**Purpose:** Separate SDI's intent-routing decision from the runtime so routing can be reused, tested, deployed, and scaled independently.

## 1. The main decision

**The LLM makes the routing decision. Deterministic code only validates that decision.**

We will not use keyword rules or deterministic logic to choose a domain instead of the LLM. Normalization or schema checks are acceptable, but they must not become an alternative intent classifier.

The routing service will return a structured decision. The existing SDI runtime will validate it against registered domains/capabilities and will own the subsequent procedure selection, context/filter handling, authorization, skill execution, and response generation.

## 2. Why change the current architecture?

Today, intent selection and routing logic are part of the SDI runtime's selector/orchestration flow. As SDI adds more skills and specialist agents, routing becomes a reusable capability that should have a clear API, independent tests, releases, monitoring, and scaling.

The goal is to separate the routing responsibility—not to rewrite the agent or move data execution into the router.

## 3. Proposed architecture

```text
User
  |
  v
SDI UI + Express BFF              ssc-planning-sdi-ui
  |
  v
Planning API / Access Gate        ssc-planning-sdi-ser
  |
  v
SDI Runtime / Orchestrator        ssc-planning-sdi-rt
  |  loads current conversation context
  |  sends current message + small routing context
  v
Intent Routing Service            NEW repository/service
  |  LLM chooses the route
  |  deterministic validation checks the response only
  v
Structured routing decision
  |
  v
SDI Runtime validates route, selects procedure, resolves filters,
checks authorization, executes approved skill/MCP, and builds response
  |
  v
Response streams back through the existing path to the UI
```

The router returns a decision to the runtime. It does not directly call agents, MCP tools, SQL, Snowflake, or other execution services.

## 4. What happens inside the routing service?

1. Receive the current user message and minimal routing context from the runtime.
2. Load or use the approved routing catalogue containing available domains and their descriptions/aliases.
3. Ask the configured LLM to select the best domain and, if included in the agreed contract, broad capability.
4. Validate that the LLM response follows the required schema and contains a supported domain/capability.
5. Return the decision, or return a structured invalid/ambiguous/no-route outcome. Do not replace an invalid decision with a deterministic guess.

Example: for **“Show forecast accuracy for PID CAB-AC”**, the LLM may return `forecast` as the domain. The runtime then performs the detailed procedure and filter work.

## 5. Responsibilities

| Component | Responsibility | Not responsible for |
|---|---|---|
| Intent Routing Service | LLM-based intent classification and domain/capability decision; schema and catalogue validation | Context storage, SQL, MCP execution, authorization |
| Context handling in SDI runtime | Load/update conversation state and provide relevant context | Choosing a route independently of the LLM |
| SDI runtime/orchestrator | Validate route, resolve procedure and filters, enforce skill contracts, invoke approved execution, build response | Trusting model output without validation |
| Skill registry/catalogue | Describe supported destinations and available capabilities | Giving the LLM permission to execute arbitrary tools |
| Existing API/access layer | Authenticate/authorize the caller and pass trusted identity | Treating a route decision as access permission |
| MCP adapters | Execute approved tools with validated inputs | Intent routing |

## 6. What context will the router receive?

Keep it small. The runtime remains the source of truth for conversation memory and sends only information needed for routing.

| Field | Example | Required? |
|---|---|---|
| Current message | `What about forecast outlook?` | Yes |
| Previous domain, if relevant | `forecast` | Optional |
| Pending clarification, if any | `Which domain do you mean?` | Optional |
| Available domain/capability catalogue | Supported domain names and descriptions | Yes, supplied from trusted server-side configuration/catalogue |
| Request ID | `req-123` | Yes for tracing/correlation |

Do not send the full transcript, full warehouse rows, credentials, or detailed filters unless a validated future requirement shows they are needed for routing. PID, BU, Product Family, planner, detailed procedure, and filter-inheritance logic should remain in the runtime initially.

## 7. How context memory will work

The routing service should be stateless: it does not keep memory between requests. The SDI runtime owns conversation context.

For each user turn:

1. Runtime loads the conversation state from the configured persistence mechanism.
2. Runtime sends the message and minimal routing context to the routing service.
3. LLM returns a route; deterministic checks validate the response but do not choose a different route.
4. Runtime validates the route against registered skills, chooses the procedure, and resolves/validates filters.
5. Runtime executes the approved skill/MCP path or asks for clarification.
6. Runtime updates context according to the outcome, keeping the last successful result separate from errors, greetings, and clarification responses.

PostgreSQL-backed LangGraph checkpointing has not been confirmed in the reviewed code/deployment. Verify the existing deployment first. Do not introduce a second database just for routing. If persistence is missing or does not meet requirements, assess the approved platform options separately.

## 8. What does the router need from skills?

The router needs a lightweight routing catalogue, not complete skill implementation details.

| Information | Needed? | Example |
|---|---|---|
| Domain name | Yes | `forecast`, `rma`, `install_base` |
| Plain-language description and common aliases | Yes | Forecast outlook and forecast accuracy questions |
| Broad capabilities, if part of Phase 1 contract | Yes, if agreed | `forecast` capability; later a more specific capability taxonomy if needed |
| Enabled/available status | Yes | Whether a destination is registered and available |
| Example utterances for evaluation | Recommended | Typical user questions per domain |
| Detailed procedure parameters and filter rules | No, initially | PID, BU, product family, months, planner |
| SQL scripts, MCP connection details, credentials | No | Execution-only information |

The catalogue must be maintained from trusted configuration or registered skill metadata. Model output must be validated against it. An LLM cannot invent a new domain, skill, or execution tool.

## 9. API proposal

Use a versioned internal HTTP JSON API, such as `POST /v1/route`. The exact path follows platform conventions.

### Request example

```json
{
  "request_id": "req-123",
  "message": "Show forecast accuracy for PID CAB-AC",
  "context": {
    "last_domain": "forecast",
    "pending_clarification": null
  },
  "routing_catalogue_version": "v1"
}
```

### Response example

```json
{
  "request_id": "req-123",
  "status": "routed",
  "domain": "forecast",
  "capability": "forecast",
  "reason_code": "ROUTE_SELECTED",
  "routing_version": "v1"
}
```

If the LLM response is invalid or the request is ambiguous, return a structured `invalid_decision`, `clarification_required`, `unsupported`, or `no_match` result as agreed in the API contract. Do not silently guess another destination. Do not return model chain-of-thought; use a short reason code or safe summary for observability.

## 10. Repository and deployment approach

Because independent deployment and scaling are long-term goals, the target should be a separate repository and service. Suggested name: `ssc-planning-sdi-router` (confirm with the team).

Suggested contents:

```text
ssc-planning-sdi-router/
  app/
    api/             # endpoints and request/response models
    routing/         # LLM routing request and response handling
    catalog/         # available domains/capabilities
    llm/             # configurable model/provider adapter
  tests/             # routing evaluation, contract, failure tests
  Dockerfile
  deployment/        # approved deployment manifests/templates
  README.md
```

The service should have its own CI/CD, configuration, health checks, metrics, logging, versioning, and rollback path. Model/provider settings should be externalized so model changes do not require source-code changes.

## 11. Reliability and security requirements

- Preserve existing authentication and authorization boundaries. A valid route does not grant data access.
- Validate all LLM output against a strict schema and trusted routing catalogue.
- Use timeouts and bounded retries for transient LLM/service errors. Do not retry indefinitely.
- If routing fails, return a safe error or clarification through the runtime; do not use a hidden deterministic route fallback.
- Keep secrets in approved secret management, not source code or logs.
- Use request IDs and structured logs, but do not log credentials, full prompts, or sensitive result rows.
- Monitor latency, failure rates, invalid decisions, clarification/no-route rates, and model usage/cost.
- Test ambiguous requests, invalid model output, unavailable destinations, timeout, cancellation, and service outage.

## 12. Implementation sequence

| Phase | Work | Exit criteria |
|---|---|---|
| 1. Agree contract | Define domain/capability vocabulary, request/response schema, and ambiguous/no-route statuses | Product and runtime owners approve contract |
| 2. Extract routing | Move LLM routing responsibility behind the service API; keep deterministic logic as validation only | Routing evaluation and contract tests pass |
| 3. Integrate runtime | Runtime sends message + minimal context and validates returned route before continuing | Existing execution and authorization safeguards remain enforced |
| 4. Deploy independently | Add repository, CI/CD, configuration, health checks, monitoring, and rollback | Service can be released/scaled independently |
| 5. Evaluate and tune | Measure correctness, latency, model cost, clarification rate, and failures | Agreed quality and latency targets met |

## 13. Decisions to confirm

1. Which domains and broad capabilities are in the first routing catalogue?
2. Should Phase 1 return domain only, or domain plus broad capability? Detailed procedure/filter extraction remains in the runtime either way.
3. Which LLM/provider configuration will the router use, and how will it be changed without code changes?
4. What are the timeout, retry, fallback, latency, and availability targets?
5. What approved deployment, secrets, monitoring, and service-to-service authentication standards must it follow?
6. Is PostgreSQL-backed LangGraph checkpointing already enabled in the deployed runtime? Verify separately; the router itself does not require a database initially.

## Summary

Create a standalone Intent Routing Service so the LLM's routing decision can be managed and scaled independently. **The LLM chooses the route; deterministic code only validates the returned decision.** The router remains stateless, receives minimal context from the runtime, and does not execute tools. The existing SDI runtime continues to own conversation memory, detailed procedure/filter handling, authorization, skill execution, and response generation.
