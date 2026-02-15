# US-C01 — Integration reliability: idempotency and retry-safe behavior

## Summary
As a platform, I want integration endpoints and event processing to be idempotent and retry-safe, so that temporary failures
(timeouts, intermittent partner issues, network instability) do not create duplicate shipments, duplicate labels, or inconsistent order states.

## Background / Context
In logistics integrations, retries are normal:
- partners may time out,
- carrier APIs may intermittently fail,
- webhooks may be delivered more than once.

Without idempotency, retries can cause serious issues such as:
- duplicate shipment creation,
- duplicate labels,
- double billing events,
- inconsistent tracking states.

This story defines the minimum contract and behaviors required to keep integrations reliable under real-world conditions.

---

## Scope
### In scope
- Idempotency behavior for key integration actions (e.g., shipment/label creation)
- Retry guidance and consistent error responses
- Duplicate event handling for inbound webhooks
- Minimum observability requirements (logs/metrics)

### Out of scope
- Full partner-specific connector implementation details
- UI design for dashboards (requirements only)
- Downstream analytics design

---

## Proposed patterns (simplified)

### A) Idempotency key for write operations
For operations that create or mutate state (e.g., create shipment/label), require an idempotency key:
- Header: `Idempotency-Key: <client_generated_key>`
- or a payload field: `idempotency_key`

**Rule:** repeated requests with the same idempotency key must not create duplicates.

### B) Event uniqueness for inbound webhooks
For inbound tracking/webhook events, require a unique event identifier:
- `event_id` or a deterministic hash of event content + timestamp

**Rule:** repeated delivery of the same event must not create duplicate processing.

### C) Consistent error handling and retry guidance
- transient errors return 5xx (e.g., 503) → client retries with backoff
- rate limiting returns 429 → client respects retry guidance (e.g., Retry-After)
- validation errors return 400 → client does not retry without fixing

---

## Acceptance Criteria

### A) Idempotent write operations (shipment/label creation)
1. A “create shipment” request includes a client-generated idempotency key.
2. If the same request is repeated with the same idempotency key:
   - the platform returns the same result (same shipment_id/label_id), and
   - no duplicate shipment/label is created.
3. Idempotency is guaranteed across common failure scenarios:
   - request timeout on client side
   - client retries after 5xx
   - client retries after reconnect

### B) Duplicate-safe webhook/event ingestion
4. Inbound webhook payloads include an `event_id` (or equivalent unique identifier).
5. If the same webhook/event is received multiple times:
   - it is processed once,
   - duplicates are acknowledged without reprocessing.

### C) Error responses and retry rules
6. For invalid payloads, the API returns 400 with:
   - error code
   - message
   - field-level validation details
7. For transient backend failures, API returns 5xx and clients are expected to retry with exponential backoff.
8. For rate limiting, API returns 429 and provides retry guidance.

### D) Observability (minimum)
9. Logs are searchable by:
   - order_id
   - shipment_id
   - idempotency key / event_id
   - partner/integration identifier
10. Metrics exist for:
   - success rate (2xx)
   - error rates (4xx/5xx/429)
   - duplicate rate (idempotency hits / duplicate events)
   - latency distribution (p50/p95)

---

## Non-functional Requirements
- **Correctness:** prevent duplicate shipments/labels/events from retries
- **Performance:** handle reconnect bursts or webhook spikes without collapsing
- **Operability:** provide logs/metrics for fast incident triage
- **Compatibility:** contract changes are versioned and backward compatible where possible

---

## Test Approach
### Ring 0 (Lab/Staging)
- Send create shipment request → verify shipment created once
- Repeat same request with same idempotency key → verify same shipment_id returned
- Simulate timeout and retry → verify no duplicates
- Send duplicate webhook events → verify processed once
- Validate metrics/logs include order_id + idempotency keys

### Pilot validation
- Monitor duplicate rate and 5xx/429 rates
- Confirm retry behavior does not produce duplicate business outcomes

---

## Dependencies
- Agreement on idempotency key generation rules with clients/connectors
- Logging/metrics tooling for correlation and dashboards
- Partner connector behavior conforms to retry guidance

---

## Definition of Done
- Idempotency and duplicate handling implemented and tested
- Error handling and retry rules documented for integrators
- Observability requirements delivered (logs + metrics)
- Stakeholders aligned on contract and behavior
