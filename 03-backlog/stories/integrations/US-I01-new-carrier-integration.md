# US-I01 — Add a new carrier integration (label + tracking)

## Summary
As a logistics platform, I want to integrate a new carrier for label creation and tracking updates, so that customers can ship
with this carrier using the standard workflow and get reliable status visibility.

## Background / Context
Adding a carrier is more than building endpoints:
- contract and field mapping must be stable,
- reliability (timeouts, retries, rate limits) must be handled,
- operations needs monitoring and runbooks,
- rollout must be phased to reduce risk.

---

## Scope
### In scope
- Carrier contract mapping for:
  - shipment creation / label generation
  - tracking updates ingestion (webhook/polling)
  - basic error handling mapping
- Idempotency and retry-safe behavior for label creation
- Monitoring dashboards and alerts for the carrier integration
- Rollout plan (pilot customers/sites → wider rollout)
- Operational runbook and escalation path

### Out of scope
- Commercial negotiation with carrier
- Customer-specific customizations beyond supported options
- Advanced carrier services (e.g., special insurance flows) unless prioritized separately

---

## Acceptance Criteria

### A) Functional coverage (minimum)
1. Platform supports creating a shipment and generating a label with the new carrier.
2. Label generation returns:
   - carrier shipment identifier
   - label artifact reference (URL/object key) or payload
   - error responses with clear codes
3. Platform ingests tracking updates for the new carrier:
   - via webhook if available, otherwise via scheduled polling
4. Tracking updates are mapped into the platform’s normalized status model.

### B) Reliability and safety
5. Label creation is idempotent:
   - repeated requests with the same idempotency key return the same shipment/label result
   - no duplicate carrier shipments/labels are created
6. Retry rules are defined and documented:
   - transient failures (timeouts/5xx) → retry with exponential backoff
   - rate limits (429) → respect Retry-After or documented backoff
7. Webhook/event ingestion is duplicate-safe:
   - duplicate events do not create duplicate state transitions

### C) Monitoring and operability
8. Carrier-specific monitoring exists:
   - success/error rate (2xx/4xx/5xx/429)
   - latency p95
   - webhook freshness (age of last event)
9. Alert rules exist for sustained degradation and include:
   - impacted carrier + endpoints
   - recommended first triage steps
   - escalation contact and mitigation options
10. A runbook exists describing:
   - common failure modes (auth failures, rate limits, timeouts)
   - mitigation steps (pause rollout, throttling, retries)
   - when/how to open carrier support tickets

### D) Testing and acceptance
11. A carrier sandbox or test environment is used (if available), with:
   - successful label generation tests
   - error-path tests (invalid address, rate limit, auth failure)
12. End-to-end test order flow validated:
   - order → shipment → label → warehouse pack → tracking visible
13. Stakeholders sign off on pilot readiness (Ops + CS + Engineering).

### E) Rollout plan and phased activation
14. A phased rollout is planned:
   - pilot customers/sites
   - observation window with hypercare
   - exit criteria before wider rollout
15. Stop criteria and rollback options are defined:
   - pause if 5xx spikes, exception rate spike, Sev1 incident
   - disable carrier for new shipments (fallback to other carriers) if needed

---

## Non-functional Requirements
- **Reliability:** stable performance under peak label demand
- **Operability:** diagnostics and monitoring enable fast triage
- **Security:** credentials managed securely; access controlled and auditable
- **Compatibility:** contract changes are versioned and tracked

---

## Test Approach
### Ring 0 (Lab/Staging)
- Label create success path
- Timeout + retry path (confirm idempotency)
- 429 handling path (confirm backoff)
- Webhook duplicate event handling
- Tracking update mapping correctness

### Pilot validation
- Monitor carrier KPIs during hypercare window
- Validate support can triage without engineering escalation for common issues

---

## Dependencies
- Carrier technical documentation and sandbox access
- Credential provisioning and rotation plan
- Normalized tracking status mapping model
- Monitoring and alerting platform

---

## Definition of Done
- Carrier integration passes Ring 0 tests and pilot readiness checks
- Monitoring, alerts, and runbook in place
- Phased rollout completed with KPI evidence and stakeholder sign-off
