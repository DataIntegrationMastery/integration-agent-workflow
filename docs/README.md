# Integration Agent Workflow
Developed by **[Data Integration Mastery](https://dataintegrationmastery.com/)**
**Version 1.15** / *10th Mar 2026*




## Table of Contents

1. [Purpose](#purpose)
2. [Installing VS Code and GitHub Copilot](#installing-vs-code-and-github-copilot)
   - [Install Visual Studio Code](#install-visual-studio-code)
   - [Install GitHub Copilot Extension](#install-github-copilot-extension)
   - [Licensing and Pricing](#licensing-and-pricing)
3. [Installing Integration Development Agents to Your Project](#installing-integration-development-agents-to-your-project)
   - [Step 1 — Getting this Repository as a Template to Your Project](#step-1--getting-this-repository-as-a-template-to-your-project)
   - [Step 2 — Bootstrap the Environment from the Design Document](#step-2--bootstrap-the-environment-from-the-design-document)
   - [Step 3 — Review the Changes](#step-3--review-the-changes)
   - [Step 4 — Validate Agent Configuration](#step-4--validate-agent-configuration)
4. [Using the Agents](#using-the-agents)
   - [Choosing the Language Models for the Agents](#choosing-the-language-models-for-the-agents)
   - [Handling the Context](#handling-the-context)
   - [Workflow Phases](#workflow-phases)
     - [Phase 1 — Architecture & Requirements (PRD)](#phase-1--architecture--requirements-prd)
     - [Phase 2 — Execution Planning (PLAN)](#phase-2--execution-planning-plan)
     - [Phase 3 — TASK Implementation](#phase-3--task-implementation)
     - [Phase 4 — Demo & Validation](#phase-4--demo--validation)
     - [Phase 5 — Change Handling & Re-planning](#phase-5--change-handling--re-planning)
   - [When Control Files Must Be Changed](#when-control-files-must-be-changed)
5. [Demo Example](#demo-example)
   - [Phase 1 — Architecture & Requirements (PRD)](#phase-1--architecture--requirements-prd-1)
   - [Phase 2 — Execution Planning (PLAN)](#phase-2--execution-planning-plan-1)
   - [Phase 3 — TASK Implementation](#phase-3--task-implementation-1)
   - [Phase 4 — Change Handling & Re-planning](#phase-4--change-handling--re-planning-1)
   - [Phase 5 — Demo & Validation](#phase-5--demo--validation-1)
6. [Removing Agent Workflow from a Project](#removing-agent-workflow-from-a-project)
   - [Removal Prompt](#removal-prompt)
   - [Important](#important)



# Purpose

GenAI has transformed how we write code. Autonomous agents can plan, break work into smaller pieces, decide sequentially when to fetch data, make calculations, or ask for help — and they adapt when new information arrives.

But here's the problem: **Integration development cannot be fully delegated to an autonomous AI loop.**

Integration work is a **contract between systems**. Every decision — retry policy, error handling, idempotency, observability, resilience — carries architectural consequences that you must understand, defend, and maintain. A single autonomous agent combining architecture, planning, and implementation in one loop gives you code. It does not give you solutions you can reason about.

## What This Template Offers

This template repository gives you a **three-agent orchestration model** where:

- **Architecture is a human responsibility.** AI accelerates, it does not decide.
- **Each phase requires explicit approval** before the next begins.
- **The audit trail is structural**, not accidental.
- **Strict role separation** prevents drift and confusion.

You get integration solutions you can **reason about, defend, and maintain**. Orchestrated AI. Bounded authority. Architectural discipline.

## How It Works

**The workflow is PRD → PLAN → TASK:**

- **PRD (Product Requirements Document)** — Human + Designer Agent create explicit architectural definition
- **PLAN** — Planner Agent breaks work into vertical delivery slices
- **TASK** — Builder Agent implements each end-to-end unit

**Each agent has defined capabilities:**
- **Designs and plans** — breaking complex work into manageable pieces
- **Uses tools independently** — fetching data, writing code, seeking guidance when needed
- **Adapts** — revising strategy when new information emerges

**But each agent is bounded.** No single agent makes architectural decisions. Authority is distributed. Humans approve before moving forward.

This is **systematic GenAI development**: structured orchestration, bounded authority, security discipline, and deep understanding of what models actually do.


***

# Installing VS Code and GitHub Copilot

To get started, install Visual Studio Code and the GitHub Copilot extension using the official documentation to ensure you always follow the latest instructions for your operating system.

## Install Visual Studio Code

Download and follow the installation guide for Windows or macOS:
https://code.visualstudio.com/

## Install GitHub Copilot Extension

After installing VS Code, open the Extensions marketplace and install GitHub Copilot, or follow the official setup guide here:
https://docs.github.com/en/copilot/getting-started-with-github-copilot


## Licensing and Pricing

Visual Studio Code is free to use. GitHub Copilot requires a paid subscription for most professional use cases. GitHub offers Individual, Business, and Enterprise plans. Pricing and feature sets may change over time, so always verify the latest details on the official page:

https://github.com/features/copilot/pricing

***

# Installing Integration Development Agents to Your Project

The goal is to let **GitHub Copilot Chat** update your current repository so that it implements the full Integration Agent environment described in instruction: **[installation/agent-workflow-setup-guide.md](installation/agent-workflow-setup-guide.md)**

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
- The file `docs/installation/agent-workflow-setup-guide.md` exists 
- The directory `docs/rules` exists 
- Copilot Chat is open



## Step 2 — Bootstrap the Environment from the Design Document

Choose the latest **Claude Opus** model and copy and paste the following prompt into Copilot Chat:


```
You are an environment setup assistant.

Your task is to update this repository so that it fully implements the agent environment described in:

docs/installation/agent-workflow-setup-guide.md

Instructions:
1. Read the entire file carefully before making changes.
2. Validate that the environment meets all requirements specified in Section 2 "Environment Requirements".
3. Identify all required files, folders, and configuration structures.
4. Create or update the repository so that it matches the specification.

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

After completing the base setup, validate:
- All environment requirements from the specification are met
- All required files and folders exist as specified
- All agent definitions are correctly structured

After setup and validation, check if the specification includes optional features (OPTIONs).
For each OPTION found in docs/installation/agent-workflow-setup-guide.md:
- Ask the user if they want to enable it
- If yes: follow the detailed instructions provided in that OPTION section

After completing all changes:
- List all created or modified files.
- Briefly summarize what was aligned with the specification.
- Report environment validation results.
- Report which OPTIONs were enabled.
```


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
2. Open `docs/rules/technology-stack.md` and define your actual technology rules and constrai[text](<../../../../../Library/Mobile Documents/.Trash/jare>)nts
3. Verify that the agent `.agent.md` files reference these files correctly

> **Without accurate project context and technology stack rules, agents will produce generic or incorrect outputs.** This is the single most important manual step in the setup.



***

## Step 4 — Validate Agent Configuration

After setup and configuration, validate that all agent instruction files are consistent and correct before starting actual work. Use this validation prompt to check for inconsistencies or conflicting guidance across all agent definitions.

**Open Copilot Chat** (do NOT select a specific agent — just use the general Copilot Chat), choose the latest Claude Opus model, and copy and paste the following prompt:

```
You are an environment validation assistant.

Your task is to audit all agent configuration files in this repository to ensure consistency,
correctness, and the absence of conflicting or contradictory instructions.

Files to check:
1. `.github/copilot-instructions.md`
2. `.github/agents/Integration Designer.agent.md`
3. `.github/agents/Integration Planner.agent.md`
4. `.github/agents/Integration Builder.agent.md`

For each file, verify:

**Structure & Completeness**
- Does it exist?
- Does it have the required frontmatter (description, tools)?
- Are all required sections present?

**Role Definitions**
- Are Designer, Planner, and Builder roles clearly separated?
- Does each agent have the correct assigned tools?
  - Designer: 'read', 'edit', 'search', 'fetch'
  - Planner: 'read', 'edit', 'search'
  - Builder: 'read', 'edit', 'search', 'execute'

**Rule Consistency**
- Are workflow rules (PRD → PLAN → TASK) consistently described across all agents?
- Is the status model (TASK and SUBTASK statuses) consistently defined?
- Are design principles and implementation guidelines consistent?

**Priority & Constraints**
- Are data source priorities clearly defined (e.g., /docs/prd before /docs/rules)?
- Are forbidden actions (e.g., "Builder must not modify PLAN") clearly stated for each role?
- Are web usage policies consistent with agent capabilities?

**Optional Features**
- If OPTION 2 (Rule System) is enabled, is it correctly referenced in Designer and/or Planner agents?
- If OPTION 3 (Delegation) is enabled, does Planner have 'agent' tool and correct delegation instructions?
- If OPTION 4 (Domain Expert Mode) is enabled, is /docs/[project-name] correctly referenced, and is the project identifier consistent across all agents?

**Inconsistencies to Flag**
- Contradictory instructions across agents
- Missing required files or sections
- Incorrect tool assignments
- Conflicting priority rules
- Incorrect file path references
- Mismatched project identifiers (if OPTION 4 enabled)

**Report Format**

For each issue found, provide:
1. **Issue:** Clear description of the problem
2. **Location:** Which file(s) and line(s)
3. **Current State:** What is currently written
4. **Expected State:** What should be corrected
5. **Fix:** Specific text replacement or addition recommended

If no issues are found, report:
"✓ All agent configuration files are consistent and correct. Ready for use."

After completing the audit, provide a summary:
- Total issues found
- By severity (Critical, Warning, Info)
- By file
- Recommended next steps
```

This validation ensures that before you start using agents for actual work, all configuration is consistent and correct. If any issues are found, apply the recommended fixes and validate again.

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

## Handling the Context

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

### **Phase 4 — Demo & Validation**

Once all TASKs are complete, request a live demonstration:

*In the same chat with Integration Builder:*

    Demonstrate this integration with one happy-path test case using curl.

Builder executes a sample request and displays the response.

### **Phase 5 — Change Handling & Re‑planning**

If new requirements or information appears:

1.  Human → Designer **(new chat)** — evaluate architectural impact, update PRD
2.  Designer → Planner **(new chat)** — update plan based on PRD changes
3.  Planner → Builder **(new chat)** — implement updated TASKs

*In a chat with Integration Planner:*

    Update plan based on updated PRD and test findings.

***

## **When Control Files Must Be Changed**

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

## **Agent Configuration Options**

Beyond the base setup, the Integration Agent Workflow supports several optional configurations to extend agent capabilities for specific project needs:

### **OPTION 1: Initialize Project Context and Technology Stack from Environment**
Automatically populate `project-context.md` and `technology-stack.md` based on your project's build files and local environment. Useful for fast onboarding in existing projects.

See: [Setup Guide — OPTION 1](installation/agent-workflow-setup-guide.md#option-1-initialize-project-context-and-technology-stack-from-environment)

### **OPTION 2: Enable Integration Architecture Rule System for Agents**
Add a deterministic, rule-based framework that guides Designer and Planner agents to make structured, traceable architecture decisions through explicit design principles and pattern selection rules.

See: [Setup Guide — OPTION 2](installation/agent-workflow-setup-guide.md#option-2-enable-integration-architecture-rule-system-for-agents)

### **OPTION 3: Agent Delegation – Orchestrating Planner and Builder**
Enable the Planner agent to delegate TASKs to the Builder agent for end-to-end implementation, testing, and committing, reducing manual coordination.

See: [Setup Guide — OPTION 3](installation/agent-workflow-setup-guide.md#option-3-agent-delegation-orchestrating-planner-and-builder-for-end-to-end-implementation)

### **OPTION 4: Project Domain Expert Mode**
Transform Designer, Planner, and Builder agents into project domain experts by having them prioritize project-specific documentation from `docs/[project-name]/`. Ensures agents apply domain context to all decisions and consistently update knowledge from project-specific guidance.

See: [Setup Guide — OPTION 4](installation/agent-workflow-setup-guide.md#option-4-project-domain-expert-mode)

Each OPTION is independent and can be combined with others to match your project's needs.

***

# **Demo Example**

Once the agents are installed in your project, you can walk through the full workflow using the **Star Wars Character Search** example below.


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


## **Phase 2 — Execution Planning (PLAN)**

**Open a new chat** with **Integration Planner Agent** (do not continue in the Designer chat). Choose the latest **Claude Opus** model and send the following prompt:

```
You are Integration Planner. Ignore previous designer and planner discussions.

Read the approved PRD under /docs/prd/ for the Star Wars Character Search service.

Create a complete execution plan with 3–6 vertical TASKs.
Each TASK must be end-to-end testable and production-ready.

Generate TASK documents under /docs/tasks/.
```

**Review and approve** the generated plan and TASK definitions before continuing.



## **Phase 3 — TASK Implementation**

**Open a new chat** with **Integration Builder Agent** (do not continue in the Planner chat). Choose the latest **Claude Sonnet** model and send the following prompt for the first TASK:

```
You are Integration Builder. Ignore previous discussions.

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



## **Phase 5 — Demo & Validation**

Once all TASKs are implemented and tests pass, request a live demonstration from the Builder. In the same **Integration Builder Agent** chat, send:

```
Looks good, but as one final request, I'd like you—as Integration Builder—to demonstrate this integration's functionality with one happy-path test case.

A curl command executed in the terminal should be sufficient for the demo environment.
```

The Builder will:
1. Start the service (if not already running)
2. Execute a sample request using `curl`
3. Display the response and confirm the integration works end-to-end

This demo session validates that:
- The service starts correctly
- External API calls work as expected
- Response formatting matches the PRD requirements
- Error handling is in place (optionally test an edge case)

> **Tip:** You can request additional demo scenarios, such as testing error handling with an invalid character name, or verifying timeout behavior.

***

# Removing Agent Workflow from a Project

If you need to remove the Agent Workflow setup from your project, use the following prompt. This prompt dynamically references the setup guide to ensure all components (including any new features added in future versions) are properly removed.

## Removal Prompt

```
Read the file agent-workflow-setup-guide.md from:
https://github.com/DataIntegrationMastery/integration-agent-workflow/blob/main/docs/installation/agent-workflow-setup-guide.md

If the guide cannot pe found from the web-address, try to find the same file from the project directory.

Analyze all components that the Agent Workflow setup creates in a project:
- Folder structures (/docs/architecture, /docs/plans, /docs/prd, /docs/rules, /docs/tasks, .github)
- Agent definition files (.github/agents/*.agent.md)
- Template files (TASK-template.md, SUBTASK-template.md, PRD-template.md, PLAN-template.md, PR-template.md)
- Rule files (project-context.md, technology-stack.md)
- Any other files and folders mentioned in the setup guide

Remove all identified Agent Workflow components from this project.

IMPORTANT: Do NOT remove:
- The project's actual source code
- User-created PRD, PLAN, or TASK documents (ask first)
- Other project-specific files

Finally, list all removed files and folders.
```

## Important

- **Always review** before confirming deletions — especially for `/docs/tasks/` which may contain completed work
- The prompt fetches the latest setup guide, ensuring it accounts for any new components added in future versions
- If you want to keep certain documentation (e.g., completed TASKs), answer accordingly when the agent asks

