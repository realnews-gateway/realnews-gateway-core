# Key Exchange

The Key Exchange subsystem defines how clients and servers derive secure, ephemeral session keys during session initialization.  
It is designed to resist replay attacks, active probing, metadata leakage, and protocol fingerprinting, while remaining indistinguishable from normal TLS/QUIC behavior.

Key exchange is a foundational component of censorship‑resistant session establishment.

---

## Purpose

Key exchange enables:

- Secure derivation of ephemeral session keys  
- Replay-resistant handshake flows  
- Covert embedding inside TLS/QUIC handshakes  
- Optional hybrid post-quantum protection  
- Authentication without exposing identifiers  
- Integration with camouflage and negotiation layers  

This ensures that every session begins with strong cryptographic guarantees and minimal detectability.

---

## Design Principles

The key exchange subsystem follows four core principles:

### **1. Ephemeral by Default**
All session keys are derived from:

- One-time ephemeral x25519 key pairs  
- Optional hybrid post-quantum KEM components  
- Nonces and tokens that prevent replay  

No long-term keys are exposed during handshake.

---

### **2. Covert Embedding**
Key exchange messages are embedded inside:

- TLS ClientHello / ServerHello extensions  
- QUIC transport parameters  
- HTTP/2 pseudo-headers (fallback mode)  
- CDN-compatible metadata (domain-fronted mode)  

This prevents DPI from identifying custom handshake patterns.

---

### **3. Replay Resistance**
Replay protection is enforced through:

- One-time handshake tokens  
- Nonces tied to session timestamps  
- Rejecting repeated ClientHello messages  
- Optional server-issued challenge/response  

Replay resistance is essential for preventing fingerprint harvesting.

---

### **4. Camouflage Alignment**
Key exchange must align with:

- TLS fingerprint camouflage  
- Real-site mimicry  
- SNI randomization  
- Handshake obfuscation  

This ensures that cryptographic operations do not create detectable anomalies.

---

## Key Exchange Flow

The key exchange process consists of five stages:

### **1. Client Ephemeral Key Generation**
Client generates:

- Ephemeral x25519 key pair  
- Optional PQ hybrid component  
- Nonce for replay protection  

### **2. Server Ephemeral Key Generation**
Server generates:

- Ephemeral x25519 key pair  
- Optional PQ hybrid component  
- Session token for authentication
