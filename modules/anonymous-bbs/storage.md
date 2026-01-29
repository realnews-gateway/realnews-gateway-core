# Storage Layer: Append-Only Logs and Region-Aware Replication

The storage layer is responsible for securely storing pseudonymous messages and replicating them across regions in a censorship-resistant manner.  
It uses append-only logs, multi-region replication, and optional delayed synchronization for high-risk environments.

The storage layer never stores real user identifiers; it only stores the public message format produced by the gateway.

---

## Objectives

The storage layer provides:

- Immutable append-only message logs
- Region-aware replication strategies
- Optional delayed or partial replication for high-risk regions
- Efficient retrieval for distribution channels
- Integrity protection and tamper detection
- No storage of real user identity or network metadata

These capabilities ensure durability, safety, and censorship resistance.

---

## Storage Model

The storage layer uses an append-only log structure:

    log:
      - message_001
      - message_002
      - message_003
      - ...

Properties:

### Immutable
    - Messages cannot be modified or deleted
    - Ensures auditability and prevents tampering

### Ordered
    - Messages are stored in the order they are accepted by the gateway
    - Timestamps are coarse-grained but ordering is preserved

### Compact
    - Only public message fields are stored
    - No internal metadata or account identifiers

---

## Message Entry Format

Each log entry contains:

    log_entry:
      id: Public message ID
      pseudonym:
        name: System-generated display name
        avatar: System-generated avatar identifier
      body: Cleaned message text
      timestamp: Coarse-grained or jittered timestamp
      tags: Optional tags
      thread_id: Thread or topic identifier
      reply_to: Optional parent message ID

This matches the public message format defined in `message-format.md`.

---

## Region-Aware Replication

The storage layer supports multiple replication strategies depending on censorship risk:

### 1. Full Replication (Low-Risk Regions)
    - All messages replicated across all nodes
    - Fast synchronization
    - Ideal for stable networks

### 2. Partial Replication (Medium-Risk Regions)
    - Only selected threads or categories replicated
    - Reduces exposure to censorship
    - Limits bandwidth usage

### 3. Delayed Replication (High-Risk Regions)
    - Messages stored locally first
    - Replicated after a delay or via fallback transports
    - Reduces risk of immediate detection

### 4. Multi-Path Replication
    - Uses multiple transport paths (TLS, QUIC, CDN)
    - Integrates with vpn-access-layer fallback chains
    - Ensures replication even under active blocking

---

## Integrity and Tamper Resistance

To prevent tampering:

### Hash Chains
    - Each log entry includes a hash of the previous entry
    - Forms a tamper-evident chain

### Optional Merkle Trees
    - Efficient verification of large logs
    - Supports partial replication

### Signature Verification
    - Storage nodes sign log segments
    - Clients can verify authenticity

---

## Storage Nodes

Storage nodes may be:

    - Regional servers
    - CDN-backed endpoints
    - Peer-to-peer nodes (optional)
    - Hidden service nodes (optional)

Nodes can operate in:

### Public Mode
    - Full replication
    - High availability

### Restricted Mode
    - Partial replication
    - Limited exposure

### Stealth Mode
    - Delayed replication
    - Hidden endpoints
    - Integration with fallback transports

---

## Retrieval and Querying

Clients retrieve messages via:

    - Thread-based queries
    - Time-based windows
    - Pagination
    - Compressed feeds for low-bandwidth regions

The storage layer does not support:

    - Full-text search (to avoid metadata leakage)
    - User-based queries (no user identity stored)

---

## Integration with Other Modules

The storage layer integrates with:

- **gateway.md**  
  Receives normalized, pseudonymous messages for storage.

- **distribution.md**  
  Retrieves messages for region-aware delivery.

- **vpn-access-layer/**  
  Ensures replication and retrieval under censorship.

- **region-config/**  
  Defines replication policies per region.

---

## Summary

The storage layer provides durable, censorship-resistant storage for pseudonymous BBS messages.  
By using append-only logs, region-aware replication, and tamper-evident structures, it ensures message integrity and availability even in hostile environments.
