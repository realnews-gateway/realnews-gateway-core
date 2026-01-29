# Distribution Layer: Region-Aware Delivery and Censorship Resistance

The distribution layer is responsible for delivering pseudonymous BBS messages to clients in a censorship-resistant, low-bandwidth, and privacy-preserving manner.  
It retrieves messages from the storage layer and distributes them using region-aware routing strategies that adapt to censorship conditions and network instability.

This layer ensures that users can read and interact with the BBS even in high-risk environments.

---

## Objectives

The distribution layer provides:

- Region-aware message delivery
- Low-bandwidth and offline-friendly formats
- Multi-path routing for censorship resistance
- Integration with fallback transports (TLS, QUIC, HTTP/2, CDN)
- Optional compressed feeds for unstable networks
- No exposure of account or network metadata
- Efficient retrieval from append-only logs

These capabilities ensure reliable access to BBS content under hostile conditions.

---

## Distribution Pipeline

The distribution pipeline consists of four stages:

### 1. Query Handling
    - Clients request messages by:
        - Thread ID
        - Time window
        - Pagination
    - No user-based queries (no user identity stored)
    - No full-text search (to avoid metadata leakage)

### 2. Region-Aware Routing
    - Selects optimal delivery path based on:
        - Local censorship conditions
        - Network stability
        - Region configuration
    - Supports fallback routing when primary paths fail

### 3. Message Packaging
    - Retrieves messages from append-only logs
    - Packages them into compact bundles
    - Applies optional compression
    - Removes any internal metadata

### 4. Delivery
    - Sends messages using censorship-resistant transports
    - Supports offline delivery modes
    - Ensures anonymity and minimal metadata exposure

---

## Delivery Formats

The distribution layer supports multiple formats depending on bandwidth and censorship risk:

### 1. Standard JSON Feed
    - Default format
    - Includes pseudonym, body, timestamp, tags
    - Suitable for stable networks

### 2. Compressed Feed
    - Gzip or Brotli compression
    - Ideal for low-bandwidth regions
    - Reduces message size significantly

### 3. Delta Feed
    - Sends only new messages since last fetch
    - Minimizes bandwidth usage
    - Useful for mobile or unstable networks

### 4. Offline Bundle
    - Pre-packaged message bundles
    - Can be downloaded when connectivity is available
    - Supports delayed reading

---

## Censorship-Resistant Routing

The distribution layer integrates with the vpn-access-layer to ensure delivery even under active blocking.

### Routing Strategies

#### Primary Path
    - QUIC or TLS
    - CDN-backed endpoints
    - Region-optimized servers

#### Fallback Path
    - HTTP/2 disguised as browser traffic
    - Domain fronting (if allowed)
    - CDN edge nodes
    - Proxy-based mirrors

#### Stealth Path (High-Risk Regions)
    - Delayed delivery
    - Randomized timing
    - Encrypted bundles
    - Hidden service endpoints (optional)

Routing decisions are dynamic and adapt to real-time conditions.

---

## Privacy and Metadata Protection

The distribution layer ensures:

### No Exposure of:
    - Account IDs
    - IP addresses
    - Device identifiers
    - User-agent strings
    - Internal timestamps

### Only Public Message Fields Are Delivered:
    - Pseudonym (name + avatar)
    - Body
    - Coarse timestamp
    - Tags
    - Thread ID
    - Reply-to ID

No additional metadata is added during distribution.

---

## Rate Limiting and Abuse Prevention

To prevent scraping or DDoS attacks:

    - Rate limiting is applied per IP or per region
    - No persistent identifiers are required
    - Limits are coarse and privacy-preserving
    - Excessive requests trigger fallback routing

---

## Integration with Other Modules

The distribution layer integrates with:

- **storage.md**  
  Retrieves messages from append-only logs.

- **gateway.md**  
  Ensures all messages are normalized and pseudonymous before distribution.

- **message-format.md**  
  Defines the public message structure used in feeds.

- **vpn-access-layer/**  
  Provides censorship-resistant delivery paths.

- **region-config/**  
  Defines routing and replication policies per region.

---

## Summary

The distribution layer delivers pseudonymous BBS messages to clients using region-aware, censorship-resistant routing strategies.  
By supporting low-bandwidth formats, multi-path delivery, and strict metadata protection, it ensures reliable access to the BBS even in hostile network environments.
