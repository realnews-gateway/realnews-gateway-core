# Threat Model

This document outlines the threat model for RealNews Free Publish Core. The system is designed for environments with strong, adaptive, state‑level censorship and surveillance.

## Protocol Strategy

The system consistently maintains six core protocols (Hysteria2, Reality, VLESS, Trojan, XTLS, XHTTP). Future protocols may be added, but these six remain the foundation.

## Accounts and Anonymity

- Accounts are required for system access.  
- Anonymity is also required: account identity is strictly separated from publishing identity.  
- This ensures controlled access while preserving anonymous publishing.

## Adversaries

### 1. State‑Level Censors
Capabilities:
- Deep Packet Inspection (DPI)
- Active probing
- Traffic fingerprinting
- IP/domain blocking
- TLS fingerprinting
- Platform takedowns

Mitigations:
- Six protocols maintained and updated
- TLS 1.3 camouflage
- Low‑fingerprint transport
- Multi‑node routing and fallback

### 2. Surveillance Agencies
Capabilities:
- Metadata collection
- Device monitoring
- Traffic correlation
- Social graph analysis

Mitigations:
- Metadata stripping
- Anonymous submission channels (account/identity separation)
- Automatic encryption
- Decentralized storage (IPFS/Arweave)

### 3. Platform Moderation Systems
Capabilities:
- Automated content removal
- Hash‑based media filtering
- Keyword detection

Mitigations:
- Multi‑mirror distribution
- Decentralized persistence
- Content deduplication and re‑encoding

## User Risks

- Device seizure  
- Account compromise  
- Network monitoring  
- Social graph exposure  

Mitigations:
- Account/identity separation  
- Automatic encryption  
- Minimal client footprint  
- No persistent identifiers  

## Out‑of‑Scope Threats

- Compromised user devices  
- Hardware implants  
- Zero‑day OS exploits  
- Physical coercion  

These threats exceed the scope of software‑level protections.
