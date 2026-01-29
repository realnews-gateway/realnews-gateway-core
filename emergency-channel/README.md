# Emergency Channel Module

The Emergency Channel is the system’s core module for censorship‑resistant publishing and multi‑protocol communication.  
It provides a resilient, multi‑hop, multi‑protocol pipeline for delivering critical content under hostile network conditions.

This module integrates transport protocols, routing logic, distribution mechanisms, publishing workflows, sanitization, and storage into a unified subsystem.

---

## Purpose

The Emergency Channel enables:

- Reliable content delivery during censorship events  
- Multi‑path routing across diverse network environments  
- Stealth communication that mimics legitimate traffic  
- Automatic fallback when protocols or paths are blocked  
- Sanitization and transformation of incoming content  
- Secure storage and retrieval of published material  

It is the backbone of the system’s emergency publishing capability.

---

## Directory Structure

This module contains the following subdirectories:

- **core/**  
  Core logic, shared interfaces, and system‑level coordination.

- **protocols/**  
  Transport layer protocols (XHTTP, XTLS, Reality, Hysteria2, VLESS, Trojan), including selection, integration, and security.

- **router/**  
  Multi‑hop routing, session migration, and protocol probing.

- **distributor/**  
  Content distribution across diverse paths and regions.

- **publisher/**  
  Client‑facing delivery, fallback access channels, and publishing logic.

- **pipeline/**  
  End‑to‑end flow for message ingestion, transformation, and delivery.

- **sanitizer/**  
  Content sanitization, metadata stripping, and format normalization.

- **storage/**  
  Secure, censorship‑resistant storage of published content.

- **security/**  
  Threat modeling, trust boundaries, and protocol‑level defenses.

- **interface.md**  
  Unified interface definitions for transport, routing, and publishing layers.

---

## Key Features

- **Multi‑protocol transport**  
  Six complementary protocols with dynamic selection and fallback.

- **Multi‑hop routing**  
  Each hop may use a different protocol for maximum resilience.

- **Stealth and camouflage**  
  Real‑site mimicry, TLS camouflage, and HTTP‑based obfuscation.

- **Content sanitization**  
  Removes sensitive metadata and normalizes formats before publishing.

- **Secure storage**  
  Ensures published content is retrievable even under censorship.

- **Modular architecture**  
  Each subsystem is independently extensible and testable.

---

## Integration

The Emergency Channel is used by:

- **VPN Access Layer**  
  As a covert transport path.

- **News Aggregation**  
  For delivering aggregated content under censorship.

- **Anonymous BBS**  
  For multi‑hop message relay and posting.

- **Emergency Publishing**  
  As the primary delivery mechanism for critical updates.

It is the most resilient and censorship‑resistant layer in the system.

---

## Recommended Reading Order

For new contributors or reviewers:

1. **protocols/overview.md**  
   Understand the transport layer.

2. **protocols/selection.md**  
   Learn how protocols are chosen dynamically.

3. **router/**  
   Understand multi‑hop routing and session migration.

4. **distributor/**  
   Learn how content is distributed across diverse paths.

5. **publisher/**  
   Understand client‑facing delivery and fallback channels.

6. **pipeline/**  
   See the full end‑to‑end flow.

7. **sanitizer/**  
   Review content cleaning and metadata stripping.

8. **storage/**  
   Understand secure storage and retrieval.

9. **security/**  
   Review threat model and protocol defenses.

---

## Summary

The Emergency Channel module provides the system’s most critical capability:  
**reliable, censorship‑resistant publishing under hostile conditions.**

By combining multiple protocols, multi‑hop routing, adaptive fallback, sanitization, and secure storage, it ensures that essential information can always reach users—even in the most restricted environments.
