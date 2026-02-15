# System context (high-level)

This document provides a simplified system context for a logistics platform that connects:
- shops (order sources),
- the logistics platform (core services and integrations),
- warehouse operations (WMS/fulfillment),
- carriers (shipping and tracking),
- and customer-facing visibility (status updates, support).

> Note: This is a fictional context diagram for portfolio purposes.

---

## 1) Context diagram

```mermaid
flowchart LR
  subgraph SH[Shops / Sales Channels]
    S1[Shop Platform A]
    S2[Marketplace B]
    S3[Custom Webshop]
  end

  subgraph LP[Logistics Platform]
    P1[Order Ingestion]
    P2[Orchestration / Business Rules]
    P3[Integration Layer<br/>APIs, Webhooks, Connectors]
    P4[Observability<br/>Metrics, Logs, Alerts]
    P5[Billing / Reporting]
  end

  subgraph WH[Warehouse Operations]
    W1[WMS / Warehouse App]
    W2[Operators<br/>Receive / Pick / Pack / Returns]
  end

  subgraph CA[Carriers]
    C1[Carrier API A<br/>Label + Tracking]
    C2[Carrier API B<br/>Label + Tracking]
    C3[Carrier API C<br/>Manifests]
  end

  subgraph CX[Customer Experience]
    X1[Tracking Page]
    X2[Customer Support]
    X3[Notifications<br/>Email/SMS]
  end

  SH -->|Orders, Updates| P1
  P1 --> P2
  P2 --> P3
  P3 -->|Create shipment / label| CA
  CA -->|Tracking events| P3

  P2 -->|Fulfillment tasks| W1
  W2 --- W1

  P2 -->|Status updates| CX
  P5 --- CX

  P4 --- LP
  P4 --- WH
  P4 --- CA
