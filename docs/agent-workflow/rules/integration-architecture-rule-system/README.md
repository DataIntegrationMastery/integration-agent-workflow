# Integration Architecture Rule System
**VERSION 1.3** / *21th Feb 2026*

---
**Integration Architecture Rule Engine Specification for Deterministic AI-Based Design**

Ruleset files developed by **[Data Integration Mastery](https://dataintegrationmastery.com/)**

*This Integration Architecture Rule System is derived from the architectural philosophy and integration design methodology presented in the Mastering Integration Development  course by Data Integration Mastery. It formalizes the core principles of coupling minimization, boundary discipline, asynchronous-first design, failure isolation, consistency clarity, and context-driven pattern selection into a deterministic rule engine specification suitable for AI-based integration design.*


---

## 1. Purpose

The Integration Architecture Rule System defines a deterministic, technology-neutral rule engine for designing integration architectures in enterprise system landscapes.

It serves as:

- A normative authority for AI-driven integration design agents  
- A structured decision framework for integration architects  
- A deterministic validation and enforcement model  
- A foundation for automated architecture auditing  

This specification governs how integration decisions are made, validated, and justified.

---

## 2. Normative Authority

This document and its referenced sub-files collectively form the authoritative rule system.

AI agents and automated systems using this framework MUST:

- Apply rules deterministically
- Cite rule identifiers in architectural decisions
- Execute validation before finalizing design outputs
- Enforce precedence and conflict resolution mechanisms
- Reject or escalate non-compliant designs

If any rule conflict or scope ambiguity cannot be resolved deterministically, the system MUST require human review.

---

## 3. System Structure

All normative rules are defined in this directory.

### Core Components

- [00 – Scope Definition](00-scope.md)  
- [01 – Design Principles](01-design-principles.md)  
- [02 – Decision Model](02-decision-model.md)  
- [03 – Pattern Selection Rules](03–pattern-selection-rules.md)  
- [04 – Non-Functional Architecture Rules](04–non-functional-architecture-rules.md)  
- [05 – Validation Checklist](05–validation-checklist.md)  
- [06 – Agent Integration Layer](06–agent-integration-layer.md)  

---

## 4. Execution Model

The Integration Architecture Rule System operates as a deterministic rule engine with the following properties:

- Explicit rule identifiers (Pxx, Dxx, PRxx, NFRxx, Vxx)
- Enforcement priority levels (E0–E3)
- Deterministic conflict resolution
- Mandatory validation scoring
- Structured output requirements
- Human review triggers

All integration design outputs produced under this system MUST be traceable to rule identifiers.

---

## 5. Intended Use

This specification is intended for:

- AI-based integration design agents
- Enterprise architecture governance
- Deterministic integration validation tooling
- Structured architectural review processes

It is technology-neutral and vendor-agnostic.

---

## 6. Attribution

Integration Architecture Rule System  
© Data Integration Mastery  

Developed by Data Integration Mastery  
https://dataintegrationmastery.com/

---

## 7. Installation Guide — GitHub Copilot Agent Instruction File

This section describes how to integrate the Integration Architecture Rule System into a **GitHub Copilot Agent** definition file (`.agent.md`) in VS Code.

### 7.1 Prerequisites

- VS Code with GitHub Copilot and Copilot Chat enabled
- Agent Mode enabled
- The rule system files placed under `/docs/agent-workflow/rules/` in your repository:
  ```
  /docs/agent-workflow/rules/
      integration-architecture-rule-system/
          README.md
          00-scope.md
          01-design-principles.md
          02-decision-model.md
          03–pattern-selection-rules.md
          04–non-functional-architecture-rules.md
          05–validation-checklist.md
          06–agent-integration-layer.md
  ```

### 7.2 Target Agent

The rule system is designed for an **Integration Designer** agent — an architect-level thinking partner that:
- Defines and validates PRD documents
- Evaluates integration patterns
- Enforces non-functional requirements
- Does NOT generate production implementation code

### 7.3 Agent File Location

Create or update the agent definition at:

```
.github/agents/Integration Designer.agent.md
```

### 7.4 Section to Add

Add the following section to the agent's `.agent.md` file (recommended placement: before the `## Project Context` section):

```markdown
## Integration Architecture Rule System

This repository includes the **Integration Architecture Rule System** under `/docs/agent-workflow/rules/`.
It is your **normative authority** for integration architecture decisions.

### Rule System Files (load order)

1. `/docs/agent-workflow/rules/integration-architecture-rule-system/00-scope.md` — scope definition
2. `/docs/agent-workflow/rules/integration-architecture-rule-system/01-design-principles.md` — design principles (P01–P25)
3. `/docs/agent-workflow/rules/integration-architecture-rule-system/02-decision-model.md` — decision rules (D01–D14)
4. `/docs/agent-workflow/rules/integration-architecture-rule-system/03–pattern-selection-rules.md` — pattern selection (PR01–PR15)
5. `/docs/agent-workflow/rules/integration-architecture-rule-system/04–non-functional-architecture-rules.md` — NFR rules (NFR01–NFR12)
6. `/docs/agent-workflow/rules/integration-architecture-rule-system/05–validation-checklist.md` — validation checklist (V01–V12)
7. `/docs/agent-workflow/rules/integration-architecture-rule-system/06–agent-integration-layer.md` — agent execution model

### How to Use the Rule System

- **When making architecture decisions:** Read relevant rule files and apply principles (`Pxx`),
  decision rules (`Dxx`), and pattern selection rules (`PRxx`) deterministically.
- **When defining NFRs in PRD:** Validate against `NFR01–NFR12`.
- **When finalizing a PRD:** Run the validation checklist (`V01–V12`) and include scores.
- **Citation requirement:** Every architecture decision in a PRD MUST cite applicable rule
  identifiers (e.g., `Cites: P01, P04, D01, NFR02`).
- **Human review triggers:** If any validation category scores 5, or rule conflicts cannot be
  resolved, return `Status: Requires Human Review`.

### Loading Strategy

Do NOT load all rule files at once. Read them on demand:
- Start with `00-scope.md` to verify the request is in scope.
- Load `01-design-principles.md` and `02-decision-model.md` for architecture decisions.
- Load `03–pattern-selection-rules.md` when evaluating integration patterns.
- Load `04–non-functional-architecture-rules.md` when defining or validating NFRs.
- Load `05–validation-checklist.md` when performing final PRD validation.
- Refer to `06–agent-integration-layer.md` for output structure and citation rules.
```

### 7.5 Behavioral Impact

Once configured, the Designer agent will:

1. **Cite rule identifiers** in every architecture decision (e.g., `Cites: P01, P04, D01`)
2. **Apply deterministic decision rules** instead of relying on general knowledge
3. **Run validation scoring** (V01–V12) before finalizing PRD documents
4. **Trigger human review** when rule conflicts cannot be resolved or critical risks are detected
5. **Load rule files on demand** to preserve context window efficiency

### 7.6 Verification

After installation, verify by asking the Designer agent:

```
Read the Integration Architecture Rule System under /docs/agent-workflow/rules/.
Confirm you can access the rule files and cite rule identifiers.
```

Expected behavior: The agent should reference specific rule IDs (Pxx, Dxx, PRxx, NFRxx, Vxx) and follow the deterministic execution order defined in `06–agent-integration-layer.md`.

