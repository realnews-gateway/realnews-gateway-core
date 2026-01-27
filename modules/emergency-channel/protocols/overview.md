# Protocols Module — Overview

## Purpose

The Protocols module provides the transport foundation for the Emergency Channel system.  
It defines how data moves across networks—securely, reliably, and in a censorship‑resistant manner—using six complementary protocols:

1. **XHTTP**  
2. **XTLS**  
3. **Reality**  
4. **Hysteria2**  
5. **VLESS**  
6. **Trojan**

Each protocol is optimized for different threat models, network conditions, and deployment environments.  
Together, they form a resilient, multi‑path communication layer.

---

## Design Goals

The protocol stack is designed to achieve:

- **Censorship resistance**  
  Ability to bypass blocking, throttling, and DPI fingerprinting.

- **Stealth and camouflage**  
  Traffic that blends into normal HTTPS or HTTP patterns.

- **High performance**  
  Efficient transport even under packet loss or unstable networks.

- **Low fingerprint surface**  
  Minimal protocol‑level identifiers or anomalies.

- **Fallback and redundancy**  
  Multiple independent paths to ensure delivery.

- **Extensibility**  
  New protocols can be added without modifying upstream modules.

---

## The Six Protocols

### XHTTP  
A hardened, encrypted transport embedded inside normal‑looking HTTP traffic.

### XTLS  
A high‑performance TLS transport with reduced fingerprinting and optimized data paths.

### Reality  
A real‑site mimicry protocol that uses legitimate TLS fronting to evade detection.

### Hysteria2  
A QUIC‑based, loss‑resistant, high‑throughput protocol optimized for unstable networks.

### VLESS  
A lightweight, flexible transport protocol with no legacy baggage and strong extensibility.

### Trojan  
A TLS‑camouflaged protocol that looks identical to normal HTTPS traffic.

---

## Protocol Selection Logic

The system dynamically selects protocols based on:

- Network stability  
- Censorship level  
- Latency and packet loss  
- Endpoint capabilities  
- Deployment configuration  

Fallback chains allow seamless switching when a protocol is blocked or degraded.

---

## Integration with the Pipeline

Protocols are used by:

- Router  
- Distributor  
- Publisher  
- Optional multi‑hop or P2P subsystems  

They provide the underlying transport layer for all inter‑module and external communication.

---

## Summary

The Protocols module provides:

- A unified, multi‑protocol transport layer  
- Strong censorship resistance  
- High performance under adverse conditions  
- Dynamic fallback and negotiation  
- Six proven, complementary protocols  

This module ensures that the Emergency Channel remains operational even in the most hostile network environments.
