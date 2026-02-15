# UAT checklist — Warehouse flows (receiving → picking → packing → returns)

This checklist supports User Acceptance Testing (UAT) for a warehouse site rollout.
It is designed to validate that core warehouse workflows work end-to-end and that key users can operate the system reliably.

> Note: This is a fictional UAT checklist for portfolio purposes.

---

## 1) UAT objectives
- Validate the “golden path” order flow end-to-end
- Validate common exceptions and operational edge cases
- Confirm usability for warehouse operators and supervisors
- Confirm operational readiness (supportability + reporting signals)
- Produce evidence for go/no-go decisions

---

## 2) Participants (typical)
- Warehouse key users (operators + supervisors)
- Operations/support representative
- Product Owner / Delivery representative
- Engineering/QA support on standby (as needed)

---

## 3) Preconditions (before starting UAT)
- Site configuration completed (locations/zones/bins, workflows)
- Master data loaded (SKUs/barcodes, packaging, carrier options)
- User roles/permissions configured
- Integrations available in UAT environment (or controlled production pilot)
- Reporting dashboards accessible (at least basic operational KPIs)
- Issue log template prepared (severity, owner, workaround, next step)

---

## 4) Core test scenarios (minimum set)

### A) Receiving (inbound)
- [ ] Create inbound receipt (ASN or manual) for a sample SKU set
- [ ] Putaway to correct location(s)
- [ ] Handle barcode mismatch (wrong SKU scanned)
- [ ] Handle damaged goods / quarantine flow (if applicable)
- [ ] Verify inventory updates correctly after receiving

**Evidence:** screenshots/logs of successful receipt + inventory state.

---

### B) Picking
- [ ] Generate pick tasks from new orders
- [ ] Pick single-order flow (one order, few items)
- [ ] Pick multi-item order (multiple lines)
- [ ] Handle out-of-stock during pick (substitution/backorder/cancel rule)
- [ ] Verify pick confirmation updates order state

**Evidence:** pick task list + completion confirmation.

---

### C) Packing
- [ ] Pack a picked order and confirm package contents
- [ ] Validate packaging selection rules (if applicable)
- [ ] Handle “wrong item scanned at packing”
- [ ] Handle split shipment (partial pack) if supported
- [ ] Verify packing confirmation updates order state

**Evidence:** pack confirmation record.

---

### D) Shipping + label creation (carrier integration)
- [ ] Create shipping label successfully (happy path)
- [ ] Verify label artifact accessible/printable
- [ ] Confirm shipment is created with the carrier ID
- [ ] Simulate transient carrier failure (timeout/5xx) and retry safely (no duplicates)
- [ ] Confirm shipped status is reflected in platform/customer view

**Evidence:** label, shipment_id, and order status transitions.

---

### E) Tracking visibility (inbound events)
- [ ] Verify tracking events are ingested and visible (webhook/poll)
- [ ] Verify duplicates do not cause duplicate status transitions
- [ ] Verify “freshness” signal (last tracking event age) is visible on dashboard

**Evidence:** tracking event timeline.

---

### F) Returns (basic)
- [ ] Initiate return (RMA or manual)
- [ ] Receive returned item and validate condition handling
- [ ] Restock or quarantine outcome recorded
- [ ] Verify inventory and order state updates

**Evidence:** return receipt confirmation.

---

## 5) Operational readiness checks (during UAT)
- [ ] Support can locate an order by order_id and trace its lifecycle
- [ ] Dashboards show the site’s key signals (volume, exceptions, label success rate)
- [ ] Alerts are configured (at least basic thresholds) for:
  - label creation failures
  - exception rate spike
  - integration 5xx/latency spikes
- [ ] Runbook links and escalation contacts are available

---

## 6) Defects & issue management
For each issue found, capture:
- description + steps to reproduce
- severity (Blocker / High / Medium / Low)
- impacted flow (receiving/picking/packing/shipping/returns)
- workaround (if any)
- owner (team/individual)
- target resolution timing (before go-live vs post go-live backlog)

---

## 7) Go/No-Go criteria (simple)
### Go (examples)
- All **Blocker** issues resolved
- No unresolved **High** issues in core shipping flow
- End-to-end “golden path” works reliably
- Warehouse key users confirm usability for daily ops
- Monitoring dashboards available for hypercare

### No-Go (examples)
- Shipping/label flow unreliable or creates duplicates
- Inventory updates incorrect or inconsistent
- Exception handling not workable without constant manual fixes
- Support cannot triage issues with available tooling

---

## 8) UAT completion output (what you produce)
- UAT pass/fail summary
- Issue log with final statuses
- Evidence pack (screenshots/log references)
- Recommendations for go-live + hypercare focus areas
