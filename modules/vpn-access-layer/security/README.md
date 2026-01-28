# Security Layer

The Security Layer defines the threat model, defensive strategies, and protocol‑level protections used by the VPN Access Layer.  
It ensures that all access paths remain safe, stealthy, and resilient against censorship, surveillance, and active attacks.

This subsystem provides the security foundation for all entrypoints, camouflage mechanisms, fallback strategies, and session initialization flows.

---

## Purpose

The Security Layer enables:

- A clear threat model for hostile network environments  
- Defenses against DPI, traffic classification, and active probing  
- Protection against replay attacks, metadata leakage, and fingerprinting  
- Secure integration with camouflage and session-init layers  
- Region‑specific security considerations  
- Continuous adaptation to evolving censorship techniques  

It ensures that the VPN Access Layer remains robust under real‑world adversarial pressure.

---

## Directory Structure

This directory includes:

- **threat-model.md**  
  Defines adversaries, capabilities, and attack surfaces relevant to the VPN Access Layer.

- **dpi-evasion.md**  
  Techniques for evading deep packet inspection and traffic classifiers.

- **active-probing-defense.md**  
  Defenses against active probing, handshake manipulation, and protocol impersonation.

- **security.md**  
  High-level specification of security requirements, guarantees, and integration.

---

## Key Features

- **Comprehensive threat model**  
  Covers:
  - state‑level censors  
  - ISP‑level filtering  
  - active probing systems  
  - machine‑learning traffic classifiers  
  - throttling and selective blocking  

- **DPI evasion**  
  Uses:
  - TLS fingerprint shaping  
  - QUIC traffic normalization  
  - HTTP camouflage  
  - SNI randomization  
  - packet size/timing obfuscation  

- **Active probing resistance**  
  Ensures:
  - no static signatures  
  - no predictable handshakes  
  - no distinguishable error messages  
  - protocol mimicry under probe conditions  

- **Metadata minimization**  
  Reduces exposure of:
  - client identifiers  
  - session tokens  
  - timing patterns  
  - transport‑level hints  

- **Defense‑in‑depth**  
  Security is layered across:
  - entrypoints  
  - camouflage  
  - session-init  
  - fallback  
  - routing  

- **Modular and extensible**  
  New defenses can be added without breaking existing components.

---

## Integration

The Security Layer connects to:

- **entrypoints/**  
  Ensures all inbound traffic is indistinguishable from legitimate traffic.

- **camouflage/**  
  Provides security constraints for TLS/QUIC fingerprinting and mimicry.

- **session-init/**  
  Ensures key exchange and negotiation are safe under adversarial conditions.

- **fallback/**  
  Adjusts fallback behavior based on detected attacks or interference.

- **emergency-publishing/security/**  
  Aligns with system‑wide threat models and guarantees.

Security is the backbone of the VPN Access Layer’s resilience.

---

## Summary

The Security Layer defines the threat model and defensive strategies that protect the VPN Access Layer from censorship, surveillance, and active attacks.  
By combining DPI evasion, active probing resistance, metadata minimization, and layered defenses, it ensures that users can safely and reliably access the system—even in the most hostile network environments.
