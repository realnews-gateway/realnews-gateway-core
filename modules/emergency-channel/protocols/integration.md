# Protocol Integration

## Overview

The Emergency Channel system uses six transport protocols:

- XHTTP  
- XTLS  
- Reality  
- Hysteria2  
- VLESS  
- Trojan  

These protocols are not standalone components.  
They integrate deeply with the system’s Router, Distributor, Publisher, and multi‑hop relay subsystems.  
This document describes how each protocol is used, how modules interact with the protocol layer, and how the system maintains reliability and censorship resistance across diverse network environments.

---

## Integration Principles

Protocol integration follows these principles:

- **Transport abstraction**  
  Modules do not depend on specific protocols; they use a unified interface.

- **Dynamic selection**  
  The system chooses the best protocol at runtime.

- **Fallback and redundancy**  
  Multiple protocols are available for each outbound connection.

- **Separation of concerns**  
  Protocol logic is isolated from application logic.

- **Composable layering**  
  Protocols can be stacked (e.g., VLESS over XTLS).

---

## Unified Transport Interface

All modules interact with protocols through a unified interface:

- `connect()`  
- `send()`  
- `receive()`  
- `close()`  
- `probe()`  
- `fallback()`  

This abstraction allows modules to switch protocols without changing their internal logic.

---

## Module‑Level Integration

### Router Integration

The Router uses protocols to:

- Establish upstream connections  
- Probe multiple protocols in parallel  
- Select the best protocol based on real‑time conditions  
- Migrate active sessions between protocols  
- Maintain multi‑hop routing paths  

Router prioritizes:

- Reality / Trojan for stealth  
- XTLS / Hysteria2 for performance  
- XHTTP for HTTP‑restricted environments  

---

### Distributor Integration

The Distributor uses protocols to:

- Deliver content across multiple hops  
- Maintain redundancy across diverse transports  
- Adapt to regional censorship differences  
- Balance load across protocol families  

Distributor often mixes protocols across hops:

- Hop 1: Reality  
- Hop 2: Hysteria2  
- Hop 3: XHTTP  

This diversity increases resilience.

---

### Publisher Integration

The Publisher uses protocols to:

- Deliver content to end‑users  
- Support multiple access channels  
- Provide fallback paths for censored regions  
- Maintain compatibility with browsers and mobile clients  

Publisher typically uses:

- Trojan / Reality for TLS‑based channels  
- XHTTP for browser‑compatible channels  
- Hysteria2 for high‑speed channels  

---

## Protocol Layering

Protocols can be layered to combine strengths:

- **VLESS over XTLS**  
  High performance + flexible framing

- **VLESS over Reality**  
  Maximum stealth + extensibility

- **VLESS over Trojan**  
  TLS camouflage + simple framing

- **XHTTP over CDN**  
  HTTP camouflage + global distribution

Layering is transparent to upper modules.

---

## Multi‑Hop Integration

The system supports multi‑hop routing where each hop may use a different protocol:

Client → Reality → XTLS → Hysteria2 → XHTTP → Publisher

Benefits:

- Harder to fingerprint  
- Harder to block  
- More resilient to regional censorship  
- Independent failure domains  

Each hop independently selects its protocol based on local conditions.

---

## Failure Handling

When a protocol fails:

1. The unified interface reports the failure  
2. The selection layer chooses a fallback protocol  
3. The session migrates without data loss  
4. The module continues operation transparently  

Failures include:

- Connection timeout  
- TLS interception  
- DPI resets  
- Packet loss spikes  
- Bandwidth throttling  

Fallback is automatic and continuous.

---

## Monitoring & Telemetry

The protocol layer exposes:

- Latency  
- Packet loss  
- Jitter  
- Throughput  
- Failure rates  
- DPI interference patterns  

Modules use this data to:

- Adjust routing  
- Trigger fallback  
- Switch protocols  
- Optimize performance  

No sensitive metadata is logged.

---

## Summary

Protocol integration provides:

- A unified transport abstraction  
- Dynamic protocol selection  
- Multi‑hop, multi‑protocol routing  
- Layered protocol composition  
- Automatic fallback and recovery  
- Deep integration with Router, Distributor, and Publisher  

This integration ensures that the Emergency Channel remains reliable, fast, and censorship‑resistant across all environments.
