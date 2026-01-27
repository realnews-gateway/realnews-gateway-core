# Protocol Selection Logic

## Overview

The Emergency Channel system supports six transport protocols:

- XHTTP  
- XTLS  
- Reality  
- Hysteria2  
- VLESS  
- Trojan  

Each protocol is optimized for different network conditions and threat models.  
The protocol selection layer determines which protocol to use at any given moment, ensuring reliability, censorship resistance, and optimal performance.

Protocol selection is dynamic, adaptive, and fully automated.

---

## Selection Principles

The system chooses protocols based on the following principles:

- **Stealth first**  
  When censorship is aggressive, prioritize protocols that mimic real traffic.

- **Performance when possible**  
  When the network is stable, prefer high‑throughput protocols.

- **Fallback always available**  
  Every protocol has at least one fallback path.

- **No single point of failure**  
  Multiple independent transports ensure resilience.

- **Continuous adaptation**  
  Selection adjusts to real‑time network conditions.

---

## Protocol Characteristics Summary

| Protocol   | Strengths | Weaknesses | Best Use Cases |
|-----------|-----------|------------|----------------|
| **XHTTP** | Strong camouflage, CDN‑friendly, flexible | Moderate throughput | High‑risk, HTTP‑restricted networks |
| **XTLS** | High performance, low fingerprint | Less stealth than Reality | High‑bandwidth links |
| **Reality** | Maximum stealth, real‑site mimicry | Moderate speed | Hostile, DPI‑heavy environments |
| **Hysteria2** | Fastest under loss, QUIC‑based | Less stealth | Poor or unstable networks |
| **VLESS** | Lightweight, extensible | No built‑in encryption | Base layer under TLS/XTLS/Reality |
| **Trojan** | Strong TLS camouflage, stable | Lower throughput | TLS‑restricted or stealth‑critical links |

---

## Dynamic Selection Algorithm

The system evaluates:

- Packet loss  
- Latency  
- Jitter  
- Bandwidth  
- TLS interception  
- DPI behavior  
- Connection failure patterns  
- Regional censorship signatures  

Based on these signals, the system selects the optimal protocol.

### Example Logic

1. **If TLS interception or DPI is detected → Use Reality or Trojan**  
2. **If packet loss > 15% → Use Hysteria2**  
3. **If HTTP‑only environment → Use XHTTP**  
4. **If high bandwidth available → Use XTLS**  
5. **If stealth required but TLS is allowed → Use Trojan**  
6. **If all else fails → Use XHTTP fallback mode**

The system continuously re‑evaluates conditions and switches protocols when necessary.

---

## Fallback Chains

Fallback chains define the order in which protocols are attempted when one fails.

### High‑Stealth Chain
1. Reality  
2. Trojan  
3. XHTTP  
4. XTLS  
5. Hysteria2  

### High‑Performance Chain
1. XTLS  
2. Hysteria2  
3. VLESS over TLS  
4. Trojan  
5. XHTTP  

### High‑Compatibility Chain
1. XHTTP  
2. Trojan  
3. Reality  
4. VLESS  
5. Hysteria2  

Fallback chains are configurable per deployment.

---

## Multi‑Protocol Redundancy

The system supports:

- **Parallel probing**  
  Test multiple protocols simultaneously.

- **Opportunistic switching**  
  Switch when a better protocol becomes available.

- **Session migration**  
  Move active sessions between protocols without data loss.

- **Multi‑hop diversity**  
  Each hop may use a different protocol.

This ensures resilience even under targeted blocking.

---

## Integration

Protocol selection is used by:

- Router  
- Distributor  
- Publisher  
- Multi‑hop relay subsystems  

It acts as the decision‑making layer for all outbound connections.

---

## Summary

The protocol selection subsystem provides:

- Dynamic, adaptive protocol choice  
- Strong censorship resistance  
- High performance under diverse conditions  
- Configurable fallback chains  
- Multi‑protocol redundancy and resilience  

It ensures that the Emergency Channel remains operational even when individual protocols are blocked or degraded.
