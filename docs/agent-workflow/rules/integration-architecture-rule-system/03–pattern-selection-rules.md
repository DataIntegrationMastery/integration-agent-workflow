# 03 – Pattern Selection Rules

> *[< Main page](README.md)*

---

## 03.0 Purpose

This file defines deterministic pattern selection rules for integration design.
Patterns MUST be selected according to context and MUST comply with the design principles and decision model.

---

## 03.1 Pattern Rule Object Model

Each pattern rule MUST follow this structure:

- **Pattern ID**
- **Pattern Name**
- **Use When**
- **Avoid When**
- **Mandatory Constraints**
- **Failure Risk**
- **Agent Enforcement Level**
- **Cites**

### 03.1.1 Pattern Template

**PR{NN} – {Pattern Name}**  
- **Use When:** {conditions}  
- **Avoid When:** {anti-conditions}  
- **Mandatory Constraints:** {must-have constraints}  
- **Failure Risk:** {primary risks if misused}  
- **Agent Enforcement Level:** {E0 | E1 | E2 | E3}  
- **Cites:** {Pxx[, Dyy...]}

---

# 03.2 Routing Patterns

**PR01 – Direct Routing (Point-to-Point Route)**  
- **Use When:** A single producer sends to a single consumer with stable ownership and minimal branching needs.  
- **Avoid When:** Multiple consumers or future branching is likely; when it creates tight coupling to a single endpoint.  
- **Mandatory Constraints:** Route responsibility MUST remain within integration boundary; avoid embedding domain logic.  
- **Failure Risk:** Hard coupling, brittle change impact, cascading refactor when new consumers appear.  
- **Agent Enforcement Level:** E2  
- **Cites:** P01, P02, P25, P24  

---

**PR02 – Content-Based Router**  
- **Use When:** Routing destination depends on message content, type, or validated attributes; multiple target paths exist.  
- **Avoid When:** Routing depends on deep domain logic; routing rules change frequently without governance; message content is not trustworthy.  
- **Mandatory Constraints:** Routing predicates MUST be explicit and testable; routing MUST NOT require access to internal target system state.  
- **Failure Risk:** Hidden business logic in router, uncontrolled branching complexity, incorrect routing under schema evolution.  
- **Agent Enforcement Level:** E1  
- **Cites:** P24, P02, P18, D07  

---

**PR03 – Recipient List**  
- **Use When:** One message must be delivered to multiple recipients where recipients are known and bounded.  
- **Avoid When:** Recipient set is unbounded or dynamically discovered without governance; when it creates fan-out overload.  
- **Mandatory Constraints:** Backpressure controls MUST exist for fan-out; delivery semantics MUST be defined per recipient.  
- **Failure Risk:** Load amplification, partial delivery inconsistencies, observability gaps across recipients.  
- **Agent Enforcement Level:** E1  
- **Cites:** P16, P09, P14, D11  

---

**PR04 – Dynamic Recipient List**  
- **Use When:** Recipient set is determined at runtime from explicit configuration within integration boundary.  
- **Avoid When:** Runtime discovery requires calling remote systems synchronously; when recipients change without traceability.  
- **Mandatory Constraints:** Recipient discovery MUST be local/config-driven; changes MUST be auditable.  
- **Failure Risk:** Hidden temporal dependencies, unpredictable fan-out, operational instability.  
- **Agent Enforcement Level:** E2  
- **Cites:** P20, P16, P14  

---

# 03.3 Filtering Patterns

**PR05 – Message Filter**  
- **Use When:** Messages must be excluded based on simple, explicit criteria to reduce downstream noise.  
- **Avoid When:** Filtering implements business decisions that belong to domain systems; when filter rules are ambiguous.  
- **Mandatory Constraints:** Filter criteria MUST be explicit; rejected messages MUST have defined handling (drop, park, or audit).  
- **Failure Risk:** Silent data loss, hidden business logic, compliance risk if discarding is not auditable.  
- **Agent Enforcement Level:** E1  
- **Cites:** P02, P14, P18, D14  

---

# 03.4 Transformation & Mediation Patterns

**PR06 – Message Translator (Schema Transformation)**  
- **Use When:** Message format or schema must be adapted between systems with different contracts.  
- **Avoid When:** Translation requires domain decisions or side effects; when it forces shared internal models.  
- **Mandatory Constraints:** Translation MUST occur at integration boundary; contract MUST be explicit and versionable.  
- **Failure Risk:** Structural coupling via shared schemas, breaking changes under evolution, inconsistent mappings.  
- **Agent Enforcement Level:** E0  
- **Cites:** P07, P08, P19, D07, D08  

---

**PR07 – Content Enricher**  
- **Use When:** Additional data is required to complete the target contract and can be sourced reliably without tight coupling.  
- **Avoid When:** Enrichment requires synchronous dependency chains or introduces strong coupling to volatile systems.  
- **Mandatory Constraints:** Enrichment MUST respect trust boundaries; failures MUST be handled explicitly (fallback/park).  
- **Failure Risk:** Latency inflation, cascading failures, inconsistent enrichment results under partial failure.  
- **Agent Enforcement Level:** E1  
- **Cites:** P03, P09, P11, P15, D09  

---

**PR08 – Canonical Data Model (CDM)**  
- **Use When:** Multiple systems must exchange similar concepts and a stable enterprise-level contract is feasible.  
- **Avoid When:** Domains are highly volatile or meanings differ across bounded contexts; when CDM becomes a shared internal model.  
- **Mandatory Constraints:** CDM MUST be treated as an explicit contract, not as internal domain objects; versioning MUST be defined.  
- **Failure Risk:** Organization-wide coupling, semantic drift, slow evolution due to coordinated releases.  
- **Agent Enforcement Level:** E2  
- **Cites:** P02, P08, P19, P03  

---

# 03.5 Reliability Patterns

**PR09 – Retry with Backoff**  
- **Use When:** Failure is transient and classified as technical; operation is safe to repeat.  
- **Avoid When:** Failure is a business error; operation is not idempotent; retries amplify load on failing systems.  
- **Mandatory Constraints:** Retries MUST require idempotency guarantees; max attempts and backoff MUST be defined.  
- **Failure Risk:** Duplicate processing, thundering herd, prolonged outages due to load amplification.  
- **Agent Enforcement Level:** E0  
- **Cites:** P10, P12, P11, D10, D09  

---

**PR10 – Dead Letter Handling (Parking / Quarantine)**  
- **Use When:** Messages cannot be processed after defined retry policy or validation failure requires manual resolution.  
- **Avoid When:** Dead-letter becomes a silent sink without ownership or resolution process.  
- **Mandatory Constraints:** Ownership MUST be defined; message MUST remain traceable; reprocessing MUST be possible.  
- **Failure Risk:** Permanent data loss, operational backlog, hidden failures.  
- **Agent Enforcement Level:** E1  
- **Cites:** P14, P13, P09, P12  

---

**PR11 – Circuit Break / Dependency Isolation**  
- **Use When:** Remote dependency exhibits instability and synchronous calls risk cascading failures.  
- **Avoid When:** Used without fallback or without clear recovery strategy; used to mask persistent design coupling.  
- **Mandatory Constraints:** Timeouts MUST exist; fallback/alternative path MUST be defined; monitoring MUST be in place.  
- **Failure Risk:** False sense of safety, degraded service without visibility, inconsistent states if misapplied.  
- **Agent Enforcement Level:** E1  
- **Cites:** P09, P11, P05, D09  

---

# 03.6 API & Contract Patterns

**PR12 – API Façade**  
- **Use When:** Exposing stable external contract while shielding internal systems from volatility and complexity.  
- **Avoid When:** Façade leaks internal models or becomes a dumping ground for orchestration logic.  
- **Mandatory Constraints:** Contract MUST be minimal; MUST NOT expose internal structures; versioning MUST be defined.  
- **Failure Risk:** Contract drift, tight coupling to internals, breaking changes for consumers.  
- **Agent Enforcement Level:** E1  
- **Cites:** P03, P18, P19, P08  

---

**PR13 – Anti-Corruption Layer (Boundary Shielding)**  
- **Use When:** Integrating with systems that have incompatible models or semantics and must not contaminate your domain.  
- **Avoid When:** Used as an excuse to ignore proper contract design; used without explicit mapping ownership.  
- **Mandatory Constraints:** Mapping rules MUST be explicit; transformations MUST remain at boundary; semantics MUST be documented.  
- **Failure Risk:** Semantic leakage, long-term maintainability issues, inconsistent interpretation of shared concepts.  
- **Agent Enforcement Level:** E1  
- **Cites:** P02, P07, P08, P03  

---

# 03.7 Event Patterns

**PR14 – Event Publication (Domain Event Output)**  
- **Use When:** State changes must be communicated to multiple consumers and consumers can accept eventual consistency.  
- **Avoid When:** Events are used as commands; event meaning is ambiguous; ordering assumptions are implicit.  
- **Mandatory Constraints:** Event schema MUST be explicit and versionable; idempotent consumption MUST be assumed.  
- **Failure Risk:** Consumer coupling to event shape, semantic drift, inconsistent processing due to duplicates or ordering.  
- **Agent Enforcement Level:** E1  
- **Cites:** P04, P10, P20, P19, D04  

---

**PR15 – Event-Carried State Transfer**  
- **Use When:** Consumers need enough state in the event to avoid synchronous lookups during processing.  
- **Avoid When:** Payload becomes excessive or exposes sensitive/irrelevant data.  
- **Mandatory Constraints:** Only necessary data MUST be included; trust boundaries MUST be enforced.  
- **Failure Risk:** Data overexposure, increased coupling to producer schema, privacy/security issues.  
- **Agent Enforcement Level:** E2  
- **Cites:** P15, P18, P17  

---
> *[< Main page](README.md)*

