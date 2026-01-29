# Anonymous BBS Module

The anonymous BBS module provides a censorship-resistant, privacy-preserving message board system designed for high-risk environments.  
Users create accounts for access control and rate limiting, but all public-facing identity is pseudonymous: posts are shown under system-generated names and avatars, never exposing real user information.

This module is optimized for regions with strict censorship, intermittent connectivity, and active surveillance.

---

## Objectives

The anonymous BBS module provides:

- Account-based access with pseudonymous public identity
- System-generated display names and avatars per account or per thread
- No exposure of real user identifiers in public views
- Metadata stripping and privacy-preserving message formats
- Region-aware routing and replication
- Censorship-resistant distribution channels
- Optional moderation and spam filtering
- Compatibility with low-bandwidth or unstable networks

These capabilities ensure safe and resilient communication for users in hostile environments.

---

## Architecture Overview

The BBS system consists of five major components:

### 1. Account Layer
    - Lightweight account creation
    - Minimal stored information (no real-name, no phone/email required)
    - Used for access control, rate limiting, and abuse prevention
    - Not exposed in any public-facing message metadata

### 2. Pseudonym Layer
    - Generates random display names and avatars
    - Can be per-account, per-thread, or per-session
    - Ensures that public identity is decoupled from the underlying account
    - Prevents correlation of posts to real-world identity

### 3. Client Interface
    - Minimal UI for posting and reading messages
    - Shows only pseudonymous names and avatars
    - No public exposure of account IDs or login details
    - Optional offline mode with delayed posting

### 4. Message Gateway
    - Receives posts from authenticated accounts
    - Strips network metadata (IP, headers)
    - Attaches pseudonymous identity (name + avatar) for display
    - Applies spam and abuse filters

### 5. Storage and Distribution Layer
    - Stores messages in append-only logs
    - Replicates across multiple regions
    - Supports partial replication for high-risk areas
    - Delivers messages via region-aware, censorship-resistant channels

---

## Directory Structure

The module is organized as follows:

    anonymous-bbs/
    ├── README.md              # Overview and architecture
    ├── message-format.md      # Message structure, pseudonymous identity, metadata stripping
    ├── gateway.md             # Message ingestion, filtering, and normalization
    ├── storage.md             # Append-only logs, replication, and consistency
    └── distribution.md        # Region-aware delivery and censorship resistance

Each file documents a major subsystem of the anonymous BBS.

---

## Design Principles

The BBS follows these principles:

### Account-Based Control, Pseudonymous Exposure
    - Accounts exist for access control and abuse prevention
    - Public-facing identity is always pseudonymous
    - No real user information appears in messages or public APIs

### Anonymity in Public Space
    - No public account IDs
    - No IP or device metadata stored with messages
    - Pseudonyms are system-generated and non-identifying

### Minimal Metadata
    - Only message body, pseudonymous identity, and coarse timestamps
    - Timestamps may be bucketed or jittered
    - No client-side identifiers in public data

### Censorship Resistance
    - Region-aware routing and replication
    - Multi-path delivery
    - Integration with fallback transports and VPN access layer

### Low Bandwidth Operation
    - Compact message formats
    - Optional compressed feeds
    - Offline posting support

### Safety and Abuse Prevention
    - Spam filtering and rate limiting at the account layer
    - Optional keyword moderation
    - No need for real-world identity to enforce basic rules

---

## Integration with Other Modules

The anonymous BBS integrates with:

- **vpn-access-layer/**  
  Ensures anonymous and censorship-resistant connectivity.

- **emergency-channel/**  
  Allows urgent or high-impact posts to be promoted to emergency distribution.

- **publisher/**  
  Enables selected BBS posts to be published externally, still under pseudonymous identity.

- **region-config/**  
  Provides region-aware routing and replication policies.

---

## Summary

The anonymous BBS module provides a secure, censorship-resistant message board system designed for high-risk environments.  
By combining account-based access control with strictly pseudonymous public identity, metadata stripping, region-aware routing, and resilient replication, it enables safe communication without exposing real user information.
