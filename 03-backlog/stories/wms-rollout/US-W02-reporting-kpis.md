# US-W02 — Warehouse reporting & KPI dashboard setup (operational visibility)

## Summary
As a warehouse lead and operations/support team, I want clear reporting and KPI dashboards for each warehouse site,
so that we can monitor performance, detect issues early, and continuously improve fulfillment operations.

## Background / Context
Without reliable reporting, teams operate “blind”:
- bottlenecks are found late,
- exceptions become normal,
- site-to-site comparisons are difficult,
- incident triage takes longer.

This story establishes a minimum reporting baseline for warehouse operations and rollout readiness.

---

## Scope
### In scope
- KPI definitions and ownership (what is measured, how, by whom)
- Site-level dashboards for workflow stages
- Exception reporting and root-cause categorization
- Operational alerts for meaningful degradation
- Rollout-ready reporting pack for a new site

### Out of scope
- Full BI/advanced analytics platform design
- Complex forecasting models
- Customer-facing reporting UI (unless required separately)

---

## Acceptance Criteria

### A) KPI definitions (minimum)
1. KPI definitions are documented with:
   - name
   - definition and formula
   - data source
   - update frequency
   - owner
2. Minimum KPI set includes:
   - order-to-ship lead time (p50/p95)
   - throughput (orders/units per hour)
   - exception rate (manual interventions)
   - pick/pack accuracy (or mismatch/defect rate)
   - scan success rate (if scanning is used)

### B) Site dashboards (minimum)
3. A dashboard exists per warehouse site showing:
   - inbound receiving volume and cycle time
   - pick queue/backlog and cycle time
   - pack station throughput and cycle time
   - shipments created and shipped
4. Dashboards support filtering by:
   - date/time window
   - shift (if available)
   - workflow stage
   - customer/channel (if multi-tenant)

### C) Exception reporting
5. An exception report exists that categorizes top reasons, e.g.:
   - missing stock / inventory mismatch
   - label creation failure
   - address validation failure
   - carrier service unavailable / rate limiting
   - operational blockers (e.g., missing packaging)
6. Exceptions are linkable to relevant entities:
   - order_id
   - shipment_id
   - site/warehouse
   - integration partner (if relevant)

### D) Operational alerts (actionable)
7. Alerts exist for sustained degradation, e.g.:
   - throughput drops below threshold for N minutes
   - exception rate spikes above threshold
   - pick queue/backlog grows beyond threshold
   - label creation error rate spikes
8. Alerts provide context:
   - impacted site and workflow stage
   - time window and trend
   - link to dashboards
   - recommended first triage steps (runbook link)

### E) Rollout readiness pack for new sites
9. A “new site reporting pack” exists and includes:
   - baseline KPI targets for the observation window (hypercare)
   - links to dashboards and alert rules
   - support responsibilities and escalation path
10. During pilot go-live, reporting is reviewed daily with warehouse leads.

---

## Non-functional Requirements
- **Accuracy:** KPIs reflect real operations; definitions are consistent across sites
- **Timeliness:** dashboards update frequently enough for operations (near-real-time where needed)
- **Operability:** support can use dashboards for triage without deep engineering involvement
- **Security:** role-based access to operational and customer data

---

## Test Approach
### Ring 0 / Staging
- Validate KPI calculations with sample datasets
- Confirm dashboards show expected filters and drill-downs
- Simulate exception spikes and confirm alert behavior

### Pilot / Go-live
- Verify dashboards reflect live activity during a shift
- Validate daily checkpoint reviews and operational adoption

---

## Dependencies
- Event/metrics pipeline availability (workflow events emitted)
- Consistent identifiers across systems (order_id, shipment_id, site_id)
- Dashboard tooling and alerting platform
- Agreement on KPI definitions with ops leadership

---

## Definition of Done
- KPI definitions documented and agreed with stakeholders
- Site dashboards and exception reports available for ops
- Alerts configured with low-noise thresholds
- Reporting pack used during pilot hypercare and refined based on feedback
