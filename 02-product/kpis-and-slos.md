# KPIs & SLOs (Logistics Platform)

This document defines a practical KPI set for a logistics platform covering:
1) **E-commerce & logistics integrations** (shops + carriers)
2) **Warehouse operations / WMS rollout** (multi-site fulfillment)

> Note: Targets below are illustrative for portfolio purposes. Real thresholds depend on customer SLAs, warehouse size,
> carrier requirements, peak seasonality, and operating model.

---

## 1) KPI principles
Good KPIs should:
- reflect end-to-end outcomes (not only component health)
- be measurable and actionable
- support rollout decisions (go/no-go, pause, rollback)
- work across multiple sites and partners (comparability)

---

## 2) End-to-end KPIs (business flow)

### A) Order-to-ship lead time
**Definition:** time from order creation to shipment confirmed (handed to carrier).  
**Why it matters:** measures fulfillment efficiency and customer experience.

**Example SLO:** p95 within an agreed target per customer/warehouse.

---

### B) Order success rate (happy-path completion)
**Definition:** percentage of orders that complete the expected lifecycle without manual intervention:
created → allocated → picked → packed → shipped → tracking available.  
**Why it matters:** highlights friction, exceptions, and process gaps.

**Example SLO:** maintain ≥ X% happy-path completion over rolling windows.

---

### C) Exception rate
**Definition:** percentage of orders requiring manual resolution (missing stock, label issues, address errors, carrier failures).  
**Why it matters:** directly drives support workload and operational cost.

**Example SLO:** exceptions below a threshold; alert on spikes.

---

## 3) Integration KPIs (shops + carriers)

### D) Integration success rate
**Definition:** ratio of successful integration calls/events vs total attempts (2xx vs 4xx/5xx).  
**Why it matters:** shows stability and partner impact.

**Example SLO:** 5xx below threshold; sustained spikes trigger incident response.

---

### E) Integration latency
**Definition:** time to complete key integration steps (e.g., label creation, tracking update ingestion).  
**Why it matters:** latency issues often surface as operational delays.

**Example SLO:** p95 latency under agreed threshold for critical endpoints.

---

### F) Retry rate and idempotency hit rate
**Retry rate:** how often clients retry due to timeouts/5xx.  
**Idempotency hits:** how often duplicate events are received/handled safely.

**Why it matters:** retries are normal; sudden increases indicate instability or connectivity issues.

**Example SLO:** track as a health signal; alert on abnormal spikes.

---

### G) Partner compatibility incidents
**Definition:** incidents caused by partner API changes, contract mismatches, or deprecated behavior.  
**Why it matters:** common in ecosystem ownership; needs proactive monitoring and versioning.

**Example SLO:** trend down; maintain documented compatibility policy and monitoring.

---

## 4) Warehouse KPIs (WMS/fulfillment)

### H) Pick accuracy / inventory mismatch rate
**Definition:** discrepancy between expected and actual pick/pack outcomes, plus inventory sync mismatches.  
**Why it matters:** affects returns, customer trust, and cost.

**Example SLO:** mismatch rate below threshold; investigate spikes by SKU/zone/shift.

---

### I) Throughput (units/orders per hour)
**Definition:** processing rate per site and per workflow stage.  
**Why it matters:** capacity planning and operational performance.

**Example SLO:** targets agreed by warehouse type; track trend and degradation.

---

### J) Scan success rate (if scanning is used)
**Definition:** successful scans / total scans at receiving, picking, packing.  
**Why it matters:** highlights usability and operational friction.

**Example SLO:** maintain ≥ X% scan success; investigate top failure reasons.

---

## 5) Operational KPIs (stability & recovery)

### K) Incident count and severity trend
**Definition:** number of incidents by severity over time, tagged to integrations/sites/releases.  
**Why it matters:** real-world stability signal.

**Example SLO:** no increase in Sev1/Sev2 after rollout; trend down over time.

---

### L) MTTR (Mean Time To Restore/Repair)
**Definition:** average time to restore service after an incident.  
**Why it matters:** affects SLA and operational cost.

**Example SLO:** MTTR decreases over time; set targets per incident type.

---

### M) Rollback rate (during rollouts)
**Definition:** percentage of sites/integrations requiring rollback after activation.  
**Why it matters:** rollout safety and release quality.

**Example SLO:** below threshold; pause rollout if exceeded.

---

## 6) How KPIs are used in rollout decisions
During phased rollout (new warehouse or integration change), define:
- baseline KPI values (pre-change)
- observation window (hypercare)
- stop criteria (pause/rollback triggers)

Typical stop criteria:
- integration 5xx spikes sustained
- exception rate jumps
- order-to-ship p95 degrades beyond threshold
- incident severity increases

---

## 7) Measurement requirements (minimum)
To measure these KPIs, ensure:
- requests/events have correlation IDs (order_id, shipment_id, integration partner ID)
- logs and metrics are tagged by site/warehouse, integration, release version
- dashboards provide cohort views (pilot vs wider rollout)
- alert rules reflect real impact (avoid noise)

---

## 8) Backlog examples derived from KPI gaps
If KPIs degrade, common backlog items include:
- stronger idempotency and retry handling
- contract versioning and partner change monitoring
- workflow UX improvements to reduce exception rate
- automation to reduce manual steps
- rollout pause rules and safer rollback mechanisms
