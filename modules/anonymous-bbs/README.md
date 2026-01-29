# Anonymous BBS Module

The anonymous BBS module provides a censorship-resistant, privacy-preserving message board system designed for high-risk environments.  
It enables users to publish short text posts, share updates, and participate in discussions without revealing identity, metadata, or network fingerprints.

This module is optimized for regions with strict censorship, intermittent connectivity, and active surveillance.

---

## Objectives

The anonymous BBS module provides:

- Anonymous posting without account creation
- Metadata stripping and privacy-preserving message formats
- Region-aware routing and replication
- Censorship-resistant distribution channels
- Integration with emergency publishing workflows
- Optional moderation and spam filtering
- Compatibility with low-bandwidth or unstable networks

These capabilities ensure safe and resilient communication for users in hostile environments.

---

## Architecture Overview

The BBS system consists of four major components:

### 1. Client Interface
    - Minimal UI for posting and reading messages
    - No login or persistent identifiers
    - Optional offline mode with delayed posting

### 2. Message Gateway
    - Receives posts from clients
    - Strips metadata (IP, timestamps, headers)
    - Normalizes message format
    - Applies spam and abuse filters

### 3. Storage and Replication Layer
    - Stores messages in append-only logs
    - Replicates across multiple regions
    - Supports partial replication for high-risk areas
    - Ensures eventual consistency

### 4. Distribution Layer
    - Delivers messages to clients
    - Uses region-aware routing
    - Integrates with emergency publishing channels
    - Supports low-bandwidth delivery formats

---

## Directory Structure

The module is organized as follows:

    anonymous-bbs/
    ├── README.md              # Overview and architecture
    ├── message-format.md      # Anonymous message structure and metadata stripping
    ├── gateway.md             # Message ingestion, filtering, and normalization
    ├── storage.md             # Append-only logs, replication, and consistency
    └── distribution.md        # Region-aware delivery and censorship resistance

Each file documents a major subsystem of the anonymous BBS.

---

## Design Principles

The BBS follows these principles:

### Anonymity by Design
    - No accounts
    - No persistent identifiers
    - No IP or device metadata stored

### Minimal Metadata
    - Only message body and optional tags
    - Timestamps are coarse-grained or randomized
    - No client-side identifiers

### Censorship Resistance
    - Region-aware routing
    - Multi-path replication
    - Integration with fallback transports

### Low Bandwidth Operation
    - Compact message formats
    - Optional compressed feeds
    - Offline posting support

### Safety and Abuse Prevention
    - Spam filtering
    - Optional keyword moderation
    - Rate limiting without identity tracking

---

## Integration with Other Modules

The anonymous BBS integrates with:

- **vpn-access-layer/**  
  Ensures anonymous and censorship-resistant connectivity.

- **emergency-channel/**  
  Allows urgent posts to be promoted to emergency distribution.

- **publisher/**  
  Enables selected BBS posts to be published externally.

- **region-config/**  
  Provides region-aware routing and replication policies.

---

## Summary

The anonymous BBS module provides a secure, censorship-resistant message board system designed for high-risk environments.  
By combining anonymous posting, metadata stripping, region-aware routing, and resilient replication, it enables safe communication under hostile network conditions.
