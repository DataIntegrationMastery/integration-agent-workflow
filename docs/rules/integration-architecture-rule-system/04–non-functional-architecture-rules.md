# 04 – Non-Functional Architecture Rules

> *[< Main page](README.md)*

---

## 04.0 Purpose

This file defines mandatory non-functional architecture rules for integration systems.  
These rules govern reliability, scalability, observability, security, and operational resilience.

All rules MUST align with:

- `01-design-principles.md`
- `02-decision-model.md`
- `03-pattern-selection-rules.md`

---

## 04.1 Rule Object Model

Each rule MUST follow this structure:

- **NFR ID**
- **Category**
- **Rule**
- **Condition**
- **Mandatory Controls**
- **Failure Risk**
- **Agent Enforcement Level**
- **Cites**

### 04.1.1 Template

**NFR{NN} – {Title}**  
- **Category:** {Reliability | Scalability | Observability | Security | Resilience}  
- **Rule:** {Normative statement using MUST/SHOULD/MUST NOT}  
- **Condition:** {When applicable}  
- **Mandatory Controls:** {Required mechanisms}  
- **Failure Risk:** {Primary risk if violated}  
- **Agent Enforcement Level:** {E0 | E1 | E2 | E3}  
- **Cites:** {Pxx[, Dyy[, PRzz]]}

---

# 04.2 Reliability

**NFR01 – Timeout Enforcement**  
- **Category:** Reliability  
- **Rule:** All remote calls MUST define explicit timeout limits.  
- **Condition:** When synchronous or request-based communication is used.  
- **Mandatory Controls:** Configurable timeout; failure classification; monitoring of timeout rates.  
- **Failure Risk:** Thread exhaustion, cascading failures, system-wide degradation.  
- **Agent Enforcement Level:** E0  
- **Cites:** P11, P09, D09  

---

**NFR02 – Retry Governance**  
- **Category:** Reliability  
- **Rule:** Retry behavior MUST be explicitly defined and bounded.  
- **Condition:** When technical failures are retried.  
- **Mandatory Controls:** Max attempts; exponential backoff; idempotency verification.  
- **Failure Risk:** Load amplification, duplicate processing, prolonged outages.  
- **Agent Enforcement Level:** E0  
- **Cites:** P10, PR09, D10  

---

**NFR03 – Failure Isolation**  
- **Category:** Reliability  
- **Rule:** Integration components MUST prevent failure propagation across unrelated systems.  
- **Condition:** Always in distributed environments.  
- **Mandatory Controls:** Circuit isolation; buffering; decoupling boundaries.  
- **Failure Risk:** System-wide outage, dependency chain collapse.  
- **Agent Enforcement Level:** E0  
- **Cites:** P09, P15, PR11  

---

# 04.3 Scalability

**NFR04 – Horizontal Scalability Capability**  
- **Category:** Scalability  
- **Rule:** Integration components SHOULD support horizontal scaling when throughput is variable.  
- **Condition:** When load variability or growth is expected.  
- **Mandatory Controls:** Stateless processing or controlled state; externalized session/context storage.  
- **Failure Risk:** Throughput bottlenecks, degraded responsiveness under load.  
- **Agent Enforcement Level:** E1  
- **Cites:** P16, D11  

---

**NFR05 – Backpressure Management**  
- **Category:** Scalability  
- **Rule:** Flow control MUST be implemented between producer and consumer components.  
- **Condition:** When throughput mismatch is possible.  
- **Mandatory Controls:** Rate limiting, queue depth monitoring, throttling mechanisms.  
- **Failure Risk:** Consumer overload, memory exhaustion, message loss.  
- **Agent Enforcement Level:** E0  
- **Cites:** P16, D11  

---

# 04.4 Observability

**NFR06 – Correlation Propagation**  
- **Category:** Observability  
- **Rule:** End-to-end correlation identifiers MUST be propagated across system boundaries.  
- **Condition:** When a request or event spans multiple systems.  
- **Mandatory Controls:** Unique correlation ID; log inclusion; trace continuity.  
- **Failure Risk:** Inability to diagnose cross-system failures.  
- **Agent Enforcement Level:** E0  
- **Cites:** P13, D13  

---

**NFR07 – Metrics Exposure**  
- **Category:** Observability  
- **Rule:** Integration components MUST expose operational metrics.  
- **Condition:** Always in enterprise environments.  
- **Mandatory Controls:** Throughput, latency, error rate, backlog size metrics.  
- **Failure Risk:** Undetected degradation, delayed incident response.  
- **Agent Enforcement Level:** E1  
- **Cites:** P14, D14  

---

# 04.5 Security

**NFR08 – Authentication Enforcement**  
- **Category:** Security  
- **Rule:** All cross-boundary communication MUST be authenticated.  
- **Condition:** When crossing trust boundaries.  
- **Mandatory Controls:** Strong authentication mechanism; credential management.  
- **Failure Risk:** Unauthorized access, data compromise.  
- **Agent Enforcement Level:** E0  
- **Cites:** P17  

---

**NFR09 – Authorization Enforcement**  
- **Category:** Security  
- **Rule:** Access rights MUST be validated for each integration interaction.  
- **Condition:** When data or capabilities are exposed externally.  
- **Mandatory Controls:** Role or policy-based access checks; least privilege principle.  
- **Failure Risk:** Privilege escalation, sensitive data exposure.  
- **Agent Enforcement Level:** E0  
- **Cites:** P17, P18  

---

**NFR10 – Data Minimization**  
- **Category:** Security  
- **Rule:** Integration contracts MUST expose only required data attributes.  
- **Condition:** When defining APIs or events.  
- **Mandatory Controls:** Contract review; schema validation; field-level control.  
- **Failure Risk:** Increased attack surface, privacy violation, unnecessary coupling.  
- **Agent Enforcement Level:** E1  
- **Cites:** P18, PR15  

---

# 04.6 Resilience & Recovery

**NFR11 – Replay Capability**  
- **Category:** Resilience  
- **Rule:** Integration systems SHOULD support safe replay of messages or events.  
- **Condition:** When asynchronous processing or eventual consistency is used.  
- **Mandatory Controls:** Idempotency; replay logging; audit trail.  
- **Failure Risk:** Permanent data loss, inability to recover from partial outages.  
- **Agent Enforcement Level:** E1  
- **Cites:** P10, P09  

---

**NFR12 – Operational Ownership Definition**  
- **Category:** Resilience  
- **Rule:** Each integration flow MUST have a clearly defined operational owner.  
- **Condition:** Always in multi-team environments.  
- **Mandatory Controls:** Responsibility assignment; escalation path; documented support model.  
- **Failure Risk:** Unresolved incidents, accountability gaps.  
- **Agent Enforcement Level:** E1  
- **Cites:** P02, PR10  

---

> *[< Main page](README.md)*