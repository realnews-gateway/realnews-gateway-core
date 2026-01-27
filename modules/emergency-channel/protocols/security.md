# Protocol Security Considerations

## Overview

This document describes the security properties, risks, and defensive measures associated with the six transport protocols used by the Emergency Channel system:

- XHTTP  
- XTLS  
- Reality  
- Hysteria2  
- VLESS  
- Trojan  

Each protocol provides different strengths and trade‑offs.  
This document explains how they behave under adversarial conditions and how the system mitigates protocol‑level threats.

---

## Threat Model

Protocols are evaluated against the following adversarial capabilities:

- **Passive surveillance**  
  DPI, traffic analysis, TLS fingerprinting, metadata collection.

- **Active probing**  
  Connection attempts to detect proxy behavior or protocol responses.

- **SNI filtering and TLS interception**  
  Blocking based on domain names or TLS handshake content.

- **Packet injection and reset**  
  Disruption via forged TCP RST or QUIC CLOSE frames.

- **Bandwidth throttling**  
  Selective degradation of suspected proxy traffic.

- **Protocol fingerprinting**  
  Detection via header order, timing, packet size, or handshake anomalies.

- **Replay and downgrade attacks**  
  Attempts to reuse or weaken session parameters.

Each protocol includes defenses tailored to these threats.

---

## Protocol‑Specific Security Analysis

### XHTTP Security

**Strengths**

- Looks identical to normal HTTP traffic  
- Can run behind CDNs and reverse proxies  
- Encrypted payloads hidden inside legitimate HTTP bodies  
- Header order and casing mimic real browsers  
- Optional HTML/JSON wrappers for camouflage  

**Threat Resistance**

- **Passive DPI**: Hard to distinguish from real HTTP  
- **Active probing**: Responds with valid HTTP semantics  
- **SNI filtering**: Not applicable (HTTP layer)  
- **Traffic analysis**: Padding and timing jitter reduce detectability  

**Risks**

- Lower encryption strength if deployed without TLS  
- Some CDNs may normalize headers, reducing camouflage  

---

### XTLS Security

**Strengths**

- TLS 1.3 handshake identical to real HTTPS  
- No unusual extensions or cipher suites  
- Optimized encrypted data path  
- Low fingerprint surface  

**Threat Resistance**

- **Passive DPI**: Indistinguishable from HTTPS  
- **Active probing**: Behaves like a normal TLS server  
- **Traffic analysis**: Data path optimizations reduce anomalies  

**Risks**

- Not as stealth‑focused as Reality  
- Performance optimizations must not weaken encryption  

---

### Reality Security

**Strengths**

- Real‑site mimicry using genuine certificates  
- TLS handshake identical to real browser traffic  
- Unauthorized clients receive real website content  
- Extremely strong active probing resistance  

**Threat Resistance**

- **Passive DPI**: Impossible to distinguish from real HTTPS  
- **Active probing**: Server behaves like a real website  
- **SNI filtering**: Uses legitimate domains  
- **Traffic analysis**: Matches real site patterns  

**Risks**

- Requires access to a real domain and certificate  
- Blocking Reality often means blocking the real site  

---

### Hysteria2 Security

**Strengths**

- QUIC handshake is encrypted  
- Resistant to packet loss and throttling  
- No plaintext protocol identifiers  
- Adaptive congestion control masks traffic patterns  

**Threat Resistance**

- **Passive DPI**: QUIC handshake is encrypted  
- **Traffic analysis**: Packet size randomization available  
- **Throttling**: Congestion control resists shaping  

**Risks**

- QUIC traffic is easier to classify than TLS  
- Less stealthy than Reality or Trojan  

---

### VLESS Security

**Strengths**

- Minimal framing  
- No built‑in encryption → no cryptographic fingerprint  
- Extensible metadata fields  
- Works under TLS/XTLS/Reality for full protection  

**Threat Resistance**

- **Passive DPI**: Security depends on outer layer  
- **Active probing**: Outer layer handles camouflage  
- **Traffic analysis**: Minimal overhead reduces anomalies  

**Risks**

- Must always be paired with a secure transport  
- Incorrect deployment may expose metadata  

---

### Trojan Security

**Strengths**

- TLS camouflage identical to real HTTPS  
- Real certificates and realistic handshake patterns  
- Simple, stable, and low‑fingerprint  
- Strong active probing resistance  

**Threat Resistance**

- **Passive DPI**: Looks like normal HTTPS  
- **Active probing**: Invalid clients receive real website content  
- **Traffic analysis**: Matches HTTPS patterns  

**Risks**

- Slightly lower performance than XTLS  
- Requires valid certificates  

---

## Cross‑Protocol Security Measures

### Encryption

All protocols (except raw VLESS) use:

- AES‑GCM or ChaCha20‑Poly1305  
- Integrity protection  
- Optional forward secrecy  
- Replay protection  

VLESS inherits encryption from its outer layer.

---

### Padding & Obfuscation

Protocols support:

- Randomized padding  
- Timing jitter  
- Packet size normalization  
- Stream multiplexing  

These reduce fingerprintability.

---

### Active Probing Defense

Reality and Trojan provide the strongest defense:

- Unauthorized clients receive real website content  
- No observable difference from legitimate traffic  

XHTTP also provides strong defense via valid HTTP semantics.

---

### Metadata Minimization

The system avoids:

- Static identifiers  
- Predictable headers  
- Unique handshake patterns  
- Protocol‑specific plaintext markers  

This reduces the attack surface.

---

## Summary

The protocol stack provides:

- Strong encryption  
- Real‑site mimicry (Reality)  
- TLS camouflage (Trojan, XTLS)  
- HTTP camouflage (XHTTP)  
- QUIC‑based resilience (Hysteria2)  
- Minimal framing (VLESS)  
- Resistance to DPI, SNI filtering, and active probing  
- Multi‑protocol fallback and redundancy  

Together, these measures ensure that the Emergency Channel remains secure, stealthy, and resilient even under the most hostile network conditions.
