# Protocols Module

The Protocols module defines the network‑level transports used by the Emergency Channel system.  
It provides a unified abstraction for multiple censorship‑resistant, high‑performance, and low‑fingerprint protocols, enabling the system to operate reliably across diverse and adversarial network environments.

This module contains the specifications and integration details for the six supported protocols:

- **XHTTP** — Hardened, HTTP‑based encrypted transport with strong camouflage  
- **XTLS** — High‑performance TLS transport with minimal fingerprint surface  
- **Reality** — Real‑site mimicry with TLS fronting and indistinguishable handshakes  
- **Hysteria2** — QUIC‑based, loss‑resistant, high‑throughput transport  
- **VLESS** — Lightweight, extensible, and flexible transport protocol  
- **Trojan** — TLS‑camouflaged protocol designed for stability and stealth

Each protocol serves a different purpose and threat model.  
The system can dynamically select, combine, or fall back between protocols based on network conditions.

---

## Module Structure

This directory contains the following files:

- **overview.md**  
  High‑level description of the protocol stack and selection logic.

- **xhttp.md**  
  Specification for the XHTTP protocol.

- **xtls.md**  
  Specification for the XTLS transport layer.

- **reality.md**  
  Specification for Reality‑style real‑site mimicry.

- **hysteria2.md**  
  Specification for the Hysteria2 QUIC‑based protocol.

- **vless.md**  
  Specification for the VLESS transport protocol.

- **trojan.md**  
  Specification for the Trojan TLS‑camouflaged protocol.

---

## Summary

The Protocols module provides:

- A unified, extensible protocol abstraction  
- Strong censorship resistance  
- Multiple independent transport paths  
- Dynamic fallback and negotiation  
- Support for six proven, widely deployed protocols  

These protocols ensure that the Emergency Channel can operate reliably even in hostile or heavily censored environments.
