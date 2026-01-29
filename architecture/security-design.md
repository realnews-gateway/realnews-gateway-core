# Security Design

This document describes the security model of the Emergency Publishing System. It defines the system’s threat model, trust boundaries, data handling rules, and defensive strategies used to maintain confidentiality, integrity, and availability under hostile network conditions.

The system is designed for environments where adversaries may perform censorship, surveillance, traffic analysis, active probing, or targeted disruption.

---

## Threat Model

The system assumes the following adversary capabilities:

- Network-level blocking and throttling
- Deep packet inspection
- Active probing of suspected endpoints
- Traffic correlation and timing analysis
- Selective or global content filtering
- Compromise of user devices
- Regional infrastructure interference

The system does not assume:

- Full endpoint compromise of both sender and receiver simultaneously
- Unlimited adversary resources
- Ability to break modern cryptography

---

## Security Goals

The system is designed to achieve:

- Confidentiality of user-generated content
- Integrity of messages and metadata
- Availability under censorship and network disruption
- Anonymity for pseudonymous users
- Minimal metadata exposure
- Resistance to traffic fingerprinting
- Safe operation in high-risk regions

---

## Trust Boundaries

The system defines three major trust boundaries:

### 1. User Boundary
- User devices may be partially compromised
- Clients must strip local metadata before sending
- Pseudonyms prevent identity correlation

### 2. Access Boundary
- vpn-access-layer provides covert entry points
- Region-aware routing prevents predictable patterns
- No sensitive data is stored in access nodes

### 3. Core Boundary
- Emergency Channel is the only trusted processing environment
- Performs sanitization, storage, routing, and distribution
- Maintains strict separation between ingestion modules and core logic

---

## Data Minimization

The system minimizes data at every stage:

- No IP addresses stored
- No device identifiers retained
- No user-agent strings preserved
- Timestamps normalized to coarse granularity
- Region tags generalized when possible
- Logs disabled or heavily restricted

All modules pass only normalized, sanitized data into the Emergency Channel.

---

## Sanitization Pipeline

Before any content enters the Emergency Channel, it passes through a sanitization pipeline:

    1. Remove HTML, scripts, embedded objects
    2. Normalize whitespace and encoding
    3. Strip metadata and client-side identifiers
    4. Validate content length and structure
    5. Convert to internal message format

This ensures that no fingerprintable artifacts or malicious payloads enter the core system.

---

## Storage Security

The Emergency Channel stores messages using:

- Encrypted storage
- Region-aware retention policies
- Minimal metadata
- Content hashing for integrity
- Segmented storage to prevent correlation

No raw logs or access traces are retained.

---

## Routing Security

Routing is designed to resist censorship and correlation:

- Multi-hop routing
- Randomized path selection
- Region-aware routing rules
- Transport-level obfuscation
- Redundant delivery attempts
- No persistent routing identifiers

Routing decisions are stateless and cannot be linked across sessions.

---

## Transport Security

Transports are selected based on censorship intensity:

- Standard transports for low-risk regions
- Obfuscated transports for DPI environments
- Covert transports for high-risk regions
- Offline sync for unstable networks

All transports use encrypted payloads and mimic benign traffic patterns.

---

## Pseudonymity and Identity Protection

The anonymous-bbs module provides:

- System-generated pseudonyms
- Synthetic avatars
- No persistent identifiers
- No linkability between sessions
- Sanitized posts with no client metadata

This prevents identity correlation even under surveillance.

---

## Regional Safety Considerations

The system adapts behavior based on region:

- Different fallback chains
- Different retention policies
- Different transport preferences
- Different content distribution rules

This prevents predictable patterns that adversaries could exploit.

---

## Summary

The Emergency Publishing System is designed for hostile environments with strong adversaries. Security is enforced through strict data minimization, sanitization, encrypted storage, multi-hop routing, pseudonymity, and adaptive transport selection. The Emergency Channel forms the core trust boundary, ensuring that all content is processed securely and delivered under censorship pressure.
