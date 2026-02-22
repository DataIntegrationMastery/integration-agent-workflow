# Integration Agent Workflow

A production-oriented agent workflow for integration development using **VS Code** and **GitHub Copilot Agents**.  

This repository implements a structured **PRD → PLAN → TASK** delivery model with strict role separation:

- **Integration Designer** – Architecture & PRD
- **Integration Planner** – TASK breakdown & execution planning
- **Integration Builder** – Implementation & commit discipline

The model follows the principle:

> **Human-as-Architect, AI-as-Builder**

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

Allowed TASK statuses:

- `TO-DO`
- `IN-PROGRESS`
- `IN-REVIEW`
- `DONE`
- `CANCELLED`

---

## 3. Implementation Discipline

Integration Builder:

- Implements one TASK at a time
- Breaks work into SUBTASK files if needed
- Commits per SUBTASK
- Moves TASK to `IN-REVIEW`
- Never sets `DONE` (human approval required)

---

# 📂 Repository Structure
