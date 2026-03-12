# **Agent Workflow Setup Guide**
Developed by **[Data Integration Mastery](https://dataintegrationmastery.com/)**
**Version 2.0** / *12th Mar 2026*

## Table of Contents

1. [Introduction](#introduction)
   - [PRD → PLAN → TASK Model](#prd--plan--task-model)
   - [Terminology](#terminology)
   - [Status Model](#status-model)
2. [Environment Requirements](#environment-requirements)
   - [Terminal Access Inside VS Code](#terminal-access-inside-vs-code)
   - [Build Toolchain Installed](#build-toolchain-installed)
   - [Git Installed and Configured](#git-installed-and-configured)
   - [GitHub Access and Repository Cloning](#github-access-and-repository-cloning)
3. [Environment Setup](#environment-setup)
   - [Minimum Folder Structure](#minimum-folder-structure)
   - [Purpose of the Folders](#purpose-of-the-folders)
4. [Control Files](#control-files)
   - [INSTRUCTIONS for Copilot](#instructions-for-copilot)
   - [Project Context](#project-context)
   - [Technology Stack Rules](#technology-stack-rules)
   - [PRD template](#prd-template)
   - [PLAN template](#plan-template)
   - [TASK template](#task-template)
   - [SUBTASK template](#subtask-template)
   - [TASK Folder Structure](#task-folder-structure)
   - [Status Model](#status-model-1)
   - [PR Template (OPTIONAL)](#pr-template-optional)
5. [Creating the Agents in VS Code](#creating-the-agents-in-vs-code)
   - [Integration Designer Agent](#integration-designer-agent)
   - [Integration Planner Agent](#integration-planner-agent)
   - [Integration Builder Agent](#integration-builder-agent)
   - [NOTE: If you are creating agents in the existing integration project](#note-if-you-are-creating-agents-in-the-existing-integration-project)
6. [OPTION 1: Initialize Project Context and Technology Stack from Environment](#option-1-initialize-project-context-and-technology-stack-from-environment)
   - [What It Does](#what-it-does)
   - [How to Use](#how-to-use)
   - [What Gets Populated](#what-gets-populated)
   - [Important](#important)
   - [Default Stack for New Integration Projects](#default-stack-for-new-integration-projects)
     - [Why This Stack?](#why-this-stack)
7. [OPTION 2: Enable Integration Architecture Rule System for Agents](#option-2-enable-integration-architecture-rule-system-for-agents)
   - [How to Enable](#how-to-enable)
   - [Affected Agents](#affected-agents)
   - [Important](#important-1)
8. [OPTION 3: Agent Delegation: Orchestrating Planner and Builder for End-to-End Implementation](#option-3-agent-delegation-orchestrating-planner-and-builder-for-end-to-end-implementation)
   - [Why Use Delegation?](#why-use-delegation)
   - [How to Enable](#how-to-enable-1)
   - [Required Agent Configuration Changes](#required-agent-configuration-changes)
9. [OPTION 4: Project Domain Expert Mode](#option-4-project-domain-expert-mode)
   - [What It Does](#what-it-does-domain-expert)
   - [How to Enable](#how-to-enable-domain-expert)
   - [Required Configuration](#required-configuration-domain-expert)
   - [Agent Instruction Modifications](#agent-instruction-modifications)
   - [Important](#important-domain-expert)


## **Introduction**

This document describes a practical and controlled method for using GitHub Copilot in integration development so that **the human acts as the architect and the AI as the implementer**.

The goal is not to build fully autonomous agents but to create a controlled, repeatable, and scalable working approach where:

*   Architecture and non‑functional requirements remain under human authority
*   The AI performs planning decomposition, code generation, and test iteration
*   Version control remains clean with a clear audit trail
*   Context is preserved even when development proceeds through several phases

Integration development is naturally complex: multiple systems, data contracts, messaging mechanisms, error handling, retry policies, performance requirements, and reliability constraints. Without structure, AI‑assisted development easily becomes non‑deterministic experimentation.

The three‑agent model described here – **Integration Designer**, **Integration Planner**, and **Integration Builder** – introduces:

*   Structure (artifacts before code)
*   Focus (separate roles)
*   Iterative quality assurance
*   Controlled productivity gains through AI

With this model, Copilot becomes not only an “automatic code completer” but a disciplined component of both the planning and implementation workflow. The agents will be "IDE-integrated co-pilots with agent behavior".


This document explains how to:

1.  **Prepare the environment, project, and repository** for AI‑guided development
2.  Create three separate agents in **VS Code**
3.  Manage agent roles and memory
4.  Implement integrations using a **TASK-based model**
5.  Maintain development discipline through version control

If integration development is your "thing" or central in your organization, this model provides a tangible way to combine **architectural discipline** with **AI‑driven productivity**—without shifting decision‑making away from humans.

### **PRD → PLAN → TASK Model**

This workflow is based on a guided **PRD → PLAN → TASK** approach.

The idea:

*   Before coding → create an explicit definition (PRD)
*   After that → build a phased plan (PLAN)
*   Then → deliver small end‑to‑end TASKs like one usecase at time.

In integration development this is essential, because integrations are **contracts**, **flows**, **error handling**, **reliability**, and **observability**, not just endpoints.

### **Terminology**

| Term | Definition |
|---|---|
| **PRD** | **P**roduct **R**equirements **D**ocument – defines what needs to be built. |
| **PLAN** | Execution plan derived from PRD – breaks work into **VERTICAL SLICE**s and **TASK**s. |
| **VERTICAL SLICE** | A thin but end-to-end implementation of one complete integration flow — from source system to target system — including routing, transformation, error handling, and monitoring. |
| **TASK** | One end-to-end **VERTICAL SLICE** that delivers a usable integration capability (e.g., an API endpoint, a message consumer, a data sync flow). Each TASK has its own folder under `/docs/agent-workflow/tasks/TASK-XX/`. |
| **SUBTASK** | A technical implementation step within a TASK. Builder generates 5–12 SUBTASKs per TASK. Each SUBTASK results in one commit. |

### **Status Model**

**TASK statuses:** TO-DO → IN-PROGRESS → IN-REVIEW → DONE → CANCELLED

**SUBTASK statuses:** TO-DO → IN-PROGRESS → DONE → CANCELLED

Only the **Human** can set a TASK to DONE.

## Environment Requirements

To work effectively with Copilot Agents, your development environment must also support the following:

### Terminal Access Inside VS Code

Builder agents use execution tools to:
	•	Run builds
	•	Execute tests
	•	Validate compilation

Ensure:
	•	Integrated terminal works
	•	Project builds successfully before starting agent orchestration


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

### Git Installed and Configured

Additionally you must have Git installed and configured locally.
Download: https://git-scm.com/

Verify installation:
```
git --version
```
You should also configure:
	•	user.name
	•	user.email

***

#### GitHub Access and Repository Cloning

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



# **Environment Setup**

Before creating agents, the project must be made “AI‑driven”.  
This is critical.

## **Minimum Folder Structure**

    /docs/agent-workflow
        /architecture
        /plans
        /prd
        /rules
            project-context.md
            technology-stack.md
        /tasks
            TASK-template.md
            SUBTASK-template.md
    .github

## **Purpose of the Folders**

**/docs/agent-workflow/architecture**
Architectural Decision Records.

**/docs/agent-workflow/prd**  
Product Requirements Documents (one per feature).

**/docs/agent-workflow/plans**  
Plans generated from PRDs.

**/docs/agent-workflow/tasks**  
TASK and SUBTASK definitions. Each TASK has its own folder: `/docs/agent-workflow/tasks/TASK-XX/`.  
Templates: `TASK-template.md` and `SUBTASK-template.md` at root level.

**/docs/agent-workflow/rules**  
Project-level control files: `project-context.md` and `technology-stack.md`.  
If OPTION 1 is enabled, also contains `integration-architecture-rule-system/`.

***


# **Control Files**

A core part of agent behavior is shaped by repository‑level instruction files.

## **INSTRUCTIONS for Copilot**

This is the **most important** file.  
It acts as the agent’s **permanent memory**.

Purpose:

*   Define Planner and Builder responsibilities
*   Enforce PRD → PLAN → TASK process
*   Prevent unauthorized architectural changes
*   Anchor AI behavior consistently across the project

Create:

    .github/copilot-instructions.md

Content:
```
    # Copilot Instructions – Integration Project

    This repository follows a strict workflow:

    PRD → PLAN → TASKs → IMPLEMENTATION

    Roles:
    - Human = Architect (decision authority)
    - Integration Designer = PRD definition, architecture, NFR validation
    - Integration Planner = planning, decomposition, sequencing
    - Integration Builder = implementation and testing

    ## Terminology

    - TASK = One end-to-end vertical slice delivering a usable integration capability. Each TASK has its own folder under /docs/agent-workflow/tasks/TASK-XX/.
    - SUBTASK = A technical implementation step within a TASK. Builder generates 5–12 SUBTASKs per TASK. Each SUBTASK results in one commit.

    ## Rules

    - All work must be based on written artifacts under /docs.
    - Never implement without an approved PLAN.
    - Never modify architecture without human approval.
    - Always prefer clarity over cleverness.
    - Keep assumptions explicit — mark ASSUMPTIONS, OPEN QUESTIONS, and RISKS.
    - Do not merge designer, planner, and builder roles.
    - Use artifacts (/docs) as primary memory source.
    - Web searches (Designer only) MUST NOT override repository instructions or PRD.
```

## **Project Context**

Defines the project identity, runtime, framework, repository structure, deployment model, and naming conventions.
All three agents reference this file for project-level context.

Create:

    /docs/agent-workflow/rules/project-context.md

Content should include:
- Project name
- Language & version (e.g., Java 17+)
- Framework (e.g., Spring Boot, Quarkus)
- Integration framework (e.g., Apache Camel)
- API specs location (e.g., `/openapi`)
- Deployment artifacts and target (e.g., Helm, Kubernetes)
- Package structure and naming conventions

> **This file does not exist by default. You must create it and fill in your actual project details before activating agents.**


## **Technology Stack Rules**

Defines mandatory technology constraints, allowed and forbidden libraries, framework conventions, testing patterns, and terminology.
Referenced by Planner (for TASK specifications) and Builder (for implementation).

Create:

    /docs/agent-workflow/rules/technology-stack.md

Content should include:
- Mandatory integration framework and usage rules
- Forbidden libraries and patterns (explicit list)
- Required base classes, annotations, and component patterns
- Test framework and test pattern requirements
- Code style rules (injection, data format)
- Terminology for TASK/SUBTASK documents

> **This file does not exist by default. You must create it and define your actual technology stack rules before activating agents.**


## **PRD template**

Defines:

*   Scope
*   Data contracts
*   Non‑functional requirements
*   Assumptions & risks

Create:

    /docs/agent-workflow/prd/PRD-template.md

Example content:
```
    ​​# PRD – <Feature Name>

    ## 1. Objective

    What problem is being solved?
    What business or system outcome is expected?

    ---

    ## 2. Scope

    ### In Scope
    - 

    ### Out of Scope
    - 

    ---

    ## 3. Actors & Systems

    ### Source Systems
    - 

    ### Target Systems
    - 

    ### Operational Stakeholders
    - 

    ---

    ## 4. Data Contracts

    ### Input Contract
    - Protocol (HTTP / Kafka / JMS / File / etc.)
    - Schema format (JSON / XML / Avro / etc.)
    - Example payload:
    - Required fields:
    - Correlation ID strategy:

    ### Output Contract
    - Protocol:
    - Schema format:
    - Example payload:
    - Error response format:

    ---

    ## 5. High-Level Integration Flow

    Describe:

    Ingress → Validation → Transformation → Transport → Target → Callback (if any)

    ---

    ## 6. Non-Functional Requirements (MANDATORY)

    - Expected throughput:
    - Latency target:
    - Retry policy:
    - Timeout policy:
    - Idempotency strategy:
    - Dead-letter handling:
    - Observability requirements (logs, metrics, traces):
    - Security (authN, authZ, secrets, encryption):
    - Compliance constraints (PII, audit):

    ---

    ## 7. Acceptance Criteria

    - End-to-end happy path works
    - Validation errors handled
    - Target failure handled
    - Retry behavior verified
    - Tests exist

    ---

    ## 8. Assumptions

    -

    ---

    ## 9. Open Questions

    -

    ---

    ## 10. Risks

    -

```

## **PLAN template**

Purpose:

*   Convert PRD → phased plan
*   Define architecture summary
*   Break into TASKs

Create:

    /docs/agent-workflow/plans/PLAN-template.md

Example content:
```
    # PLAN – <Feature Name>

    Based on PRD: <link or file reference>

    ---

    ## 1. Architecture Overview

    Summarize architecture decisions derived from PRD.

    - Entry point:
    - Transport mechanism:
    - Processing model:
    - Error handling strategy:

    ---

    ## 2. Implementation Phases (Macro Steps)

    1. Architecture & Wiring
    2. Happy Path Implementation
    3. Error Handling & Resilience
    4. Testing
    5. Deployment & Observability

    ---

    ## 3. TASKs (Execution Units)

    ### TASK-01
    Short description.

    ### TASK-02
    Short description.

    ### TASK-03
    Short description.

    (3–6 TASKs recommended)

    ---

    ## 4. Dependencies

    - External services:
    - Schemas:
    - Infrastructure components:

    ---

    ## 5. Execution Order

    List TASKs in recommended order and justify briefly.

    ---

    ## 6. Risks & Mitigation

    -

    ---

    ## 7. Human Approval Required

    - Architecture approved? [ ]
    - Non-functionals confirmed? [ ]
    - TASK order confirmed? [ ]
```

## **TASK template**

Purpose:

*   Define a single end‑to‑end deliverable (TASK)
*   Describe scope, flow, acceptance criteria
*   Reference SUBTASKs for implementation details

Each TASK gets its own folder: `/docs/agent-workflow/tasks/TASK-XX/`

Reusable template:

    /docs/agent-workflow/tasks/TASK-template.md

Create per TASK:

    /docs/agent-workflow/tasks/TASK-XX/TASK-XX.md

Example content:
```
    # TASK-XX – <Name>

    ## Status
    TO-DO | IN-PROGRESS | IN-REVIEW | DONE | CANCELLED

    ## Description

    What end-to-end capability is delivered?

    ---

    ## End-to-End Flow

    Describe:

    Ingress → Route → Transport → Target → Response

    ---

    ## Acceptance Criteria

    - Build succeeds
    - Tests pass
    - Behavior matches PRD
    - Error handling implemented
    - Logging/observability in place

    ---

    ## SUBTASKs

    See SUBTASK files in this folder.
```

## **SUBTASK template**

Purpose:

*   Define a single technical implementation step within a TASK
*   Small enough to implement and commit atomically

Reusable template:

    /docs/agent-workflow/tasks/SUBTASK-template.md

Create per SUBTASK:

    /docs/agent-workflow/tasks/TASK-XX/SUBTASK-XX.md

Example content:
```
    # SUBTASK-XX – <Name>

    **Parent TASK:** TASK-XX

    ## Status
    TO-DO | IN-PROGRESS | DONE | CANCELLED

    ## Description

    What specific technical step is performed?

    ---

    ## Definition of Done

    - Implementation complete
    - Tests pass
    - No compilation errors
```

## **TASK Folder Structure**

Each TASK is organized as a folder containing the TASK definition and its SUBTASKs:

```
    /docs/agent-workflow/tasks/
        TASK-01/
            TASK-01.md
            SUBTASK-01.md
            SUBTASK-02.md
            SUBTASK-03.md
        TASK-02/
            TASK-02.md
            SUBTASK-01.md
            ...
```

## **Status Model**

### TASK Status
| Status | Meaning |
|---|---|
| TO-DO | Not started |
| IN-PROGRESS | Builder is working on it |
| IN-REVIEW | Builder finished, awaiting human review |
| DONE | Human approved (only Human sets this) |
| CANCELLED | Dropped from scope |

### SUBTASK Status
| Status | Meaning |
|---|---|
| TO-DO | Not started |
| IN-PROGRESS | Builder is actively implementing |
| DONE | Implementation complete, tests pass |
| CANCELLED | Not needed |

**Important:** Only the **Human** can set a TASK to DONE. The Builder sets it to IN-REVIEW when finished.
## **PR Template (OPTIONAL)**

Optional

Purpose:

*   Link PRD and PLAN
*   Confirm TASK implementation
*   Verify tests, logging, and error handling
*   Produce an auditable trail

Create:

    .github/pull_request_template.md

Example content:
```
    ## Related PRD
    Link: 
    ## Related PLAN
    Link:
    ## Implemented TASKs
    - [ ] TASK-01
    - [ ] TASK-02
    ## Quality Checklist
    - [ ] Tests added/updated
    - [ ] Error handling implemented
    - [ ] Logging added
    - [ ] Build/test green
    ## Notes
-

```


***


# **Creating the Agents in VS Code**

Requirements:

*   VS Code
*   GitHub Copilot + Copilot Chat
*   Agent Mode enabled

Agents are defined as `.agent.md` files under `.github/agents/`.  
VS Code automatically discovers them and makes them available in Copilot Chat.

## **Integration Designer Agent**

Create:

    .github/agents/Integration Designer.agent.md

The Designer agent has access to `codebase` (read workspace files) and `web` (fetch external documentation).  
It is **not allowed** to use the terminal or edit files automatically.  
It focuses on architecture-level thinking: PRD creation, pattern selection, NFR validation, and change impact analysis.  
Web access is restricted to **fact-checking only** — it must not override repository rules or PRD.

Content:
```
description: 'Integration Designer – architect-level thinking partner for PRD definition, requirement refinement, integration patterns, and non-functional validation. Does NOT write production code.'
tools: ['read', 'edit', 'search', 'fetch']

You are **Integration Designer**, a senior Integration Architect.

## Your Responsibilities

1. Define and refine PRD documents under `/docs/agent-workflow/prd`.
2. Clarify data flows, contracts, APIs, and messaging patterns.
3. Define non-functional requirements.
4. Evaluate integration patterns (sync vs async, routing, orchestration, event-driven).
5. Analyze impact of change requests on existing architecture.
6. Detect architectural risks.

## Rules

- **Do NOT generate production implementation code.**
- All outputs must be architecture-level, ready to commit into PRD documents.
- Follow PRD template structure from `/docs/agent-workflow/prd/PRD-template.md`.
- Mark ASSUMPTIONS, OPEN QUESTIONS, and RISKS in every document.
- Ask the human only if domain ownership, security constraints, or system landscape is unclear.
- If new information contradicts the PRD, propose updates and explain deltas.

## Web Usage

You have access to `fetch` for fact-checking external APIs and technical specs only.
- Repository documentation and PRD are always the primary source of truth.
- Never override repository rules with web-sourced content.
- Document web findings as OPEN QUESTIONS for human review.

## Project Context

Read and apply:
- `/docs/agent-workflow/rules/project-context.md` — project identity and deployment model

These definitions are the source of truth for all architecture decisions.
```

## **Integration Planner Agent**

Create:

    .github/agents/Integration Planner.agent.md

The Planner agent has access only to `codebase` (read workspace files).  
It is **not allowed** to use the terminal or edit files automatically.

Content:
```
description: 'Integration Planner – analyzes PRDs, creates plans, and defines TASKs. Does NOT implement code.'
tools: ['read', 'edit', 'search']

You are **Integration Planner**.

You operate after the Integration Designer has finalized the PRD.
Treat PRD as the architectural source of truth. Do not redefine architecture.

## Your Responsibilities

1. Analyze PRD documents under `/docs/agent-workflow/prd`.
2. Analyze repository structure before proposing changes.
3. Create PLAN documents under `/docs/agent-workflow/plans`.
4. Break work into TASKs under `/docs/agent-workflow/tasks`.
5. Propose execution order and justify it.

## Rules

- **Do not implement code.**
- **Do not redefine architecture.** If PRD is unclear or incomplete, request clarification instead of making architectural decisions.
- Follow PLAN and TASK template structures from `/docs/agent-workflow/`.
- Mark ASSUMPTIONS, OPEN QUESTIONS, and RISKS in every document.
- Ask human only if NFRs are missing, security constraints are unclear, or domain ownership is unclear.
- If new information contradicts the plan, update the PLAN and explain deltas.

## TASK Planning Rules

- Each TASK must deliver one **end-to-end vertical capability** (not horizontal concerns).
- Create a folder per TASK: `/docs/agent-workflow/tasks/TASK-XX/`
- Initialize all TASK statuses as `TO-DO`.
- Do NOT create SUBTASK files — the Builder generates those.

## Project Context & Technology Stack

Read and apply:
- `/docs/agent-workflow/rules/project-context.md` — project identity and deployment model
- `/docs/agent-workflow/rules/technology-stack.md` — framework conventions and terminology
```

## **Integration Builder Agent**

Create:

    .github/agents/Integration Builder.agent.md

The Builder agent has access to `codebase`, `terminal`, and `editor`.  
It **can** edit files and run commands.

Content:
```
description: 'Integration Builder – implements exactly one approved TASK at a time. Does NOT plan or modify architecture.'
tools: ['read', 'edit', 'search', 'execute']

You are **Integration Builder**.

You implement based strictly on TASK definitions and PRD.
If you detect inconsistencies or unclear requirements, pause and request clarification.

## Your Responsibilities

1. Implement exactly **ONE** approved TASK at a time.
2. Follow the TASK specification under `/docs/agent-workflow/tasks/TASK-XX/`.
3. Add: implementation, tests, logging/observability, error handling.
4. Run build/tests and fix until green.
5. Summarize: changed files, test results, open issues.

## Rules

- **Never change architecture or modify the PLAN.**
- Never implement without an approved PLAN and TASK definition.

## TASK Implementation Workflow

1. Read TASK definition from `/docs/agent-workflow/tasks/TASK-XX/TASK-XX.md`.
2. Set TASK status to `IN-PROGRESS`.
3. If **no SUBTASKs exist**: generate 5–12 SUBTASK files and **STOP for human approval**.
4. Implement each SUBTASK in order:
   - Set SUBTASK status to `IN-PROGRESS`
   - Implement, run build/tests
   - Set SUBTASK status to `DONE`
   - Commit: `TASK-XX SUBTASK-0n: <short description>`
5. After all SUBTASKs done, set TASK to `IN-REVIEW`.
6. **Never set TASK to DONE** — only the Human does this.

## Quality Gates

Before reporting TASK as done: happy path works, validation errors handled, retry/timeout applied (if relevant), tests pass, no secrets hardcoded, no PII logged, build succeeds, behavior matches PRD.

## Project Context & Technology Stack Rules

Read and apply:
- `/docs/agent-workflow/rules/project-context.md` — project identity and deployment model
- `/docs/agent-workflow/rules/technology-stack.md` — mandatory technology stack rules

Do NOT use libraries, patterns, or frameworks not defined in these files.
```

***
## **NOTE: If you are creating agents in the existing integration project**

When adding the three-agent model to a project that already has an existing
codebase, the setup differs from a greenfield project. The agents need to
understand what already exists before they can plan or build effectively.

### Step 1 — Audit the existing structure

Before creating any agent files, map what is already in place:

- Does a `/docs` folder exist? If not, create it with the full folder structure
  (`/prd`, `/plans`, `/tasks`, `/arc`).
- Does `.github/copilot-instructions.md` exist? If yes, review and extend it —
  do not replace it blindly.
- Are there existing routes, endpoints, or integration flows? List them. These
  become the baseline context for the Designer agent.

### Step 2 — Document existing architecture before using agents

Do not start with the Planner or Builder. Start with the Designer.

Ask the Designer to produce a **baseline PRD** that captures the existing
integration landscape:

```
Act as Integration Designer.
Read the existing codebase under /src.
Read the also existing README.md (or equivalent) from the root but remember the thruth is in the sourse code (/src)
Produce a baseline PRD under /docs/agent-workflow/prd/PRD-baseline.md
that documents the current integration flows, data contracts,
and non-functional characteristics as they exist today.
Mark unclear areas as OPEN QUESTIONS.
```

This gives the agents a written source of truth for the existing system —
without it, planning and building will produce inconsistent results.

### Step 3 — Retrofit the folder structure

Create the missing artifact folders and add a `PRD-baseline.md` before creating
any PLAN or TASK documents. Existing code does not need to be restructured —
the `/docs` layer is additive.

Minimum additions to an existing project:

```
.github
  copilot-instructions.md ← extend if already exists
  /agents
    Integration Designer.agent.md
    Integration Planner.agent.md
    Integration Builder.agent.md
/docs/agent-workflow
  /prd
    PRD-baseline.md       ← documents existing system
  /plans                  ← empty, ready for first plan
  /tasks                  ← empty, ready for first tasks
  /architecture            ← empty, for future decisions
```

### Step 4 — Define the first TASK as a new capability, not a refactor

Resist the temptation to start by refactoring existing code with the Builder.
The first TASK in an existing project should add a **new, bounded capability**
so that the agent workflow is validated without disturbing production logic.

Refactoring tasks can follow once the team trusts the agent workflow.

### Step 5 — Align copilot-instructions.md to the existing stack

If the project uses a specific framework, naming convention, or deployment model
that differs from the template defaults, update `.github/copilot-instructions.md`
before activating agents. The Builder will follow whatever is written there —
outdated or generic instructions produce inconsistent code.

Key things to verify and update:

- Technology stack (framework versions, build tool)
- Existing naming conventions for routes and processors
- Package structure
- Test framework in use
- Deployment target (ECS, EC2, Kubernetes, etc.)

***

# **OPTION 1:** Initialize Project Context and Technology Stack from Environment

The control files `project-context.md` and `technology-stack.md` define the project identity and technology constraints that all three agents depend on. Instead of filling these files manually, you can ask **GitHub Copilot Chat** to populate them automatically based on project files and the local development environment.

## What It Does

Copilot reads your project's build configuration (e.g., `pom.xml` or `build.gradle`), source code structure, and local toolchain versions, then fills in the two control files with concrete values derived from the actual environment.

## How to Use

Open **GitHub Copilot Chat** in VS Code (not an agent chat — a regular Copilot Chat session) and send the following prompt:

```
Read the project build file (pom.xml or build.gradle), the source code structure
under /src, and check the local Java and build tool versions from the terminal.

Based on what you find, fill in the following control files with actual values:
- /docs/agent-workflow/rules/project-context.md
- /docs/agent-workflow/rules/technology-stack.md

Use the existing section headings in each file. Replace placeholder comments
with concrete values. If a value cannot be determined from the project files
or environment, write "Not yet defined" with a short explanation.
```

Review the generated content and adjust any values that need correction.

## What Gets Populated

### project-context.md

| Section | Source |
|---------|--------|
| Project Name | `<name>` or `<artifactId>` from build file |
| Language & Version | `<java.version>` property from build file |
| Framework | Parent POM or dependency (e.g., Spring Boot version) |
| Build Tool | Detected from `pom.xml` (Maven) or `build.gradle` (Gradle) |
| Integration Framework | Integration dependencies in build file, or "Not yet defined" |
| API Specs Location | Presence of `/openapi` or similar folder, or "Not yet defined" |
| Deployment Artifacts and Target | Build plugins and packaging type, or "Not yet defined" |
| Package Structure | Base package from main application class |

### technology-stack.md

| Section | Source |
|---------|--------|
| Mandatory Framework Rules | Framework and version from build file |
| Forbidden Libraries | Inferred from framework version (e.g., `jakarta.*` vs `javax.*`) |
| Required Annotations | Framework conventions (e.g., `@SpringBootApplication`, `@RestController`) |
| Test Framework | Test dependencies from build file (e.g., JUnit 5, Mockito) |
| Code Style Rules | Standard conventions for the detected framework |
| Terminology | Default integration terminology |

## Important

- Always **review the generated content** before activating agents — Copilot infers values but may not capture project-specific conventions.
- If the project has no integration framework configured yet, the field will be set to "Not yet defined" — this is expected for new projects.
- You can re-run this step at any time if the project setup changes (e.g., new dependencies added).
- This step is **optional** — you can always fill in the control files manually.


## Default Stack for New Integration Projects

If the environment has **no integration framework defined**, instruct Copilot to use the following **recommended open-source integration stack** as defaults:

```
If the project has no integration framework configured yet, use the following
defaults as the recommended open-source integration stack:

Integration Framework: Apache Camel (latest stable version)
Runtime Framework: Quarkus (latest stable version)
Build Tool: Maven
Language: Java 21+

Additional recommended components:
- REST Client: Quarkus REST Client (MicroProfile)
- Messaging: Apache Kafka or ActiveMQ Artemis
- Observability: OpenTelemetry + Micrometer
- Health & Metrics: SmallRye Health, SmallRye Metrics
- Configuration: MicroProfile Config
- Testing: JUnit 5, Camel Test, REST Assured, Testcontainers
- API Documentation: SmallRye OpenAPI
- JSON Processing: Jackson
- Containerization: Jib or Docker multi-stage builds

Apply these defaults to both project-context.md and technology-stack.md.
```

### Why This Stack?

| Component | Rationale |
|-----------|-----------|
| **Apache Camel** | Industry-standard integration framework with 300+ connectors, enterprise integration patterns, and excellent testability |
| **Quarkus** | Cloud-native runtime with fast startup, low memory footprint, native compilation support, and excellent developer experience |
| **OpenTelemetry** | Vendor-neutral observability standard for distributed tracing, metrics, and logging |
| **Testcontainers** | Enables realistic integration testing with real databases, message brokers, and external services |
| **MicroProfile** | Standardized APIs for cloud-native Java (config, health, metrics, REST client) |

This stack provides a production-ready foundation for enterprise integration development with strong community support and no vendor lock-in.

***

# **OPTION 2:** Enable Integration Architecture Rule System for Agents

The **Integration Architecture Rule System** provides a deterministic, rule-based framework that guides the **Designer** and **Planner** agents to make structured, traceable architecture decisions instead of relying on general AI knowledge.

When enabled, these agents will:

- Apply explicit design principles, decision rules, and pattern selection rules
- Cite rule identifiers (e.g., `P01`, `D01`, `NFR02`) in every architecture decision
- Run validation checklists before finalizing PRD documents
- Trigger human review when rule conflicts cannot be resolved

## How to Enable

Follow the installation guide in:

**[Integration Architecture Rule System — Installation Guide](../rules/integration-architecture-rule-system/README.md#7-installation-guide--github-copilot-agent-instruction-file)**

The guide describes:

1. **Prerequisites** — required file structure under `/docs/agent-workflow/rules/`
2. **Target agent** — which agent to configure (Designer, and optionally Planner)
3. **Section to add** — the exact Markdown block to insert into the agent's `.agent.md` file
4. **Loading strategy** — how the agent should load rule files on demand
5. **Verification** — how to confirm the agent is using the rule system correctly

## Affected Agents

| Agent | Impact |
|-------|--------|
| **Integration Designer** | Primary target. Applies rules when creating and validating PRDs. |
| **Integration Planner** | Optional. Can reference rules when evaluating TASK feasibility and NFR compliance. |
| **Integration Builder** | Not affected. Builder focuses on implementation, not architecture decisions. |

## Important

- The rule system files must exist under `/docs/agent-workflow/rules/integration-architecture-rule-system/` in your repository
- Do NOT copy the rule content into the agent file — the agent loads rule files on demand from the repository
- After modifying agent `.agent.md` files, start a **new chat** to ensure the agent picks up the changes

***

# **OPTION 3:** Agent Delegation: Orchestrating Planner and Builder for End-to-End Implementation

It is technically possible to let the **Planner orchestrate full implementation together with the Builder** by using **agent delegation**. In this model, once the PRD is finalized, the Planner converts it into structured TASKs and delegates each TASK to the Builder for implementation, testing, and committing.

## Why Use Delegation?

**Benefits**
- Clear separation of responsibilities (architecture → planning → implementation)
- Faster end-to-end delivery
- Consistent TASK-based commits
- Better control over scope and quality gates
- Reduced manual coordination

**Risks**
- Role drift (Planner starts implementing code)
- Architectural decisions being redefined during implementation
- Context overload if delegation is too large in scope
- Reduced transparency if TASKs are not clearly defined

Delegation works best when implementation is performed TASK-by-TASK rather than in one large task.


## How to Enable

### Step 1 — Update Integration Planner frontmatter

In `.github/agents/Integration Planner.agent.md`, update the `tools:` line in the frontmatter.

Change:
```
tools: ['read', 'edit', 'search']
```
To:
```
tools: ['read', 'edit', 'search', 'agent']
```

### Step 2 — Add delegation instructions to Integration Planner

Add the following section to `.github/agents/Integration Planner.agent.md`, before `## Project Context & Technology Stack Rules`:

```markdown
## TASK Delegation to Integration Builder

When delegation is enabled:

1. Create and document all TASKs under `/docs/agent-workflow/tasks/` as normal.
2. Present the full TASK list to the human for approval before delegating.
3. After approval, delegate each TASK to **Integration Builder** one at a time:
   - Reference the TASK file: `/docs/agent-workflow/tasks/TASK-XX/TASK-XX.md`
   - Instruct Builder to implement exactly one TASK
   - Wait for Builder to complete and report before delegating the next TASK
4. After each TASK, verify:
   - TASK status is set to `IN-REVIEW`
   - Builder reported test results and changed files
   - No unresolved architectural issues
5. Report overall progress to the human after each TASK cycle.

**CRITICAL:**
- Do NOT delegate multiple TASKs simultaneously.
- Do NOT implement code yourself — delegation only.
- If Builder detects an architectural issue, stop and escalate to the human before continuing.
```

### Step 3 — Verify Builder capability

No changes are required to the **Integration Builder** agent. It already implements TASKs correctly via its `edit` and `execute` capabilities.

Confirm that the Builder frontmatter includes:
```
tools: ['read', 'edit', 'search', 'execute']
```

### Important

- After modifying agent `.agent.md` files, start a **new chat** to ensure the agents pick up the changes
- Delegation works best for well-defined, isolated TASKs — avoid delegating TASKs with unclear acceptance criteria
- The human must still review each completed TASK and set TASK status to `DONE`


## Required Agent Configuration Changes

To enable safe delegation, the three agents must be configured as follows:

### Integration Designer
- Focus strictly on PRD and architectural decisions
- Must NOT generate production implementation code
- Remains the architectural source of truth

### Planner
- Must have access to the `agent delegate` tool
- Should NOT have full execution privileges
- Must be explicitly instructed **not to implement code directly**
- Responsible for:
  - Creating TASKs
  - Delegating implementation
  - Verifying completion criteria

### Builder
- Must retain `edit` and `execute` capabilities
- Is the only agent allowed to modify production code
- Responsible for:
  - Implementing TASKs
  - Running tests
  - Fixing failures
  - Committing changes

***

# **OPTION 4:** Project Domain Expert Mode

Project Domain Expert Mode transforms the **Integration Designer**, **Integration Planner**, and **Integration Builder** agents into specialized experts for a specific project domain. When enabled, agents prioritize project-specific documentation stored in a dedicated folder and apply domain context to all architectural decisions, planning, and implementation work.

This is useful when you have project-specific guidelines, conventions, architectural patterns, development playbooks, or other domain knowledge that should be referenced before and alongside the standard rules system.

## What It Does

When Domain Expert Mode is enabled:

1. **Agents prioritize project documentation** from `docs/[project-name]/` over generic rules and templates
2. **All project-specific guidelines** (architecture patterns, technical decisions, development practices, naming conventions, etc.) are loaded and applied first
3. **Agents update their knowledge** from this folder consistently throughout development
4. **Scope conflicts are resolved predictably** — project-specific guidance takes precedence over `docs/rules/` guidance in ambiguous situations
5. **The project name is parameterized** in setup prompts to ensure consistency between agent configurations and documentation location

## How to Enable

### Step 1 — Organize Project Documentation

Create a folder under `/docs/[project-name]/` containing all project-specific guidance and documentation. The folder name must be **exactly identical** to the project name identifier you will use in agent prompts.

For example, if your project is called **imdb**:

```
/docs/imdb/
    development-playbook.md      ← project development practices, conventions and design decisions
    environment.md               ← technical environment architecture 
    concept-models.md            ← projects models
    ... (any other project-specific docs)
```

**Important:** All project-specific documentation should be in this single folder (and its subfolders). This becomes the single source of truth for domain expertise.

### Step 2 — Identify the Project Name

Choose a **project identifier** that will be used consistently:
- In the `docs/[project-name]/` folder name
- In agent prompts and system instructions
- In the setup and update prompts

For the imdb example, the project identifier is: **imdb**

### Step 3 — Add Domain Expert Section to Agent Instruction Files

For each agent (Integration Designer, Integration Planner, Integration Builder), add a new section to its `.agent.md` file that instructs it to load and prioritize project-specific documentation.

#### Addition to `.github/agents/Integration Designer.agent.md`:

Add this section after the "Web Usage Rules" section and before the closing backticks:

```markdown
## Project Domain Expertise – [project-name]

This project has dedicated domain expertise documentation under `/docs/[project-name]/`.

**Priority loading** (in order):
1. Project-specific documentation under `/docs/[project-name]/`
2. Rules system under `/docs/agent-workflow/rules/integration-architecture-rule-system/` (if enabled)
3. Standard integration patterns and PRD templates

Before creating or updating any PRD:
1. Read all files in `/docs/[project-name]/` (including subfolders)
2. Extract project-specific patterns, conventions, constraints, and architectural decisions
3. Apply these patterns consistently in the PRD
4. When PRD content could conflict with project guidance, prioritize project guidance
5. Mark any uncertainties as OPEN QUESTIONS for human review

Update your understanding of project patterns whenever you access domain documentation. Keep project-specific context active throughout all architecture discussions.
```

#### Addition to `.github/agents/Integration Planner.agent.md`:

Add this section after the "Project Context & Technology Stack Rules" section and before the closing backticks:

```markdown
## Project Domain Expertise – [project-name]

This project has dedicated domain expertise documentation under `/docs/[project-name]/`.

**Priority loading** (in order):
1. Project-specific documentation under `/docs/[project-name]/`
2. Rules system under `/docs/agent-workflow/rules/integration-architecture-rule-system/` (if enabled)
3. Standard planning templates and TASK models

Before creating any PLAN or TASK:
1. Read all files in `/docs/[project-name]/` (including subfolders)
2. Extract project-specific patterns, conventions, architectural decisions, and naming conventions
3. Apply these patterns to TASK definitions and sequencing
4. Orient SUBTASK breakdowns according to project practices
5. Mark any uncertainties as OPEN QUESTIONS for human review

Maintain project context throughout planning. Reference project patterns explicitly in PLAN and TASK documents.
```

#### Addition to `.github/agents/Integration Builder.agent.md`:

Add this section after the "Project Context & Technology Stack Rules" section and before the closing backticks:

```markdown
## Project Domain Expertise – [project-name]

This project has dedicated domain expertise documentation under `/docs/[project-name]/`.

**Priority loading** (in order):
1. Project-specific documentation under `/docs/[project-name]/`
2. Technology Stack Rules under `/docs/agent-workflow/rules/technology-stack.md`
3. Standard implementation templates and patterns

Before implementing any SUBTASK:
1. Read all files in `/docs/[project-name]/` (including subfolders)
2. Extract project-specific code patterns, naming conventions, architectural decisions, and implementation practices
3. Apply these patterns to code generation, test creation, and error handling
4. Follow project code style and component organization
5. Use project-specific terminology and abstractions

Maintain project context throughout implementation. Reference project patterns explicitly in code comments and commit messages.
```

### Step 4 — Replace Placeholder `[project-name]`

In each agent `.agent.md` file, replace the placeholder **`[project-name]`** with your actual **project identifier**.

Example for project **imdb**:

**Before:**
```
## Project Domain Expertise – [project-name]
This project has dedicated domain expertise documentation under `/docs/[project-name]/`.
```

**After:**
```
## Project Domain Expertise – imdb
This project has dedicated domain expertise documentation under `/docs/imdb/`.
```

Make this replacement consistently throughout all three agent files.

### Step 5 — Verify Configuration

1. Confirm that `/docs/[project-name]/` folder exists and contains your project documentation
2. Confirm that all three agent files have been updated with the correct project identifier
3. Close any open chat sessions (especially with the agents)
4. Start a **new chat** with one of the agents and verify it references the project-specific documentation

Test prompt for verification:

```
Confirm that you are now operating in Project Domain Expert Mode for [project-name].
List the project-specific documentation files you loaded from `/docs/[project-name]/`.
```

### Step 6 — Rename Agents with Project Identifier

To make the domain expertise role visually distinct in Copilot Chat, rename the three agent files to include the project identifier. This transforms generic agents into project-specific domain experts.

**Rename the agent files:**

```
BEFORE (generic):
.github/agents/Integration Designer.agent.md
.github/agents/Integration Planner.agent.md
.github/agents/Integration Builder.agent.md

AFTER (domain experts for project "imdb"):
.github/agents/[project-name] Integration Designer.agent.md
.github/agents/[project-name] Integration Planner.agent.md
.github/agents/[project-name] Integration Builder.agent.md

EXAMPLE (for project "imdb"):
.github/agents/Imdb Integration Designer.agent.md
.github/agents/Imdb Integration Planner.agent.md
.github/agents/Imdb Integration Builder.agent.md
```

**Benefits of renaming:**
- Agents appear as "Imdb Integration Designer" in Copilot Chat (not generic "Integration Designer")
- Visually distinct from any generic Integration agents you may use elsewhere
- Reinforces that these agents understand project-specific patterns and context
- Makes it clear in chat history which project's agents are being used

**After renaming:**
1. Commit the renamed files to version control
2. Close all open chat sessions
3. Start a **new chat** and verify the agents appear with the project-specific names
4. Confirm that agents still load project documentation correctly

## Required Configuration

### Folder Structure

```
/docs
    /[project-name]              ← replace with actual project identifier
        development-playbook.md
        technical-description.md
        development-process.md
        ... (any other project-specific docs)
    /rules
        project-context.md
        technology-stack.md
        /integration-architecture-rule-system/ (if OPTION 2 enabled)
    /prd
    /plans
    /tasks
```

### Naming Convention

The **project identifier** must be **exactly identical** in three places:
1. Folder name: `/docs/[project-name]/`
2. Agent instruction text: `## Project Domain Expertise – [project-name]`
3. Agent setup prompts: references to `/docs/[project-name]/`

Keep this naming consistent to ensure agents reliably find and load the correct documentation.

## Agent Instruction Modifications

When enabling Domain Expert Mode, all three agents require modifications to their `.agent.md` files:

### Designer Agent
- Loads project documentation first when creating PRDs
- Validates PRD decisions against project patterns
- Marks deviations from project guidance as explicit assumptions

### Planner Agent
- Incorporates project-specific patterns in PLAN and TASK structures
- Sequences TASKs according to project dependencies and conventions
- Ensures naming and organization match project standards

### Builder Agent
- Implements code according to project-specific patterns and conventions
- Applies project naming and organization standards
- References project documentation in comments and commit messages

## Important

- **Project documentation must exist before enabling agents** — create `/docs/[project-name]/` and populate it before adding the Domain Expertise section to agent files
- **Project identifier must match exactly** — inconsistent naming will prevent agents from locating documentation
- **Start a new chat after configuration** — agents will not pick up changes without a fresh chat session
- **Project documentation takes precedence** — in conflicts between project guidance and standard rules, project guidance wins (by design)
- **Maintain documentation currency** — update project documentation whenever architectural patterns or conventions change, and agents will apply updated patterns on next access
- **This option can be combined with OPTION 2** — both the rule system and domain expertise can be enabled simultaneously (rule system loads after project docs in priority order)






