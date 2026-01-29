# Modules Directory

The Modules directory contains all runtime subsystems of the Emergency Publishing architecture.  
Each module represents a distinct functional layer, designed to be independently testable, deployable, and extensible.

These modules implement the system’s core capabilities: censorship‑resistant transport, content aggregation, pseudonymous communication, and emergency delivery.

---

## Contents

This directory includes the following modules:

- **vpn-access-layer/**  
  Provides covert access to the system via VPN‑like transports.  
  Supports fallback routing, stealth entry points, protocol‑level obfuscation, and region‑aware transport selection.

- **news-aggregation/**  
  Aggregates external news sources, normalizes content, removes duplicates, and classifies topics.  
  Includes source registry, region‑aware fetching, parsing, deduplication, and classification.

- **anonymous-bbs/**  
  Provides pseudonymous posting with account‑based access control.  
  Users authenticate with lightweight accounts, but all public identity is system‑generated (name + avatar).  
  Supports metadata stripping, moderation, content sanitization, and censorship‑resistant discussion.

- **emergency-channel/**  
  Core subsystem for multi‑protocol, multi‑hop delivery of critical content.  
  Includes routing logic, distribution pipelines, sanitization, secure storage, and fallback transports for emergency publishing.

(Additional modules may be added as the system evolves.)

---

## Design Principles

All modules follow these architectural principles:

- **Modularity**  
  Each module is self‑contained and independently testable.

- **Extensibility**  
  Modules can evolve without breaking system‑wide compatibility.

- **Separation of concerns**  
  Transport, aggregation, publishing, and user communication are clearly separated.

- **Censorship resistance**  
  All modules are designed to operate under hostile network conditions, including active blocking and DPI.

- **Stealth and resilience**  
  Protocols and workflows mimic legitimate traffic and recover gracefully from failure.

---

## Integration

Modules interact via shared interfaces and pipelines:

- **Transport and fallback logic** are provided by `vpn-access-layer/`  
- **News ingestion and processing** are handled by `news-aggregation/`  
- **User‑generated content** is handled by `anonymous-bbs/`  
- **Emergency routing and distribution** are handled by `emergency-channel/`  
- **Content flows** are coordinated through parsing, deduplication, classification, and distribution layers  
- **Storage and retrieval** are managed by each module’s internal storage subsystem  
- **Region‑aware behavior** is supported across all modules

---

## Summary

The Modules directory defines the system’s runtime architecture.  
Each submodule implements a critical capability—transport, aggregation, pseudonymous communication, or emergency delivery—and together they form a resilient, censorship‑resistant communication ecosystem.

This directory is the operational core of the Emergency Publishing system.
