# 02 – Decision Model

> *[< Main page](README.md)*

---

## 02.0 Purpose

This file defines deterministic IF/THEN decision rules for integration architecture.
All decisions MUST align with principles defined in `01-design-principles.md`.

---

# 02.1 Communication Style Selection

## D01 – Synchronous vs Asynchronous

IF:
- Immediate user response is required
- AND strong consistency is mandatory
THEN:
- Synchronous communication MAY be used
- MUST comply with P05
- Cites: P05, P22

ELSE:
- Asynchronous communication SHOULD be used
- MUST comply with P01 and P04
- Cites: P01, P04

---

## D02 – Long Dependency Chain Detection

IF:
- A synchronous call requires more than one downstream system
THEN:
- Design MUST be refactored
- Introduce asynchronous boundary or decoupling layer
- Cites: P15, P01

---

# 02.2 Consistency Model Selection

## D03 – Distributed Transaction Prohibition

IF:
- Proposed design requires atomic commit across multiple independent systems
THEN:
- Reject design
- Use eventual consistency or compensating actions
- Cites: P21

---

## D04 – Explicit Consistency Declaration

IF:
- Data is propagated across system boundaries
THEN:
- Consistency model MUST be declared (strong, eventual, compensating)
- Cites: P22

---

# 02.3 Orchestration vs Choreography

## D05 – Centralized Process Coordination

IF:
- A multi-step business process spans multiple systems
- AND requires ordered execution
THEN:
- Explicit orchestration component SHOULD be used
- Cites: P06, P20

---

## D06 – Independent Event Reaction

IF:
- Systems react independently to domain events
- AND no strict ordering is required
THEN:
- Choreography MAY be used
- MUST ensure idempotent handling
- Cites: P10, P20

---

# 02.4 Data Transformation Placement

## D07 – Transformation Location

IF:
- Two systems use different data schemas
THEN:
- Transformation MUST occur in integration boundary
- MUST NOT modify internal domain model for integration convenience
- Cites: P07, P08

---

## D08 – Shared Model Rejection

IF:
- Proposed integration shares internal domain objects between systems
THEN:
- Reject design
- Introduce explicit contract or schema
- Cites: P08

---

# 02.5 Failure Handling

## D09 – Remote Dependency Failure Risk

IF:
- Integration depends on remote system availability
THEN:
- Timeout control MUST be defined
- Retry policy MUST be defined
- Failure isolation MUST be ensured
- Cites: P09, P11

---

## D10 – Retry Safety Check

IF:
- Retry mechanism is introduced
THEN:
- Idempotency MUST be guaranteed
- Cites: P10

---

# 02.6 Flow Control

## D11 – Throughput Mismatch Detection

IF:
- Producer throughput exceeds consumer capacity
THEN:
- Buffering or throttling MUST be introduced
- Cites: P16

---

# 02.7 Authority & Ownership

## D12 – Source of Truth Definition

IF:
- Data is replicated or cached
THEN:
- Authoritative source MUST be declared
- Bidirectional uncontrolled updates MUST NOT be allowed
- Cites: P23

---

# 02.8 Observability

## D13 – Multi-System Flow

IF:
- A request or event traverses more than one system
THEN:
- Correlation identifier MUST be propagated end-to-end
- Cites: P13

---

## D14 – Operational Visibility

IF:
- Integration component processes asynchronous messages
THEN:
- Metrics for throughput, latency, and failure rate MUST be exposed
- Cites: P14

---
> *[< Main page](README.md)*
