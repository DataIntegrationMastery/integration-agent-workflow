# Integration Agent Workflow
Developed by **[Data Integration Mastery](https://dataintegrationmastery.com/)**
**Version 1.9** / *5th Mar 2026*

A production-oriented agent workflow for integration development using **VS Code** and **GitHub Copilot Agents**.  

This repository implements a structured **PRD → PLAN → TASK** delivery model with strict role separation:

- **Integration Designer** – Architecture & PRD
- **Integration Planner** – TASK breakdown & execution planning
- **Integration Builder** – Implementation & commit discipline

The model follows the principle:

> **Human-as-Architect, AI-as-Builder**

---

## Table of Contents

1. [Purpose](#purpose)
   - [Why Not Use an Autonomous Coding Agent?](#why-not-use-an-autonomous-coding-agent)
2. [Installing VS Code and GitHub Copilot](#installing-vs-code-and-github-copilot)
   - [Install Visual Studio Code](#install-visual-studio-code)
   - [Install GitHub Copilot Extension](#install-github-copilot-extension)
   - [Licensing and Pricing](#licensing-and-pricing)
   - [Additional Environment Requirements for Agent-Based Development](#additional-environment-requirements-for-agent-based-development)
3. [Installing Integation Development Agents to Your Project](#installing-integation-development-agents-to-your-project)
   - [Step 1 — Getting this Repository as a Template to Your Project](#step-1--getting-this-repository-as-a-template-to-your-project)
   - [Step 2 — Bootstrap the Environment from the Design Document](#step-2--bootstrap-the-environment-from-the-design-document)
   - [Step 3 — Review the Changes](#step-3--review-the-changes)
4. [Using the Agents](#using-the-agents)
   - [Choosing the Language Models for the Agents](#choosing-the-language-models-for-the-agents)
   - [Handling the context](#handling-the-context)
   - [Workflow Phases](#workflow-phases)
   - [When Controll files must be Changed](#when-controll-files-must-be-changed)
5. [Demo Example](#demo-example)

---

# Purpose

This repository is a **template project** on GitHub. It contains the complete specification, agent definitions, templates, and rule system needed to bootstrap the Integration Agent Workflow in any integration project.

It demonstrates how to:

- Structure integration development using Copilot Agents
- Maintain architectural control while accelerating implementation
- Enforce vertical delivery (TASK-based execution)
- Keep repository documentation as the single source of truth
- Prevent role drift between design, planning, and coding

It is designed for real-world API and integration services (e.g., aggregation, enrichment, transformation).

**NOTE:** This Agent flow methodology is based on GitHub Copilot with precisely defined different agents in the VS Code development environment (can also be applied to other IDEs). More detailed descriptions of these agents can be found in the instructions: **[docs/developing-integrations-with-copilot.md](docs/developing-integrations-with-copilot.md)**


## Why Not Use an Autonomous Coding Agent?

Tools like Claude Code are powerful — but they operate at a different level
of abstraction. A single autonomous agent receives a high-level goal and
executes independently, combining architecture, planning, and implementation
in one loop.

For integration development, this is a problem.

Integration work is not a coding task. It is a contract between systems.
Every decision — retry policy, error handling, idempotency, observability —
has architectural consequences that cannot be delegated to an autonomous loop.

The three-agent model here makes a deliberate choice:

- The **human remains the architect** — not the AI
- Each phase requires **explicit approval** before the next begins
- The **audit trail is structural** — not accidental

An autonomous agent can write code faster.
This model produces integration solutions you can reason about, defend, and maintain.

> Architecture is a human responsibility.
> AI is the accelerator, not the decision-maker.

***

# Installing VS Code and GitHub Copilot

To get started, install Visual Studio Code and the GitHub Copilot extension using the official documentation to ensure you always follow the latest instructions for your operating system.

## Install Visual Studio Code

Download and follow the installation guide for Windows or macOS:
https://code.visualstudio.com/

## Install GitHub Copilot Extension

After installing VS Code, open the Extensions marketplace and install GitHub Copilot, or follow the official setup guide here:
https://docs.github.com/en/copilot/getting-started-with-github-copilot

The official documentation includes the most up-to-date steps for Windows and Mac, account authentication, subscription requirements, and enabling Copilot Chat and Agents.

***

## Licensing and Pricing

Visual Studio Code is free to use. GitHub Copilot requires a paid subscription for most professional use cases. GitHub offers Individual, Business, and Enterprise plans. Pricing and feature sets may change over time, so always verify the latest details on the official page:

https://github.com/features/copilot/pricing

***

## Additional Environment Requirements for Agent-Based Development

To work effectively with Copilot Agents (Designer, Planner, Builder), your development environment must also support the following:

### Git Installed and Configured

You must have Git installed and configured locally.
Download: https://git-scm.com/

Verify installation:
```
git --version
```
You should also configure:
	•	user.name
	•	user.email

***

### GitHub Access and Repository Cloning

You must be able to:
	•	Authenticate to GitHub (SSH or HTTPS)
	•	Clone repositories locally
	•	Push and pull changes

Example:
```
git clone https://github.com/<your-org>/<your-repo>.git
```
Agent workflows assume:
	•	The repository exists
	•	The project structure is version-controlled
	•	Commits can be created and pushed

Without proper Git access, Builder-agent workflows (commit-per-subtask) will not function correctly.

***

### Build Toolchain Installed

Depending on your stack (e.g., Spring Boot + Camel), you must install the required runtime and build tools, for example:
	•	Java (JDK 17+ recommended)
	•	Maven or Gradle

Verify:
```
java -version
mvn -version
```
Agents can generate code, but they rely on a working local build environment.

***

### Terminal Access Inside VS Code

Builder agents use execution tools to:
	•	Run builds
	•	Execute tests
	•	Validate compilation

Ensure:
	•	Integrated terminal works
	•	Project builds successfully before starting agent orchestration

***


### Installing summary

To successfully use Copilot Agents in a structured integration development workflow, you need:
	•	VS Code + Copilot installed
	•	Git properly configured
	•	GitHub repository access
	•	Working build toolchain

Copilot Agents enhance development — but they operate inside a properly configured engineering environment.

***

# Installing Integation Development Agents to Your Project

The goal is to let **GitHub Copilot Chat** update your current repository so that it implements the full Integration Agent environment described in instruction: **[docs/developing-integrations-with-copilot.md](docs/developing-integrations-with-copilot.md)**

## Step 1 — Getting this Repository as a Template to Your Project

To use this template as a starting point for your own project, add it as a temporary Git remote and copy the files:

```bash
git remote add template https://github.com/DataIntegrationMastery/integration-agent-workflow.git
git fetch template
git checkout template/main -- .    # copies all template files into your working directory
git remote remove template
```

After this, make sure:
- You are in the root of your repository in VS Code
- The file `docs/developing-integrations-with-copilot.md` exists 
- The directory `docs/rules` exists 
- Copilot Chat is open

---

## Step 2 — Bootstrap the Environment from the Design Document

Choose the latest Claude Opus model and copy and paste the following prompt into Copilot Chat:


```
You are an environment setup assistant.

Your task is to update this repository so that it fully implements the agent environment described in:

docs/developing-integrations-with-copilot.md

Instructions:
1. Read the entire file carefully before making changes.
2. Identify all required files, folders, and configuration structures.
3. Create or update the repository so that it matches the specification.

Specifically ensure:
- `.github/agents/` exists
- copilot-instructions.md exists in the repository root
- Required *template.md -files exists

Rules:
- Do NOT modify business source code.
- Do NOT generate example PoC implementations.
- Do NOT implement anything from instruction which is marked as 'OPTIONAL'
- Only create or update configuration and documentation files required by the workflow.
- If something already exists, update it to align with the specification rather than duplicating it.
- Do not introduce new architectural concepts not defined in the specification document.

After completing the base setup, check if the specification includes optional features (OPTIONs).
For each OPTION found in docs/developing-integrations-with-copilot.md:
- Ask the user if they want to enable it
- If yes: follow the detailed instructions provided in that OPTION section

After completing all changes:
- List all created or modified files.
- Briefly summarize what was aligned with the specification.
- Report which OPTIONs were enabled.
```
---

## Step 3 — Review the Changes

After Copilot completes:

1. Review all `.agent.md` files
2. Verify role separation:
   - Designer: read, search, web (controlled)
   - Planner: read, search, edit
   - Builder: read, search, edit, execute
3. Verify TASK structure under `/docs/tasks/`
4. Verify status model consistency
5. Confirm that PRD is defined as architectural source of truth

### **Critical: Review Project Context and Technology Stack**

The setup assistant generates two files that **require your input before agents can work correctly**:

| File | Purpose |
|------|---------|
| `docs/rules/project-context.md` | Defines project name, runtime, framework, API spec locations, deployment model |
| `docs/rules/technology-stack.md` | Defines mandatory technology constraints, allowed/forbidden libraries, conventions |

**These files contain placeholder values.** You must:

1. Open `docs/rules/project-context.md` and replace all `[...]` placeholders with your actual project details
2. Open `docs/rules/technology-stack.md` and define your actual technology rules and constraints
3. Verify that the agent `.agent.md` files reference these files correctly

> **Without accurate project context and technology stack rules, agents will produce generic or incorrect outputs.** This is the single most important manual step in the setup.



***


# **Using the Agents**

## **Choosing the Language Models for the Agents**

***Best models as of Feb 2026***

**Designer → *Claude Opus 4.6***

Reasons:

*   Excellent long-context architecture reasoning
*   Low hallucination rate
*   Ideal for PRD writing and NFR validation
*   Strong system-level thinking

Alternative: ***GPT‑5.2***

**Planner → *Claude Opus 4.6***

Reasons:

*   Excellent long-context planning
*   Low hallucination rate
*   Ideal for PRD/PLAN writing

Alternative: ***GPT‑5.2***

**Builder → *Claude Sonnet 4.6***

Reasons:

*   Strong code generation
*   Stable in agent-mode loop
*   Good for tests & error handling

Alternatives:

*   ***GPT‑5.3 Codex*** (complex Java/Camel)
*   ***Claude Haiku / GPT‑5 mini*** (fast small tasks)

## Handling the context

Agent memory comes from:

1.  System prompt
2.  `copilot-instructions.md`
3.  Current conversation
4.  Workspace files:
   *   RULEs
   *   PRDs
   *   PLANs
   *   TASKs 
   *   Other documents

### **Do not mix roles**

*   Use a separate chat for Designer
*   Use a separate chat for Planner
*   Use a separate chat for Builder

**To reset context**

Designer:

    Act as Integration Designer. Read the current PRD under /docs/prd/PRD-X.md. Ignore planning and build discussions.

Planner:

    Act as Integration Planner. Read PRD under /docs/prd/PRD-X.md. Ignore previous build discussions.

Builder:

    Act as Integration Builder. Implement TASK-02 from /docs/tasks. Ignore planning discussions.

***


## **Workflow Phases**

### **Phase 1 — Architecture & Requirements (PRD)**

A **PRD** (Product Requirements Document) is created by the **Integration Designer** and defines:
- Integration flow
- Canonical response model
- Business rules
- Non-functional requirements
- Error handling policy

**PRD is the architectural source of truth.**

Designer + Human:

*   Define problem
*   Define system landscape
*   Define flows
*   Define NFRs

*In a chat with Integration Designer:*

    Create PRD for feature X in /docs/prd.

Human approves.

### **Phase 2 — Execution Planning (PLAN)**

The **Integration Planner** translates the PRD into vertical delivery units.

A **PLAN** breaks work into **VERTICAL SLICE**s and **TASK**s:

- Each **TASK** represents one **end-to-end capability**
- Has its own folder under `/docs/tasks/TASK-XX/`
- Contains a `TASK-XX.md` root document
- May contain multiple `SUBTASK-XX.md` files

*Open a new chat and use Integration Planner:*

    Create plan from approved PRD. Include 3–6 TASKs.

Human approves.

### **Phase 3 — TASK Implementation**

Each **TASK** is a thin but end-to-end implementation of one complete integration flow — from source system to target system — including routing, transformation, error handling, and monitoring.

One **TASK** delivers a usable integration capability (e.g., an API endpoint, a message consumer, a data sync flow).

*Open a new chat and use Integration Builder:*

    Implement TASK-01 exactly as defined.

Builder:

*   Implements
*   Runs tests
*   Fixes issues
*   Reports

### **Phase 4 — Change Handling & Re‑planning**

If new requirements or information appears:

1.  Human → Designer **(new chat)** — evaluate architectural impact, update PRD
2.  Designer → Planner **(new chat)** — update plan based on PRD changes
3.  Planner → Builder **(new chat)** — implement updated TASKs

*In a chat with Integration Planner:*

    Update plan based on updated PRD and test findings.

***

## **When Controll files must be Changed**

If you update `.github/copilot-instructions.md` during development, make sure the agents actually use the new rules.

### Recommended (most reliable)
1.	Save and commit the updated copilot-instructions.md.
2.	Close the current chat.
3.	Start a new chat and select the correct agent.
4.	Begin with: 
```Act according to the updated .github/copilot-instructions.md. Confirm you are using the latest version.```

This ensures the agent reloads repository context and applies the new rules cleanly.

### Alternative (within the same chat)

If you do not want to restart the conversation, explicitly instruct the agent: 
``` Re-read .github/copilot-instructions.md and apply the updated rules before continuing.```

This often works, but for significant rule changes, starting a new chat is safer and more deterministic.

***

# **Demo Example**

Once the agents are installed in your project, you can walk through the full workflow using the **Star Wars Character Search** example below.

---

## **Phase 1 — Architecture & Requirements (PRD)**

**Open a new chat** with **Integration Designer Agent**. Choose the latest **Claude Opus** model and send the following prompt:

```
You are Integration Designer. Ignore previous planning and builder discussions.

Create a complete PRD for a new integration service.

Requirement:
The service must expose an API endpoint that searches Star Wars characters
by name (case-insensitive, partial match allowed).

Documentation:
https://swapi.info/documentation

From the matching character, return:
- Full name, height, eye color
- All film titles and release years where the character appeared
- A timestamp indicating when the search was executed
```

This will generate a PRD under `/docs/prd/`. **Review and approve** the PRD before continuing.

---

## **Phase 2 — Execution Planning (PLAN)**

**Open a new chat** with **Integration Planner Agent** (do not continue in the Designer chat). Choose the latest **Claude Opus** model and send the following prompt:

```
You are Integration Planner. Ignore previous designer discussions.

Read the approved PRD under /docs/prd/ for the Star Wars Character Search service.

Create a complete execution plan with 3–6 vertical TASKs.
Each TASK must be end-to-end testable and production-ready.

Generate TASK documents under /docs/tasks/.
```

**Review and approve** the generated plan and TASK definitions before continuing.

---

## **Phase 3 — TASK Implementation**

**Open a new chat** with **Integration Builder Agent** (do not continue in the Planner chat). Choose the latest **Claude Sonnet** model and send the following prompt for the first TASK:

```
You are Integration Builder. Ignore previous designer and planner discussions.

Implement TASK-01 exactly as defined in /docs/tasks/TASK-01/.

Follow all rules from copilot-instructions.md.
Run tests after implementation and report results.
```

> **Note:** If no SUBTASK files exist yet for the TASK, the Builder will first generate 5–12 SUBTASK definitions and **stop for your approval** before implementing. Review the SUBTASKs, then instruct the Builder to proceed.

After TASK-01 is complete and verified, continue with the next TASK:

```
You are Integration Builder. Ignore previous planning discussions.

Implement TASK-02 exactly as defined in /docs/tasks/TASK-02/.

Follow all rules from copilot-instructions.md.
Run tests after implementation and report results.
```

Repeat for each remaining TASK until the service is fully implemented.

---

## **Phase 4 — Change Handling & Re-planning**

If during implementation new requirements or issues arise, **open a new chat** with **Integration Designer Agent** (do not continue in the Builder chat). Choose the latest **Claude Opus** model and send the following prompt:

```
You are Integration Designer. Ignore previous planner and builder discussions.

Read the current PRD under /docs/prd/ for the Star Wars Character Search service.

Evaluate the following change request and update the PRD if needed:
- [describe the change or new requirement here]
```

After the PRD is updated, switch to **Integration Planner Agent** in a **new chat**:

```
You are Integration Planner. Ignore previous designer discussions.

The PRD under /docs/prd/ has been updated.

Review the changes, update the plan, and adjust or create new TASKs as needed under /docs/tasks/.
```

Then continue implementation with the **Integration Builder Agent** as in Phase 3.

