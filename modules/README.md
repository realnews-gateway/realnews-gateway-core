# Modules Directory

The Modules directory contains all runtime subsystems of the Emergency Publishing architecture.  
Each module represents a distinct functional layer, designed to be independently testable, deployable, and extensible.

These modules implement the system’s core capabilities: censorship‑resistant transport, content aggregation, anonymous publishing, and emergency delivery.

---

## Contents

This directory includes the following modules:

- **vpn-access-layer/**  
  Provides covert access to the system via VPN‑like transports.  
  Supports fallback routing, stealth entry points, and protocol‑level obfuscation.

- **news-aggregation/**  
  Aggregates external news sources, filters content, and prepares it for publishing.  
  Includes source normalization, deduplication, and metadata tagging.

- **anonymous-bbs/**  
  Enables anonymous posting, multi‑hop message relay, and censorship‑resistant discussion.  
  Supports moderation, identity obfuscation, and content sanitization.

- **emergency-publishing/**  
  Core subsystem for multi‑protocol, multi‑hop delivery of critical content.  
  Includes transport protocols, routing logic, distribution, publishing, sanitization, and secure storage.

(Additional modules may be added as the system evolves.)

---

## Design Principles

All modules follow these architectural principles:

- **Modularity**  
  Each module is self‑contained and independently testable.

- **Extensibility**  
  Modules can evolve without breaking system‑wide compatibility.

- **Separation of concerns**  
  Transport, aggregation, publishing, and access are clearly separated.

- **Censorship resistance**  
  All modules are designed to operate under hostile network conditions.

- **Stealth and resilience**  
  Protocols and workflows mimic legitimate traffic and recover from failure.

---

## Integration

Modules interact via shared interfaces and pipelines:

- **Transport protocols** are defined in `emergency-publishing/protocols/`  
- **Routing and distribution** are handled by `emergency-publishing/router/` and `distributor/`  
- **Content flows** are coordinated via `pipeline/` and `sanitizer/`  
- **Storage and retrieval** are managed by `storage/`  
- **External access** is provided by `vpn-access-layer/`  
- **User‑generated content** is handled by `anonymous-bbs/`  
- **News ingestion** is handled by `news-aggregation/`

---

## Summary

The Modules directory defines the system’s runtime architecture.  
Each submodule implements a critical capability—transport, aggregation, publishing, or access—and together they form a resilient, censorship‑resistant communication ecosystem.

This directory is the operational core of the Emergency Publishing system.
