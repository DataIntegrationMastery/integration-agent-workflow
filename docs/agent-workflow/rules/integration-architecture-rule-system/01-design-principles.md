# 01 – Design Principles

> *[< Main page](README.md)*

---

## Coupling & Boundaries

**P01 – Minimize Cross-System Coupling**  
- **Rule:** Integration design MUST minimize temporal, structural, and transactional coupling between independent systems.  
- **Condition:** Always.  
- **Rationale:** Tight coupling increases failure propagation, limits independent evolution, and reduces system resilience.  
- **Architectural Implication:** Prefer asynchronous communication, avoid shared transactions across boundaries, and prevent direct database or internal model access between systems.  
- **Agent Enforcement Priority:** E0  

---

**P02 – Respect System Boundaries**  
- **Rule:** Integration logic MUST NOT violate bounded context or ownership boundaries.  
- **Condition:** When integrating independently owned systems or domains.  
- **Rationale:** Violating boundaries creates hidden dependencies and undermines domain autonomy.  
- **Architectural Implication:** Use explicit contracts and integration layers; avoid embedding domain logic inside integration components.  
- **Agent Enforcement Priority:** E0  

---

**P03 – Isolate Domain Volatility**  
- **Rule:** Integration architecture MUST isolate frequently changing domains from stable ones.  
- **Condition:** When one system exhibits higher change frequency.  
- **Rationale:** Volatile domains create ripple effects if directly coupled to stable systems.  
- **Architectural Implication:** Introduce mediation, transformation, or façade layers to shield stable systems from change.  
- **Agent Enforcement Priority:** E1  

---

## Communication & Integration Style

**P04 – Prefer Asynchronous Decoupling**  
- **Rule:** Integration design SHOULD prefer asynchronous communication when immediate consistency is not strictly required.  
- **Condition:** When business processes tolerate eventual consistency.  
- **Rationale:** Asynchronous models reduce temporal coupling and improve resilience under partial failure.  
- **Architectural Implication:** Use message-based or event-driven interaction instead of blocking request-response.  
- **Agent Enforcement Priority:** E1  

---

**P05 – Restrict Synchronous Dependencies**  
- **Rule:** Synchronous communication MUST be limited to cases requiring immediate response or strong consistency.  
- **Condition:** When user-facing latency or atomic validation is required.  
- **Rationale:** Synchronous chains amplify latency and propagate failures across systems.  
- **Architectural Implication:** Avoid long synchronous call chains; prevent nested cross-system dependencies.  
- **Agent Enforcement Priority:** E0  

---

**P06 – Separate Orchestration from Domain Logic**  
- **Rule:** Process coordination MUST be implemented in integration components, not embedded inside domain services of other systems.  
- **Condition:** When coordinating multi-system workflows.  
- **Rationale:** Mixing orchestration with domain logic increases coupling and reduces reuse.  
- **Architectural Implication:** Use explicit orchestration or choreography patterns to coordinate flows.  
- **Agent Enforcement Priority:** E1  

---

## Data & Transformation

**P07 – Localize Data Transformation**  
- **Rule:** Data transformation MUST occur at integration boundaries, not within core domain systems.  
- **Condition:** When mapping between heterogeneous schemas or contracts.  
- **Rationale:** Centralizing transformation logic inside domain systems creates cross-domain leakage.  
- **Architectural Implication:** Implement transformation in mediation layers or integration services.  
- **Agent Enforcement Priority:** E1  

---

**P08 – Avoid Shared Data Models Across Systems**  
- **Rule:** Systems MUST NOT share internal data models as integration contracts.  
- **Condition:** Always.  
- **Rationale:** Shared models create hidden structural coupling and hinder independent evolution.  
- **Architectural Implication:** Define explicit contracts or event schemas; prevent reuse of internal domain objects across boundaries.  
- **Agent Enforcement Priority:** E0  

---

## Reliability & Failure Handling

**P09 – Contain Failure Blast Radius**  
- **Rule:** Integration design MUST prevent failures in one system from cascading into unrelated systems.  
- **Condition:** Always.  
- **Rationale:** Uncontained failure propagation reduces system reliability and increases outage scope.  
- **Architectural Implication:** Introduce retry policies, circuit-breaking mechanisms, buffering, and isolation boundaries.  
- **Agent Enforcement Priority:** E0  

---

**P10 – Enforce Idempotent Processing**  
- **Rule:** Integration components MUST support idempotent handling of retried or duplicated messages when asynchronous or unreliable transport is used.  
- **Condition:** When retries, at-least-once delivery, or distributed messaging are present.  
- **Rationale:** Distributed systems cannot guarantee single delivery; duplicate processing causes data inconsistency.  
- **Architectural Implication:** Implement idempotency keys, deduplication strategies, or safe replay mechanisms.  
- **Agent Enforcement Priority:** E0  

---

**P11 – Design for Partial Failure**  
- **Rule:** Integration architecture MUST assume that remote systems can fail independently at any time.  
- **Condition:** Always in distributed environments.  
- **Rationale:** Distributed systems experience network failures, timeouts, and unavailable dependencies.  
- **Architectural Implication:** Implement timeout control, retries with backoff, fallback strategies, and failure detection mechanisms.  
- **Agent Enforcement Priority:** E0  

---

**P12 – Separate Business Errors from Technical Failures**  
- **Rule:** Integration components MUST distinguish between business rule violations and technical transport or infrastructure failures.  
- **Condition:** When processing cross-system requests or events.  
- **Rationale:** Mixing business and technical errors leads to incorrect retries and inconsistent behavior.  
- **Architectural Implication:** Classify errors explicitly and apply retry logic only to technical failures.  
- **Agent Enforcement Priority:** E1  

---

## Observability & Operability

**P13 – Ensure End-to-End Traceability**  
- **Rule:** Integration flows MUST support end-to-end traceability across system boundaries.  
- **Condition:** Always for multi-system flows.  
- **Rationale:** Without traceability, diagnosing failures or performance bottlenecks is unreliable.  
- **Architectural Implication:** Propagate correlation identifiers and ensure consistent logging across integration components.  
- **Agent Enforcement Priority:** E0  

---

**P14 – Make Integration Behavior Observable**  
- **Rule:** Integration components MUST expose operational metrics and processing state.  
- **Condition:** Always in enterprise environments.  
- **Rationale:** Hidden processing prevents monitoring, alerting, and operational response.  
- **Architectural Implication:** Provide metrics for throughput, latency, error rate, and backlog size.  
- **Agent Enforcement Priority:** E1  

---

## Communication & Flow Control

**P15 – Avoid Long Synchronous Call Chains**  
- **Rule:** Integration design MUST NOT create deep synchronous dependency chains across multiple systems.  
- **Condition:** When more than one remote dependency exists in a request path.  
- **Rationale:** Each additional synchronous hop increases latency and amplifies failure probability.  
- **Architectural Implication:** Break chains using asynchronous messaging or intermediate persistence.  
- **Agent Enforcement Priority:** E0  

---

**P16 – Control Backpressure Explicitly**  
- **Rule:** Integration architecture MUST manage load differences between producers and consumers.  
- **Condition:** When systems have unequal throughput or scalability characteristics.  
- **Rationale:** Uncontrolled flow leads to overload, data loss, or cascading failures.  
- **Architectural Implication:** Introduce buffering, throttling, rate limiting, or queue-based decoupling.  
- **Agent Enforcement Priority:** E1  

---

## Security & Trust Boundaries

**P17 – Enforce Explicit Trust Boundaries**  
- **Rule:** Integration design MUST treat every system boundary as a trust boundary.  
- **Condition:** When integrating systems with separate ownership or security domains.  
- **Rationale:** Implicit trust assumptions introduce security vulnerabilities and data leakage risks.  
- **Architectural Implication:** Validate input, authenticate requests, and apply authorization controls at boundaries.  
- **Agent Enforcement Priority:** E0  

---

**P18 – Avoid Overexposure of Internal Capabilities**  
- **Rule:** Integration contracts MUST expose only necessary capabilities and data.  
- **Condition:** When defining APIs or event schemas.  
- **Rationale:** Overexposure increases attack surface and coupling.  
- **Architectural Implication:** Design minimal contracts and avoid leaking internal structures.  
- **Agent Enforcement Priority:** E1  

---

## Change & Evolution

**P19 – Design for Independent Evolution**  
- **Rule:** Integration architecture SHOULD allow systems to evolve independently without synchronized releases.  
- **Condition:** When systems are owned by separate teams.  
- **Rationale:** Coordinated releases increase operational risk and reduce agility.  
- **Architectural Implication:** Use backward-compatible contracts, versioned interfaces, and event schema evolution strategies.  
- **Agent Enforcement Priority:** E1  

---

**P20 – Avoid Hidden Temporal Dependencies**  
- **Rule:** Integration flows MUST NOT depend on implicit execution order across independent systems unless explicitly coordinated.  
- **Condition:** When multiple systems react to shared events or calls.  
- **Rationale:** Implicit ordering assumptions break under concurrency and distributed timing variance.  
- **Architectural Implication:** Define explicit orchestration or idempotent event handling where ordering matters.  
- **Agent Enforcement Priority:** E0  

---

# 01 – Design Principles (continued)

> **Main handbook:** `README.md`

---

## Data & Consistency

**P21 – Avoid Distributed Transactions Across Systems**  
- **Rule:** Integration architecture MUST NOT rely on distributed transactions spanning multiple independent systems.  
- **Condition:** When integrating systems with separate ownership or runtime environments.  
- **Rationale:** Distributed transactions introduce tight coupling, reduce scalability, and create systemic fragility.  
- **Architectural Implication:** Use eventual consistency, compensating actions, or saga-style coordination instead of cross-system atomic commits.  
- **Agent Enforcement Priority:** E0  

---

**P22 – Make Consistency Model Explicit**  
- **Rule:** Integration design MUST explicitly define the consistency model (strong, eventual, or compensating).  
- **Condition:** When data is propagated or synchronized across systems.  
- **Rationale:** Implicit assumptions about consistency lead to incorrect business expectations and hidden defects.  
- **Architectural Implication:** Document and enforce whether operations are synchronous and blocking, event-driven and eventual, or compensation-based.  
- **Agent Enforcement Priority:** E1  

---

**P23 – Preserve Source System Authority**  
- **Rule:** The authoritative source of truth MUST remain clearly defined and unambiguous across integrations.  
- **Condition:** When data is replicated, cached, or transformed across systems.  
- **Rationale:** Ambiguous ownership leads to data conflicts and reconciliation complexity.  
- **Architectural Implication:** Prevent bidirectional uncontrolled updates; define explicit ownership and synchronization direction.  
- **Agent Enforcement Priority:** E0  

---

## Pattern Governance

**P24 – Select Patterns Based on Context, Not Preference**  
- **Rule:** Integration patterns MUST be selected based on contextual requirements rather than developer preference.  
- **Condition:** When choosing routing, transformation, or communication strategies.  
- **Rationale:** Pattern misuse increases complexity and reduces clarity of integration flows.  
- **Architectural Implication:** Evaluate latency tolerance, failure impact, coupling level, and scalability needs before selecting a pattern.  
- **Agent Enforcement Priority:** E1  

---

**P25 – Avoid Over-Engineering Integration Flows**  
- **Rule:** Integration design SHOULD NOT introduce unnecessary mediation, routing complexity, or abstraction layers.  
- **Condition:** When system landscape complexity does not justify advanced patterns.  
- **Rationale:** Over-architecture increases maintenance burden and cognitive load without delivering proportional value.  
- **Architectural Implication:** Prefer the simplest pattern that satisfies coupling, reliability, and scalability requirements.  
- **Agent Enforcement Priority:** E2  

---
> *[< Main page](README.md)*
