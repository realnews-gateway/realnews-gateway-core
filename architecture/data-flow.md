# Data Flow

This document describes the end-to-end data flow of the Emergency Publishing System. It explains how content enters the system, how it is normalized and sanitized, how it moves through the Emergency Channel, and how it is distributed under hostile network conditions.

The system has two ingestion paths:
1. News Aggregation Path
2. Pseudonymous BBS Path

Both paths converge in the Emergency Channel.

---

## High-Level Flow Summary

    External Sources
        ↓
    news-aggregation module
        ↓
    Emergency Channel
        ↓
    Multi-Protocol Output

    User Devices
        ↓
    anonymous-bbs module
        ↓
    Emergency Channel
        ↓
    Multi-Protocol Output

---

## 1. News Aggregation Path

### 1.1 Source Registry
- Maintains list of feeds and region-specific sources
- Supports dynamic updates and failover
- Defines fetch intervals and parsing rules

### 1.2 Fetcher
- Retrieves raw content
- Handles retries, timeouts, fallback URLs
- Uses vpn-access-layer for region-aware routing

### 1.3 Parser
- Converts HTML, JSON, or XML into structured internal objects
- Extracts title, body, timestamp, and media
- Removes tracking parameters and fingerprintable elements

### 1.4 Deduplication
- Uses hash-based and semantic similarity checks
- Prevents mirrored or repeated content

### 1.5 Classification
- Assigns topic labels
- Supports region-aware classification rules

### 1.6 Handoff to Emergency Channel
    type: news
    region: <region>
    topic: <topic>
    content: <text>
    metadata: <normalized metadata>

---

## 2. Pseudonymous BBS Path

### 2.1 Account Login
- Lightweight account system
- No personal identifiers required
- Region-aware access rules

### 2.2 Pseudonym Generation
- System-generated display name and avatar
- Prevents identity correlation

### 2.3 Post Creation
- User writes a message
- Client strips local metadata
- Server sanitizes HTML and embedded content

### 2.4 Sanitization
- Removes dangerous markup
- Normalizes text
- Ensures no fingerprintable artifacts remain

### 2.5 Handoff to Emergency Channel
    type: bbs
    region: <region>
    author: <generated pseudonym>
    content: <text>
    metadata: <normalized metadata>

---

## 3. Emergency Channel Processing

### 3.1 Sanitization Layer
- Removes remaining metadata
- Normalizes timestamps
- Ensures uniform internal format

### 3.2 Storage Layer
- Stores messages in encrypted form
- Supports region-aware retention policies
- Maintains minimal metadata for routing

### 3.3 Routing Layer
- Determines delivery path
- Applies multi-hop routing rules
- Selects fallback transports when necessary

### 3.4 Distribution Layer
- Converts messages into multiple output formats
- Selects protocol based on region and threat level
- Supports offline and low-bandwidth modes

---

## 4. Multi-Protocol Output

Supported transports include:
- HTTPS (stealth mode)
- DNS tunneling
- CDN-backed distribution
- Covert channels (timing, padding, mimicry)
- Region-specific fallback transports

Selection factors:
- Network conditions
- Censorship intensity
- User region
- Transport availability

---

## Summary

The Emergency Publishing System processes data through two ingestion paths: news aggregation and pseudonymous posting. Both paths converge in the Emergency Channel, which performs sanitization, storage, routing, and multi-protocol distribution. This ensures secure and resilient delivery under hostile network conditions.
