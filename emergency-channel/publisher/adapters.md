# Publisher Module — Delivery Adapters

## Overview

Delivery adapters are the execution layer of the Publisher module.  
While channels define *where* content should be delivered, adapters define *how* the delivery actually happens.  
Each adapter implements a consistent interface, allowing the Publisher pipeline to send content through diverse mechanisms without knowing the underlying protocol.

Adapters are modular, isolated, and replaceable.  
Deployments may enable, disable, or override adapters based on operational needs.

---

## Adapter Responsibilities

Each delivery adapter is responsible for:

- **Payload transmission**  
  Sending formatted content to an external endpoint.

- **Protocol handling**  
  Managing HTTP, email, IPFS, P2P, or other transport mechanisms.

- **Error reporting**  
  Returning structured errors to the fallback subsystem.

- **Health signaling**  
  Providing status information for channel health checks.

- **Security enforcement**  
  Ensuring TLS, signing, or encryption requirements are met.

Adapters do *not* modify content; they only deliver it.

---

## Standard Adapter Types

The Publisher module includes several built‑in adapters:

### 1. HTTP Adapter
Used for:
- HTML publishing
- JSON API endpoints
- RSS feed updates

Features:
- Supports GET/POST/PUT
- Handles redirects and retries
- Enforces TLS and certificate validation

---

### 2. Email Adapter
Used for:
- Digest emails
- Alert notifications

Features:
- SMTP or API‑based delivery
- Optional signing or encryption
- Batched or single‑message modes

---

### 3. IPFS Adapter
Used for:
- Decentralized publishing
- Mirror‑resistant content replication

Features:
- Pinning support
- Gateway fallback
- Optional local node integration

---

### 4. P2P Adapter
Used for:
- Opportunistic delivery
- High‑risk or unstable networks

Features:
- Peer discovery
- Store‑and‑forward behavior
- Delay‑tolerant operation

---

### 5. Offline Bundle Adapter
Used for:
- Air‑gapped environments
- High‑risk distribution scenarios

Features:
- Generates portable bundles
- Compatible with USB, SD, QR, or local transfer
- No network dependency

---

### 6. Micro‑Feed Adapter
Used for:
- Extreme censorship environments
- Minimal text‑only feeds

Features:
- Very small payloads
- Highly resilient to filtering
- Works with fallback micro‑channels

---

## Adapter Interface

All adapters implement a common interface:

- `prepare(payload)`  
  Validates and prepares the payload.

- `deliver(payload)`  
  Sends the payload to the target endpoint.

- `status()`  
  Returns health information for monitoring.

- `capabilities()`  
  Declares supported features (e.g., retries, encryption).

This interface ensures that new adapters can be added without modifying the Publisher pipeline.

---

## Extending the Adapter System

To add a new adapter, a deployment must provide:

1. A delivery implementation  
2. A configuration schema  
3. Optional health‑check logic  
4. Optional fallback rules  

Adapters can be registered dynamically, allowing deployments to evolve over time.

---

## Summary

The adapter subsystem provides:

- A unified interface for all delivery mechanisms  
- Modular, replaceable components  
- Strong separation between formatting and delivery  
- Built‑in support for decentralized and offline channels  
- Extensibility for future protocols and environments  

Adapters are the backbone of the Publisher module’s delivery capabilities.
