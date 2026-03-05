# 05 – Validation Checklist

> *[< Main page](README.md)*

---

## 05.0 Purpose

This file defines a deterministic audit checklist for validating integration architecture decisions.

The checklist MUST be applied before finalizing any integration design.

Each category is scored on a scale of **0–5**:

- **0** = Fully compliant, no risk
- **1–2** = Low risk
- **3** = Moderate risk (requires justification)
- **4** = High risk (requires mitigation)
- **5** = Critical risk (design MUST be revised)

If any category scores **5**, the design MUST be rejected or redesigned.

---

# 05.1 Coupling Risk Assessment

## V01 – Temporal Coupling

Score 0–5 based on:

- 0 = Fully asynchronous, no blocking dependencies
- 2 = Limited synchronous calls with bounded timeout
- 3 = Multiple synchronous dependencies
- 4 = Deep synchronous chain across systems
- 5 = Distributed transaction across systems

Mitigation Required If ≥3  
Cites: P01, P05, P15, P21, D02

---

## V02 – Structural Coupling

Score 0–5 based on:

- 0 = Explicit contract, no shared internal models
- 2 = Versioned contracts, loosely coupled schemas
- 3 = Partial schema reuse
- 4 = Shared internal domain objects
- 5 = Shared database or direct internal model access

Mitigation Required If ≥3  
Cites: P08, P07, D08

---

## V03 – Behavioral Coupling

Score 0–5 based on:

- 0 = No cross-system implicit ordering
- 2 = Explicit orchestration with defined flow
- 3 = Implicit ordering assumptions
- 4 = Hidden workflow dependencies
- 5 = Uncontrolled interdependent state transitions

Mitigation Required If ≥3  
Cites: P20, P06, D05

---

# 05.2 Failure Blast Radius

## V04 – Failure Propagation Risk

Score 0–5 based on:

- 0 = Full isolation with buffering or async decoupling
- 2 = Controlled retries and timeouts
- 3 = Limited synchronous dependencies
- 4 = Cascading synchronous chain
- 5 = No timeout, no isolation

Mitigation Required If ≥3  
Cites: P09, P11, NFR01, NFR03

---

## V05 – Retry Safety

Score 0–5 based on:

- 0 = Idempotent and bounded retry
- 2 = Retry with defined limits
- 3 = Retry without idempotency guarantee
- 4 = Unlimited retry attempts
- 5 = Retry on business errors

Mitigation Required If ≥3  
Cites: P10, P12, PR09, D10

---

# 05.3 Consistency Clarity

## V06 – Consistency Model Definition

Score 0–5 based on:

- 0 = Explicit consistency model documented
- 2 = Clear eventual consistency strategy
- 3 = Assumed but undocumented model
- 4 = Mixed consistency assumptions
- 5 = Distributed transaction dependency

Mitigation Required If ≥3  
Cites: P22, P21, D03, D04

---

## V07 – Source of Truth Clarity

Score 0–5 based on:

- 0 = Single clearly defined authority
- 2 = Authority defined with replication rules
- 3 = Partial ownership overlap
- 4 = Ambiguous ownership
- 5 = Bidirectional uncontrolled updates

Mitigation Required If ≥3  
Cites: P23, D12

---

# 05.4 Scalability Risk

## V08 – Throughput Management

Score 0–5 based on:

- 0 = Explicit backpressure or queue-based decoupling
- 2 = Throttling or rate limits defined
- 3 = Capacity mismatch acknowledged but unmanaged
- 4 = Producer can overload consumer
- 5 = No flow control in high-variance load scenario

Mitigation Required If ≥3  
Cites: P16, D11, NFR05

---

## V09 – Horizontal Scaling Capability

Score 0–5 based on:

- 0 = Stateless or horizontally scalable integration layer
- 2 = Limited shared state with mitigation
- 3 = Local state dependency
- 4 = Tight scaling constraints
- 5 = Single-node bottleneck by design

Mitigation Required If ≥3  
Cites: NFR04, P16

---

# 05.5 Observability Completeness

## V10 – Traceability Coverage

Score 0–5 based on:

- 0 = End-to-end correlation ID propagation
- 2 = Partial trace coverage
- 3 = Logging without correlation continuity
- 4 = Inconsistent trace propagation
- 5 = No cross-system trace capability

Mitigation Required If ≥3  
Cites: P13, NFR06, D13

---

## V11 – Operational Visibility

Score 0–5 based on:

- 0 = Metrics exposed (throughput, latency, error rate, backlog)
- 2 = Basic logging and monitoring
- 3 = Limited metric coverage
- 4 = Manual inspection required for diagnosis
- 5 = No operational observability

Mitigation Required If ≥3  
Cites: P14, NFR07, D14

---

# 05.6 Security Posture

## V12 – Trust Boundary Enforcement

Score 0–5 based on:

- 0 = Authentication and authorization enforced at all boundaries
- 2 = Authentication enforced, limited authorization checks
- 3 = Inconsistent enforcement
- 4 = Implicit trust between systems
- 5 = No boundary validation

Mitigation Required If ≥3  
Cites: P17, NFR08, NFR09

---

# 05.7 Final Validation Rule

After scoring all categories:

- IF any score = 5 → **Reject Design**
- IF any score ≥4 → **Mandatory Mitigation Required**
- IF three or more categories ≥3 → **Design Review Required**
- ELSE → **Design Acceptable**

Agent MUST output:

- Score table
- Risk categories ≥3
- Required mitigations
- Final status

---
> *[< Main page](README.md)*
