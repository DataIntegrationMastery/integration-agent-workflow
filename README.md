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

1. [Documentation](#documentation)
2. [Purpose](#purpose)
3. [Workflow Model](#workflow-model)
4. [Installing Agents to Your Project](#installing-agents-to-your-project)
5. [Choosing the Language Models for the Agents](#choosing-the-language-models-for-the-agents)
6. [Using the Agents](#using-the-agents)
7. [Artifact-First Principle](#artifact-first-principle)
8. [When Instructions Have Changed](#when-instructions-have-changed)
9. [Working Method](#working-method)
10. [Workflow Phases](#workflow-phases)
11. [Demo Example](#demo-example)

---

# Documentation

| Document | Description |
|----------|-------------|
| [Developing Integrations with Copilot](docs/developing-integrations-with-copilot.md) | Main workflow specification – agent roles, folder structure, status model |
| [Integration Architecture Rule System](docs/rules/integration-architecture-rule-system/README.md) | Architecture rules, design principles, pattern selection, and validation checklists |

---

# Purpose

This repository demonstrates how to:

- Structure integration development using Copilot Agents
- Maintain architectural control while accelerating implementation
- Enforce vertical delivery (TASK-based execution)
- Keep repository documentation as the single source of truth
- Prevent role drift between design, planning, and coding

It is designed for real-world API and integration services (e.g., aggregation, enrichment, transformation).

---

# Workflow Model

## 1. PRD (Product Requirements Document)
Created by **Integration Designer**.

Defines:
- Integration flow
- Canonical response model
- Business rules
- Non-functional requirements
- Error handling policy

PRD is the architectural source of truth.

---

## 2. PLAN → TASK Structure (v2.1 Model)

The Integration Planner translates PRD into vertical delivery units.

Each TASK:

- Represents one end-to-end capability
- Has its own folder under `/docs/tasks/TASK-XX/`
- Contains a `TASK-XX.md` root document
- May contain multiple `SUBTASK-XX.md` files

---

# Installing Agents to Your Project

The goal is to let **GitHub Copilot Chat** update your current repository so that it implements the full Integration Agent environment described in instruction:

`docs/developing-integrations-with-copilot.md`

Make sure:
- You are in the root of your repository in VS Code
- The file `docs/developing-integrations-with-copilot.md` already exists -
- Copilot Chat is open

---

## Step 1 — Bootstrap the Environment from the Design Document

Copy and paste the following prompt into Copilot Chat:


```
You are an environment setup assistant.

Your task is to update this repository so that it fully implements the agent environment described in:

docs/developing-integrations-with-copilot.md

Instructions:
1. Read the entire file carefully before making changes.
2. Identify all required.
3. Create or update the repository so that it matches the specification.

Specifically ensure:
- `.github/agents/` exists
- copilot-instructions.md exists in the repository root
- Required *template.md -files exists

Rules:
- Do NOT modify business source code.
- Do NOT generate example PoC implementations.
- Do NOT implement anything from instruction which is maked as 'OPTIONAL'
- Only create or update configuration and documentation files required by the workflow.
- If something already exists, update it to align with the specification rather than duplicating it.
- Do not introduce new architectural concepts not defined in the specification document.

After completing changes:
- List all created or modified files.
- Briefly summarize what was aligned with the specification.
```
---

## Step 2 — Review the Changes

After Copilot completes:

1. Review all `.agent.md` files
2. Verify role separation:
   - Designer: read, search, web (controlled)
   - Planner: read, search, edit
   - Builder: read, search, edit, execute
3. Verify TASK structure under `/docs/tasks/`
4. Verify status model consistency
5. Confirm that PRD is defined as architectural source of truth

---

# **Choosing the Language Models for the Agents**

## **Best models as of Feb 2026**

### **Designer → *Claude Opus 4.6***

Reasons:

*   Excellent long-context architecture reasoning
*   Low hallucination rate
*   Ideal for PRD writing and NFR validation
*   Strong system-level thinking

Alternative: ***GPT‑5.2***

### **Planner → *Claude Opus 4.6***

Reasons:

*   Excellent long-context planning
*   Low hallucination rate
*   Ideal for PRD/PLAN writing

Alternative: ***GPT‑5.2***

### **Builder → *Claude Sonnet 4.6***

Reasons:

*   Strong code generation
*   Stable in agent-mode loop
*   Good for tests & error handling

Alternatives:

*   ***GPT‑5.3 Codex*** (complex Java/Camel)
*   ***Claude Haiku / GPT‑5 mini*** (fast small tasks)

***


# **Using the Agents**

Agent memory comes from:

1.  System prompt
2.  `copilot-instructions.md`
3.  Current conversation
4.  Workspace files

## **Do not mix roles**

*   Use a separate chat for Designer
*   Use a separate chat for Planner
*   Use a separate chat for Builder

To reset context:

Designer:

    Act as Integration Designer. Read the current PRD under /docs/prd/PRD-X.md. Ignore planning and build discussions.

Planner:

    Act as Integration Planner. Read PRD under /docs/prd/PRD-X.md. Ignore previous build discussions.

Builder:

    Act as Integration Builder. Implement TASK-02 from /docs/tasks. Ignore planning discussions.

***


# **Artifact‑First Principle**

Agent memory **must be in files**, not in chat.

*   PRDs
*   PLANs
*   TASK documents

Avoid relying on past conversation.

***


# **When Instructions Have Changed**

If you update `.github/copilot-instructions.md` during development, make sure the agents actually use the new rules.

## Recommended (most reliable)
1.	Save and commit the updated copilot-instructions.md.
2.	Close the current chat.
3.	Start a new chat and select the correct agent.
4.	Begin with: 
```Act according to the updated .github/copilot-instructions.md. Confirm you are using the latest version.```

This ensures the agent reloads repository context and applies the new rules cleanly.

## Alternative (within the same chat)

If you do not want to restart the conversation, explicitly instruct the agent: 
``` Re-read .github/copilot-instructions.md and apply the updated rules before continuing.```

This often works, but for significant rule changes, starting a new chat is safer and more deterministic.

***


# **Working Method**

## **Purpose of the PRD → PLAN → TASK model**

Integration development needs structure because it involves:

*   Multiple systems
*   Contracts
*   Error handling
*   Retry & idempotency
*   Performance & reliability
*   Observability

Without structure:

*   Code is developed before NFR decisions
*   Retry logic added late
*   Observability incomplete
*   Tests miss error paths

The model prevents this by enforcing:

1.  **PRD:** explicit decisions
2.  **PLAN:** structured architecture & sequencing
3.  **TASK:** real end‑to‑end increments

TASKs are:

*   testable
*   production‑ready
*   compliant
*   small

AI benefits because:

*   PRD gives context
*   PLAN gives decomposition
*   TASK gives precise implementation contract

Humans maintain architecture.  
AI accelerates implementation.

***


# **Workflow Phases**

## **Phase 1 — Architecture & Requirements (PRD)**

Designer + Human:

*   Define problem
*   Define system landscape
*   Define flows
*   Define NFRs

Designer:

    Create PRD for feature X in /docs/prd.

Human approves.

## **Phase 2 — Execution Planning (PLAN)**

Planner:

    Create plan from approved PRD. Include 3–6 TASKs.

Human approves.

Planner can generate tasks for each TASK.

## **Phase 3 — TASK Implementation**

Builder:

    Implement TASK-01 exactly as defined.

Builder:

*   Implements
*   Runs tests
*   Fixes issues
*   Reports

## **Phase 4 — Change Handling & Re‑planning**

If new requirements or information appears:

1.  Human → Designer (evaluate architectural impact, update PRD)
2.  Designer → Planner (update plan based on PRD changes)
3.  Planner → Builder (implement updated TASKs)

Planner:

    Update plan based on updated PRD and test findings.

***

# Demo Example

Once the agents are installed in your project, you can walk through the full workflow using the **Star Wars Character Search** example below.

---

## **Phase 1 — Architecture & Requirements (PRD)**

Send the following prompt to **Integration Designer Agent**:

```
You are Integration Designer.

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

Open a **new chat** and send the following prompt to **Integration Planner Agent**:

```
You are Integration Planner.

Read the approved PRD under /docs/prd/ for the Star Wars Character Search service.

Create a complete execution plan with 3–6 vertical TASKs.
Each TASK must be end-to-end testable and production-ready.

Generate TASK documents under /docs/tasks/.
```

**Review and approve** the generated plan and TASK definitions before continuing.

---

## **Phase 3 — TASK Implementation**

Open a **new chat** and send the following prompt to **Integration Builder Agent** for the first TASK:

```
You are Integration Builder.

Implement TASK-01 exactly as defined in /docs/tasks/TASK-01/.

Follow all rules from copilot-instructions.md.
Run tests after implementation and report results.
```

After TASK-01 is complete and verified, continue with the next TASK:

```
You are Integration Builder.

Implement TASK-02 exactly as defined in /docs/tasks/TASK-02/.

Follow all rules from copilot-instructions.md.
Run tests after implementation and report results.
```

Repeat for each remaining TASK until the service is fully implemented.

---

## **Phase 4 — Change Handling & Re-planning**

If during implementation new requirements or issues arise, open a **new chat** with the **Integration Designer Agent**:

```
You are Integration Designer.

Read the current PRD under /docs/prd/ for the Star Wars Character Search service.

Evaluate the following change request and update the PRD if needed:
- [describe the change or new requirement here]
```

After the PRD is updated, switch to **Integration Planner Agent** in a **new chat**:

```
You are Integration Planner.

The PRD under /docs/prd/ has been updated.

Review the changes, update the plan, and adjust or create new TASKs as needed under /docs/tasks/.
```

Then continue implementation with the **Integration Builder Agent** as in Phase 3.

