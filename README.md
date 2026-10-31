Act as a Senior Software Architect. Analyse the entire current repository (`CAI.Mstr.DMSPlus.UI.OneView`) and explain its architecture in very simple terms so that a beginner or new team member can understand how the application works.

**Important:** Analyse the actual source code, package.json, configuration files, API integrations, build scripts and deployment configuration. Do not assume technologies or invent components.

### 1. Application overview
- What is this application?
- What business problem does it solve?
- What are its main functionalities?
- Who uses this application?

### 2. Architecture diagram
Generate a simple, easy-to-understand architecture diagram.

Show the flow:

User → Frontend UI → Backend/API → Database or External Services

Include actual components found in this repository.

Use simple boxes and arrows. Avoid unnecessary technical complexity.

### 3. Technology stack

Identify the actual technologies and frameworks used.

Explain in a table:

| Layer | Technology / Framework | Purpose |
| Frontend | Actual framework | UI development |
| Programming Language | Actual language | Application logic |
| UI Libraries | Actual libraries | UI components |
| State Management | Actual library | Managing application state |
| API Integration | Actual approach | Backend communication |
| Build Tools | Actual tools | Building application |
| Testing | Actual tools | Unit/integration testing |
| Code Quality | Actual tools | Linting and formatting |
| Deployment | Actual tools | Application deployment |
| Infrastructure | Actual tools | Cloud/infrastructure management |

Only include technologies confirmed in the repository.

### 4. Project folder structure
Explain the important folders in simple bullet points:
- src
- public
- docs
- terraform
- build
- .github
- Any other important folders

Explain what each folder contains and why it exists.

### 5. Application execution flow
Explain step by step:
1. What happens when a user opens the application?
2. How does the frontend load?
3. How are components rendered?
4. How does the application communicate with APIs?
5. How is data received and displayed?
6. How are errors handled?
7. How is the application built and deployed?

### 6. Frameworks and libraries
List the important frameworks and libraries from package.json and explain each in one simple sentence.

### 7. Final summary
Provide:
- One simple architecture diagram.
- Technology stack in bullet points.
- Application execution flow in 5–10 steps.
- Five important things a developer should understand before working on this repository.

### Presentation guidelines
- Use beginner-friendly English.
- Prefer simple bullet points.
- Avoid unnecessary technical jargon.
- Use Mermaid diagrams.
- Keep the explanation concise and visually clear.
- Clearly distinguish confirmed information from anything that cannot be determined from the source code.
- Do not modify any source files. This is an analysis-only task.
