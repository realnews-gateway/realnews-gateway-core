# Fetcher: Content Retrieval and Transport Fallback

The fetcher is responsible for retrieving content from all registered news sources.  
It integrates with the VPN access layer to ensure reliable fetching under censorship, network instability, or partial source failure.  
The fetcher implements retries, fallback transports, mirror rotation, and region-aware behavior.

This document describes the fetching pipeline, error handling, and transport strategies.

---

## Objectives

The fetcher provides:

- Reliable content retrieval from diverse sources
- Automatic retries and exponential backoff
- Transport fallback (TLS → QUIC → HTTP/2 → CDN)
- Mirror rotation for failing sources
- Region-aware fetching strategies
- Minimal metadata leakage
- Integration with parser and deduplication modules

These capabilities ensure continuous ingestion even under hostile network conditions.

---

## Fetching Pipeline

The fetching pipeline consists of four stages:

### 1. Source Selection
    - Selects a source from the region-aware source set
    - Prioritizes high-reliability sources
    - Falls back to mirrors if the primary source fails

### 2. Transport Selection
    - Attempts the preferred transport (TLS or QUIC)
    - Falls back to HTTP/2 or CDN if blocked
    - Uses region-aware fallback chains

### 3. Request Execution
    - Sends HTTP(S) requests with minimal headers
    - Avoids identifiable patterns
    - Applies rate limiting and jitter
    - Handles redirects and content negotiation

### 4. Response Handling
    - Validates content type
    - Extracts raw HTML or XML
    - Passes content to the parser module

---

## Transport Fallback Strategy

The fetcher integrates with the VPN access layer’s fallback logic:

### Default Fallback Chain
    QUIC → TLS → HTTP/2 → CDN → Mirror → Disabled

### High-Censorship Regions
    TLS → HTTP/2 → CDN → Mirror → Proxy → Disabled

### Low-Risk Regions
    QUIC → TLS → HTTP/2

Fallback transitions are:

    - Silent
    - Region-aware
    - Error-normalized
    - Indistinguishable from real browser behavior

---

## Request Behavior

The fetcher uses browser-like request patterns:

### Headers
    - User-Agent: Platform-specific (from client-profiles)
    - Accept: text/html, application/xml
    - Accept-Language: Region-specific
    - No tracking headers
    - No custom identifiers

### Timing
    - Randomized jitter
    - Platform-specific timing profiles
    - Retry backoff: 1s → 2s → 4s → 8s

### Connection Behavior
    - Reuses connections when possible
    - Rotates TLS fingerprints based on platform
    - Uses SNI strategies from camouflage module

---

## Error Handling

The fetcher normalizes errors to avoid detection:

### Network Errors
    - Timeout → Retry with fallback transport
    - Connection reset → Retry with mirror
    - DNS failure → Switch to CDN or proxy

### HTTP Errors
    - 429 → Backoff and retry
    - 503 → Retry with mirror
    - 404 → Mark source as unstable

### Parsing Errors
    - Pass to parser for recovery
    - If unrecoverable, mark source as low reliability

---

## Mirror Rotation

When a source fails:

    1. Retry with fallback transport
    2. Retry with mirror #1
    3. Retry with mirror #2
    4. Switch to CDN-backed mirror
    5. Temporarily disable source

Disabled sources are periodically rechecked.

---

## Region-Aware Fetching

The fetcher adapts to regional conditions:

### High-Censorship Regions
    - Avoids QUIC
    - Prefers TLS with stable fingerprints
    - Uses CDN-fronting when necessary
    - Increases retry count

### Low-Risk Regions
    - Prefers QUIC for performance
    - Uses minimal fallback

### Unstable Regions
    - Aggressive mirror rotation
    - Shorter retry intervals

---

## Integration with Other Modules

The fetcher integrates with:

- **sources.md**  
  Provides URLs, mirrors, and region-aware source sets.

- **parser.md**  
  Receives raw HTML/XML for normalization.

- **deduplication.md**  
  Receives structured content for duplicate detection.

- **classification.md**  
  Receives normalized content for topic classification.

- **vpn-access-layer/**  
  Provides fallback transports and DPI evasion.

---

## Summary

The fetcher provides reliable, censorship-resistant content retrieval using transport fallback, mirror rotation, and region-aware behavior.  
By integrating with the VPN access layer and maintaining browser-like request patterns, it ensures continuous ingestion even under hostile network conditions.
