# Message Gateway: Ingestion, Pseudonym Assignment, and Abuse Prevention

The message gateway is the entry point for all posts submitted to the anonymous BBS.  
Users authenticate with lightweight accounts, but the gateway ensures that all public-facing identity is pseudonymous and that no real user information is ever exposed.

The gateway performs metadata stripping, pseudonym assignment, normalization, and abuse prevention before messages enter the storage and distribution layers.

---

## Objectives

The gateway provides:

- Account authentication without exposing account identity
- Pseudonym assignment (name + avatar) for public display
- Metadata stripping (IP, headers, device info)
- Message normalization and safety filtering
- Rate limiting and spam prevention
- Region-aware routing to storage nodes

These capabilities ensure safe, anonymous, and censorship-resistant message ingestion.

---

## Gateway Pipeline

The gateway processes each incoming message through five stages:

### 1. Authentication
    - Validates the user's lightweight account
    - Ensures the account is not banned or rate-limited
    - Does NOT attach account ID to the public message

### 2. Metadata Stripping
    - Removes IP address after rate limiting
    - Removes user-agent and device identifiers
    - Removes region hints unless explicitly needed for routing
    - Ensures no network metadata enters the public layer

### 3. Pseudonym Assignment
    - Generates a pseudonym (name + avatar) for the message
    - Pseudonym can be:
        - Per-account (stable identity)
        - Per-thread (contextual identity)
        - Per-session (rotating identity)
    - No correlation to account ID

### 4. Message Normalization
    - Cleans and sanitizes message body
    - Removes HTML, scripts, and unsafe markup
    - Enforces maximum length
    - Normalizes whitespace and punctuation
    - Applies optional keyword moderation

### 5. Abuse Prevention
    - Spam scoring
    - Rate limiting (account-level, not identity-level)
    - Flood detection
    - Optional content moderation flags

Messages that pass all checks are forwarded to the storage layer.

---

## Pseudonym Assignment Details

The gateway uses a pseudonym generator that produces:

    pseudonym:
      name: e.g., "Quiet Lantern", "Blue Sparrow", "Silent River"
      avatar: e.g., abstract shapes, animals, geometric icons

Properties:

    - Deterministic per-account if configured
    - Or randomized per-thread for anonymity
    - Never reused across unrelated contexts
    - Never derived from account data

This ensures anonymity while maintaining conversational continuity.

---

## Rate Limiting and Abuse Prevention

Rate limiting is applied at the account layer:

    - Prevents spam without requiring real-world identity
    - Uses token bucket or sliding window algorithms
    - Does not store IP or device identifiers long-term

Spam detection uses:

    - Repetition scoring
    - Keyword heuristics
    - Message similarity checks
    - Optional ML-based filters

Moderation flags are stored only in the internal layer.

---

## Region-Aware Routing

The gateway selects storage nodes based on:

    - Region configuration
    - Network stability
    - Censorship conditions
    - Replication policies

High-risk regions may use:

    - Multi-path routing
    - Delayed replication
    - Encrypted append-only logs

Low-risk regions may use:

    - Direct routing
    - Faster replication

---

## Integration with Other Modules

The gateway integrates with:

- **message-format.md**  
  Defines the public and internal message structures.

- **storage.md**  
  Receives normalized, pseudonymous messages for append-only storage.

- **distribution.md**  
  Delivers messages to clients via region-aware channels.

- **vpn-access-layer/**  
  Ensures anonymous and censorship-resistant message submission.

---

## Summary

The message gateway ensures that all incoming posts are authenticated, anonymized, normalized, and safe before entering the BBS system.  
By combining pseudonym assignment, metadata stripping, and abuse prevention, it protects users while maintaining system integrity.
