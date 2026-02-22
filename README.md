# Integration Agent Workflow
**Version 1.8** / *22th Feb 2026*

A production-oriented agent workflow for integration development using **VS Code** and **GitHub Copilot Agents**.  

This repository implements a structured **PRD → PLAN → TASK** delivery model with strict role separation:

- **Integration Designer** – Architecture & PRD
- **Integration Planner** – TASK breakdown & execution planning
- **Integration Builder** – Implementation & commit discipline

The model follows the principle:

> **Human-as-Architect, AI-as-Builder**

Developed by **[Data Integration Mastery](https://dataintegrationmastery.com/)**

---

# 🎯 Purpose

This repository demonstrates how to:

- Structure integration development using Copilot Agents
- Maintain architectural control while accelerating implementation
- Enforce vertical delivery (TASK-based execution)
- Keep repository documentation as the single source of truth
- Prevent role drift between design, planning, and coding

It is designed for real-world API and integration services (e.g., aggregation, enrichment, transformation).

---

# 🏗 Workflow Model

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

# Getting Started

The goal is to let **GitHub Copilot Chat** update your current repository so that it implements the full Integration Agent environment described in:

`docs/developing-integrations-with-copilot.md`

Make sure:
- You are in the root of your repository in VS Code
- The file `docs/developing-integrations-with-copilot.md` already exists -
- Copilot Chat is open

---

## Step 1 — Bootstrap the Environment from the Design Document

Copy and paste the following prompt into Copilot Chat:

---

You are an environment setup assistant.

Your task is to update this repository so that it fully implements the agent environment described in:

docs/developing-integrations-with-copilot.md

Instructions:

1. Read the entire file carefully before making changes.
2. Identify all required:
   - Directory structures
   - Agent definition files
   - Workflow documentation
   - TASK model definitions
   - copilot-instructions rules
3. Create or update the repository so that it matches the specification.

Specifically ensure:

- `.github/agents/` exists
- Integration Designer.agent.md exists
- Integration Planner.agent.md exists
- Integration Builder.agent.md exists
- copilot-instructions.md exists in the repository root
- `/docs/tasks/` structure exists
- TASK-template.md exists
- Status model includes:
  - TO-DO
  - IN-PROGRESS
  - IN-REVIEW
  - DONE
  - CANCELLED
- Controlled web usage policy is implemented (Designer only, verification-only use)

Rules:

- Do NOT modify business source code.
- Do NOT generate example PoC implementations.
- Only create or update configuration and documentation files required by the workflow.
- If something already exists, update it to align with the specification rather than duplicating it.
- Do not introduce new architectural concepts not defined in the specification document.

After completing changes:
- List all created or modified files.
- Briefly summarize what was aligned with the specification.

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

