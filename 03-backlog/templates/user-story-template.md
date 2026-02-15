# User story template (logistics platform)

Use this template to create clear, testable user stories that work well in integration-heavy, operations-critical environments.

---

## Title
`US-XXX — <short, outcome-focused title>`

---

## Summary
As a `<user/stakeholder>`, I want `<capability>`, so that `<business outcome>`.

---

## Background / Context
- Why this matters now (customer impact, operational pain, incident pattern, roadmap)
- Where this fits in the end-to-end flow (shop → platform → warehouse → carrier)
- Any constraints (cutover windows, partner dependencies, peak season)

---

## Scope
### In scope
- Bullet list of what is included

### Out of scope
- Bullet list of what is explicitly excluded

---

## Assumptions
- Bullet list of assumptions (data availability, partner behavior, site readiness)

---

## Requirements
### Functional requirements
- List of behaviors and flows (happy path + key exceptions)

### Non-functional requirements
- Reliability expectations (retries, idempotency, correctness)
- Performance expectations (latency, throughput, peak behavior)
- Operability expectations (dashboards, alerts, logs)
- Security/compliance notes (PII access, auditability) if relevant

---

## Acceptance Criteria (Given / When / Then)
Write testable criteria, for example:
- **Given** `<precondition>`, **When** `<action>`, **Then** `<expected result>`

Include at least:
- happy path
- key error/exception paths
- operational signals (metrics/logs)

---

## Observability / Monitoring
Minimum expectations:
- Metrics:
  - success/error rates
  - latency (p50/p95)
  - exception rate (if applicable)
- Logging:
  - correlation IDs (order_id, shipment_id, partner_id)
  - traceability for end-to-end flow
- Alerts:
  - stop criteria signals (5xx spikes, freshness gaps, exception spikes)

---

## Rollout / Change management
- Rollout plan: pilot scope → waves
- Hypercare window and daily checkpoint cadence
- Stop criteria (pause rules)
- Rollback / contingency plan

---

## Test approach
### Ring 0 (Lab/Staging)
- What must be validated before pilot

### Pilot validation
- What KPIs must remain stable
- What evidence is required for exit criteria

---

## Dependencies
- Partner documentation/sandbox access
- Warehouse key user availability (UAT)
- Monitoring tooling availability
- Other teams/components

---

## Definition of Done
- All acceptance criteria met
- Tests and evidence captured
- Dashboards/alerts available (if required)
- Runbooks updated (if required)
- Stakeholders informed and handover completed
