# Anonymous Message Format and Pseudonymous Identity

This document defines the message format used by the anonymous BBS system.  
Although users authenticate with lightweight accounts, all public-facing identity is pseudonymous:  
messages display only system-generated names and avatars, never real user information.

The message format is designed to minimize metadata, preserve anonymity, and ensure compatibility with censorship-resistant distribution channels.

---

## Objectives

The message format provides:

- Strict separation between account identity and public identity
- System-generated pseudonyms (name + avatar)
- Minimal metadata for privacy and censorship resistance
- Coarse-grained or jittered timestamps
- Compact structure for low-bandwidth environments
- Compatibility with append-only logs and replication layers

These properties ensure that messages remain anonymous, safe, and easy to distribute.

---

## Message Structure

Each message consists of two layers:

### 1. Internal Layer (private, server-side)
    - Contains account ID for access control and abuse prevention
    - Never exposed to clients or included in public feeds
    - Used only by the gateway and moderation systems

### 2. Public Layer (exposed to clients)
    - Contains pseudonymous identity
    - Contains message body and minimal metadata
    - Safe for replication and distribution

---

## Internal Message Format (Private)

The internal format is stored only in the gateway and moderation systems:

    internal_message:
      internal_id: Unique message identifier
      account_id: Internal account identifier
      received_at: High-precision timestamp
      client_ip: Stripped immediately after rate limiting
      raw_body: Original message body before normalization
      metadata:
        user_agent: Optional, stripped after processing
        region_hint: Optional, used for routing
      flags:
        spam_score: Numeric score
        moderation_flags: List of flags

Notes:

- `account_id` is never exposed publicly.
- `client_ip` and `user_agent` are removed after rate limiting.
- `raw_body` is kept only for debugging or moderation.

---

## Public Message Format (Exposed)

The public-facing message format is compact and privacy-preserving:

    message:
      id: Public message ID
      pseudonym:
        name: System-generated display name
        avatar: System-generated avatar identifier
      body: Cleaned and normalized message text
      timestamp: Coarse-grained or jittered timestamp
      tags: Optional list of user-provided or system-generated tags
      thread_id: ID of the thread or topic
      reply_to: Optional parent message ID

### Pseudonym Rules

Pseudonyms are generated using:

    - Randomized name generator (e.g., "Silver Lantern", "Quiet Fox")
    - Avatar generator (abstract shapes, colors, animals, etc.)
    - No correlation to account ID
    - Optionally rotated per-thread or per-session

This ensures that:

    - Users have recognizable identities within a thread
    - No long-term tracking is possible across threads
    - No real-world identity is exposed

---

## Timestamp Handling

To prevent correlation attacks:

### Timestamps are:
    - Rounded to coarse intervals (e.g., 5 minutes)
    - Or jittered randomly within a small window
    - Or replaced with relative time ("2 hours ago")

### Timestamps never include:
    - Millisecond precision
    - Client local time
    - Network-derived timing metadata

---

## Message Normalization

Before storage and distribution, messages are normalized:

### Normalization Steps
    - Remove HTML or unsafe markup
    - Normalize whitespace
    - Strip tracking links or metadata
    - Enforce maximum message length
    - Convert emojis to safe representations
    - Remove personally identifiable information (optional)

### Optional Safety Filters
    - Keyword moderation
    - Spam detection
    - Abuse filtering

These filters operate on the internal layer and do not affect anonymity.

---

## Privacy Guarantees

The message format enforces:

### No Real Identity Exposure
    - No usernames
    - No emails
    - No phone numbers
    - No account IDs

### No Network Metadata
    - No IP addresses
    - No device identifiers
    - No user-agent strings

### No Correlation Across Threads
    - Pseudonyms can rotate per-thread or per-session
    - Prevents long-term behavioral tracking

---

## Integration with Other Modules

The message format integrates with:

- **gateway.md**  
  Applies pseudonym assignment, metadata stripping, and normalization.

- **storage.md**  
  Stores public messages in append-only logs.

- **distribution.md**  
  Delivers compact public messages to clients.

- **vpn-access-layer/**  
  Ensures anonymous and censorship-resistant message submission.

---

## Summary

The anonymous BBS message format provides a strict separation between account identity and public identity.  
By using system-generated pseudonyms, minimal metadata, and privacy-preserving timestamps, it ensures that users can communicate safely and anonymously even in high-risk environments.
