# 06 – Agent Integration Layer

> *[< Main page](README.md)*

---

## 06.0 Purpose

This file defines how an AI integration design agent MUST use the handbook deterministically.

It specifies:

- Execution order
- Citation rules
- Validation execution requirements
- Output structure
- Fallback logic
- Human review triggers

---

## 06.1 Handbook File Loading Order

The agent MUST load and use handbook files in this order:

1. `00-scope.md`
2. `01-design-principles.md`
3. `02-decision-model.md`
4. `03-pattern-selection-rules.md`
5. `04-non-functional-architecture.md`
6. `05-validation-checklist.md`
7. `06-agent-integration.md`

If any file is missing, the agent MUST stop and return:

- `Status: Requires Human Review`
- `Reason: Missing handbook file(s)`
- `Missing: {list}`

---

## 06.2 Deterministic Execution Order (Decision Workflow)

For every integration design request, the agent MUST execute the steps below in order.

### Step 1 — Scope Gate

- Determine whether the request is within scope as defined in `00-scope.md`.
- IF out of scope:
  - return **Out of Scope** response (see Section 06.7)
  - stop execution

### Step 2 — Inputs Check

- Identify required inputs to make a design decision.
- IF critical inputs are missing:
  - request missing inputs OR produce **Requires Human Review** (see Section 06.7)
  - stop execution if the decision cannot be made deterministically

### Step 3 — Principle Application

- Identify applicable principles (`Pxx`) from `01-design-principles.md`.
- Apply precedence and conflict resolution rules from `01-design-principles.md`.

### Step 4 — Decision Model Application

- Apply applicable decision rules (`Dxx`) from `02-decision-model.md`.
- Decision rules MUST cite governing principles.

### Step 5 — Pattern Selection

- Select integration patterns using `03-pattern-selection-rules.md`.
- Patterns MUST be chosen based on rule conditions, not preference (P24).
- If no pattern fits, return **Requires Human Review**.

### Step 6 — Non-Functional Enforcement

- Apply `04-non-functional-architecture.md` rules (`NFRxx`).
- Any E0 NFR rule MUST be satisfied.

### Step 7 — Validation Checklist Execution

- Run the full checklist in `05-validation-checklist.md` (V01–V12).
- Apply Final Validation Rule (Section 05.7).
- If validation fails, design MUST be revised and revalidated before returning final status.

---

## 06.3 Citation Rules (Mandatory)

### 06.3.1 Citation Format

The agent MUST cite all applicable rules using:

- `Cites: Pxx, Dxx, PRxx, NFRxx, Vxx`

### 06.3.2 Minimum Citation Requirements

Every response MUST include:

- At least **2 principle citations** (`Pxx`)
- At least **1 decision rule citation** (`Dxx`) when the request is a design decision
- At least **1 non-functional citation** (`NFRxx`) for enterprise designs
- Validation references (`Vxx`) in the validation section

### 06.3.3 Claim-to-Rule Mapping

For each major recommendation, the agent MUST cite the rule IDs that justify it.

If a recommendation cannot be justified by the handbook:

- It MUST NOT be presented as a decision.
- It MUST be marked as **Out of Scope** or **Requires Human Review**.

---

## 06.4 Output Structure (Mandatory)

The agent MUST output responses using this structure:

### 1) Summary

- One paragraph summarizing the proposed integration design.

### 2) Assumptions & Inputs

- List the inputs used.
- List assumptions made.
- Highlight missing inputs if any.

### 3) Architecture Decision Set

Provide a numbered list of decisions, each with:

- **Decision:** {short statement}
- **Rationale:** {one short paragraph}
- **Cites:** {rule IDs}

### 4) Pattern Set

Provide selected patterns:

- **Pattern:** {name}
- **Why this pattern:** {short statement}
- **Constraints:** {mandatory constraints}
- **Cites:** {PRxx + related P/D/NFR}

### 5) Non-Functional Controls

List required controls:

- Reliability controls
- Scalability controls
- Observability controls
- Security controls

Each item MUST include citations.

### 6) Validation Results

Provide:

- Scores for V01–V12
- Highlight categories ≥3
- Required mitigations for ≥4
- Final validation status per Section 05.7
- `Cites: Vxx`

### 7) Final Status

One of:

- `Status: Acceptable`
- `Status: Acceptable with Mitigations`
- `Status: Design Review Required`
- `Status: Reject Design`
- `Status: Requires Human Review`
- `Status: Out of Scope`

---

## 06.5 Fallback Logic (Deterministic Defaults)

Fallbacks MUST be applied only when the handbook allows a deterministic default.

### 06.5.1 Default Communication Posture

IF:
- Consistency requirements are unclear
THEN:
- Prefer asynchronous, eventual consistency
- Require explicit consistency declaration before final approval
- Cites: P04, P22, D01, D04

### 06.5.2 Default Coupling Posture

IF:
- Integration boundary responsibilities are unclear
THEN:
- Default to strict boundary enforcement and minimal contract exposure
- Cites: P02, P18

### 06.5.3 Default Failure Posture

IF:
- Failure strategy is missing
THEN:
- Enforce timeouts, bounded retries, and failure isolation controls
- Cites: P09, P11, NFR01, NFR02, NFR03

### 06.5.4 Default Observability Posture

IF:
- Observability requirements are unclear
THEN:
- Require correlation IDs and core operational metrics
- Cites: P13, P14, NFR06, NFR07

---

## 06.6 Human Review Triggers (Mandatory)

The agent MUST return **Requires Human Review** if any condition is true:

1. Any unresolved rule conflict remains after conflict resolution steps.
2. Any validation category scores **5** and cannot be redesigned within provided inputs.
3. The request requires technology/vendor/tool selection decisions.
4. Regulatory, contractual, or policy constraints are referenced but not provided.
5. Data ownership (source of truth) cannot be determined.
6. Event ordering or workflow semantics are unclear but required.
7. The design requires cross-system atomicity.
8. The design depends on unknown external system behavior or SLAs.

Output MUST include:

- `Status: Requires Human Review`
- `Trigger: {list}`
- `Missing: {inputs}`
- `Conflicts: {rule IDs}` (if any)

---

## 06.7 Standard Responses

### 06.7.1 Out of Scope Response

- `Status: Out of Scope`
- `Reason: {one sentence}`
- `Requires: {missing inputs or authority}`

### 06.7.2 Requires Human Review Response

- `Status: Requires Human Review`
- `Trigger: {list}`
- `Missing: {inputs}`
- `Next Questions: {up to 5}`

---
> *[< Main page](README.md)*

