# Integrations catalogue (shops + carriers)

This document is a lightweight catalogue template to keep integrations visible, owned, and operationally manageable.
It supports ecosystem ownership by making it clear:
- what integrations exist,
- who owns what,
- what the contracts and SLAs are,
- and what commonly breaks.

> Note: This is a fictional catalogue structure for portfolio purposes.

---

## 1) How to use this catalogue
- Keep it updated as integrations are added/changed
- Review it during releases and incident post-mortems
- Use it to drive prioritization (stability work, deprecations, partner change management)

---

## 2) Catalogue table (template)

| Integration | Type | Partner | Direction | Critical flows | Auth | Version | SLA/SLO notes | Owner | Support contact | Monitoring | Known risks |
|---|---|---|---|---|---|---|---|---|---|---|---|
| Shopify Connector | Shop | Shopify | Inbound/Outbound | Orders, cancellations, status updates | OAuth | v1 | p95 order ingest latency target | PO/Eng | Partner portal | Dashboard + alerts | Rate limits, API changes |
| Carrier A | Carrier | Carrier A | Outbound/Inbound | Label create, tracking updates | API key | v2 | Label 5xx threshold | Eng | Carrier ticketing | Partner KPI dashboard | Timeout spikes |
| Carrier B | Carrier | Carrier B | Outbound/Inbound | Label create, manifests | mTLS | v1 | Manifest cutoff time | Eng | Partner manager | Alerts + runbook | Cert expiry |
| Marketplace B | Shop | Marketplace B | Inbound | Orders | Token | v3 | Peak load handling | Eng | Account owner | Latency alerts | Contract drift |

**Directions**
- Inbound = partner pushes to platform (e.g., webhooks)
- Outbound = platform calls partner API (e.g., label create)

---

## 3) Integration record (one-page template)
Use this section to document each integration in a consistent way.

### 3.1 Overview
- **Name:**
- **Partner type:** shop / carrier
- **Business purpose:**
- **Criticality:** high / medium / low
- **Customers/sites impacted:**

### 3.2 Contracts & endpoints
- **Key endpoints/events:**
- **Payload schema versioning:**
- **Idempotency requirements:**
- **Retry/backoff rules:**
- **Rate limit behavior (429):**
- **Error mapping guidelines:**
- **Webhooks:** delivery, signature verification, replay behavior

### 3.3 Operational expectations
- **SLO targets:** success rate, latency p95, freshness for events
- **Dashboards:** links/IDs (if applicable)
- **Alert rules:** thresholds and escalation
- **Runbook:** first triage steps + mitigation options
- **On-call ownership:** who responds and who escalates

### 3.4 Lifecycle management
- **Supported versions:** current + previous
- **Deprecation policy:** how we handle breaking changes
- **Planned changes:** upcoming partner releases or contract shifts
- **Test environment:** sandbox/staging availability and limitations

### 3.5 Known failure modes (examples)
- auth failures (token/cert expiry)
- rate limiting and throttling
- timeouts / 5xx spikes
- schema drift (partner adds/removes fields)
- webhook delivery gaps or duplicates

---

## 4) Common governance rules (suggested)
- All write operations require idempotency keys (see core story US-C01)
- Breaking changes require versioning + migration plan
- Each integration must have:
  - dashboard + alert rules
  - runbook
  - named owner
- Post-incident: update this catalogue with new learnings

---

## 5) Links to related artifacts in this repository
- Core idempotency story: `03-backlog/stories/core/US-C01-idempotency-retry.md`
- Monitoring baseline: `03-backlog/stories/core/US-C02-integration-monitoring.md`
- Phased rollout + hypercare: `03-backlog/stories/core/US-C03-phased-rollout-hypercare.md`
