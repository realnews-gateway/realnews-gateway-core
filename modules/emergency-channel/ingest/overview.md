# Ingest Module — Overview

## Purpose

The Ingest module is the entry point of the Emergency Channel pipeline.  
Its primary role is to receive raw, untrusted content from external sources and prepare it for downstream processing.  
It ensures that all incoming content is collected, validated, normalized, and safely forwarded to the Sanitizer module.

The module establishes strict trust boundaries and prevents malformed or harmful content from entering the system.

---

## Responsibilities

The Ingest module is responsible for:

- **Source intake**  
  Receiving content from user submissions, crawlers, partner feeds, mirror nodes, and fallback channels.

- **Initial validation**  
  Ensuring the content is well‑formed and not obviously malicious.

- **Normalization**  
  Converting diverse formats into a consistent internal representation.

- **Metadata extraction**  
  Extracting non‑sensitive metadata such as timestamps, language, and source type.

- **Forwarding to Sanitizer**  
  Passing normalized content to the Sanitizer module for deep cleaning and verification.
