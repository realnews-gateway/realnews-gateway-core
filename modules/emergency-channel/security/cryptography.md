# Security Module — Cryptography

## Overview

The cryptographic layer provides confidentiality, integrity, and authenticity guarantees across the entire Emergency Channel system.  
It defines the primitives, key lifecycles, and verification rules used by all modules, ensuring consistent and secure handling of sensitive data.

Cryptography is applied at multiple stages:  
- During ingestion (hashing, integrity checks)  
- During internal transport (encryption, signing)  
- During storage (at‑rest encryption)  
- During publishing (optional signing or encryption)  

The system is designed to remain secure even under partial compromise.

---

## Cryptographic Primitives

The system uses a combination of modern, well‑vetted primitives:

### Symmetric Encryption
- AES‑256‑GCM  
- ChaCha20‑Poly1305 (fallback for low‑power or unstable environments)

### Asymmetric Encryption
- X25519 for key exchange  
- RSA‑3072 (legacy compatibility)

### Digital Signatures
- Ed25519 (primary)  
- ECDSA P‑256 (optional compatibility)

### Hashing & Integrity
- SHA‑256  
- BLAKE3 (optional high‑performance mode)

### Key Derivation
- HKDF  
- PBKDF2 (legacy compatibility)

All primitives are selected for security, performance, and long‑term maintainability.

---

## Key Management

The system enforces strict key lifecycle rules:

- **Key generation**  
  Uses secure randomness and modern curves.

- **Key rotation**  
  Periodic rotation to limit exposure.

- **Key revocation**  
  Immediate invalidation when compromise is suspected.

- **Key scoping**  
  Different modules use different keys to minimize blast radius.

- **Key storage**  
  Keys are stored encrypted and never logged.

- **Ephemeral keys**  
  Used for short‑lived sessions and internal transport.

Key material is never exposed to untrusted components.

---

## Signing & Verification

Digital signatures are used to ensure:

- Content authenticity  
- Module‑to‑module trust  
- Protection against tampering  
- Auditability of critical operations  

Every module verifies signatures before accepting sensitive data.

---

## Encryption in Transit

All internal communication channels enforce:

- TLS with modern cipher suites  
- Optional post‑quantum key exchange  
- Replay protection  
- Strict certificate validation  

Fallback channels may use lightweight encryption when full TLS is unavailable.

---

## Encryption at Rest

Stored data is protected using:

- AES‑256‑GCM  
- Per‑module encryption keys  
- Optional per‑record encryption  
- Integrity tags to detect corruption  

Storage never contains raw or unprotected data.

---

## Summary

The cryptographic subsystem provides:

- Strong encryption for data in motion and at rest  
- Modern, secure primitives  
- Strict key lifecycle management  
- Robust signing and verification  
- Defense‑in‑depth across all modules  

It ensures that the Emergency Channel remains secure even under adversarial conditions.
