## Dispatch Module Overview

The Dispatch module is responsible for delivering routed content to its final destination in a safe, deterministic, and metadata‑free manner. It acts as the final stage of the Emergency Channel processing pipeline, ensuring that validated submissions reach mirrors, NGOs, archives, or internal queues without exposing user identity, network metadata, or submission patterns.

Dispatching must rely exclusively on routing decisions produced by the Routing module. No semantic inspection, behavioral inference, or dynamic decision‑making is permitted at this stage.

## Dispatch Logic

Dispatching is a deterministic, metadata‑free process that delivers routed content to its final destination. The Dispatch module must not reinterpret routing decisions, perform semantic inspection, or introduce any new metadata. Its sole responsibility is to execute the delivery path defined by the Routing module.

### 1. Preconditions
A submission is eligible for dispatch only if:
- RoutingDecision is present
- RoutingDecision.destination is valid
- Integrity and Deduplication states are complete
- No pending quarantine or review flags exist

If any precondition fails, the submission is redirected to `internal_review`.

### 2. Dispatch Destinations
The module supports the following deterministic destinations:

- **mirror_outbound**  
  Sends content to public or semi‑public mirror endpoints.

- **ngo_outbound**  
  Sends content to trusted human rights organizations or partner networks.

- **archive_outbound**  
  Sends content to long‑term, tamper‑resistant archival storage.

- **internal_review**  
  Sends content to internal queues for manual structural verification.

- **quarantine**  
  Sends content to an isolated, non‑processing queue for invalid or unsafe submissions.

### 3. Dispatch Execution
Dispatching follows a strict, non‑semantic execution path:

1. Read `RoutingDecision.destination`
2. Validate destination against static configuration
3. Package sanitized content into a metadata‑free delivery envelope
4. Deliver via the appropriate outbound channel
5. Confirm delivery using a deterministic, non‑identifying acknowledgment

No additional logic, heuristics, or dynamic routing is permitted.

### 4. Delivery Envelope
All dispatched content must be wrapped in a minimal, metadata‑free envelope.

Fields:
- `payload` — sanitized content blob
- `structural_summary` — non‑semantic summary
- `destination` — final target
- `dispatch_id` — internal, non‑identifying reference

Forbidden:
- Timestamps
- Network metadata
- User identifiers
- Routing history
- Semantic annotations

### 5. Delivery Confirmation
Delivery confirmation must be:
- Deterministic
- Non‑identifying
- Free of timing or network metadata
- Independent of user identity or submission origin

Confirmation must never include:
- Payload hashes
- Delivery timestamps
- Endpoint identifiers
- Retry counts

### 6. No Dynamic Behavior
Dispatching must not:
- Retry adaptively
- Change destinations based on network conditions
- Perform load balancing
- Infer endpoint availability
- Modify content or metadata

All behavior must remain static, deterministic, and reproducible.

## Data Contracts

The Dispatch module uses strict, versioned data contracts to ensure deterministic delivery and safe interoperability with upstream and downstream systems. All fields must be metadata‑free and must not reveal user identity, timing, network details, or submission relationships.

### 1. DispatchInput
Represents a fully processed submission ready for final delivery.

Fields:
- `submission_id` — unique identifier for tracking
- `destination` — mirror_outbound | ngo_outbound | archive_outbound | internal_review | quarantine
- `payload` — sanitized content blob
- `structural_summary` — non‑semantic structural summary
- `dispatch_id` — internal, non‑identifying reference
- `size_bytes` — confirmed blob size

Rules:
- Must originate from a valid RoutingDecision
- Must not include timestamps, routing history, or semantic fields
- Must not include user‑derived metadata

### 2. DispatchEnvelope
Represents the metadata‑free wrapper used for outbound delivery.

Fields:
- `payload`
- `structural_summary`
- `destination`
- `dispatch_id`

Forbidden:
- Timestamps
- Network metadata
- Endpoint identifiers
- User identifiers
- Hashes or semantic annotations

### 3. DispatchResult
Represents the deterministic outcome of a dispatch operation.

Fields:
- `submission_id`
- `dispatch_id`
- `status` — delivered | quarantined | internal_review
- `reason` — generic, non‑diagnostic category
- `notes` — minimal internal notes (non‑semantic)

Rules:
- Must not include timing or network metadata
- Must not expose endpoint details
- Must not include payload hashes or fingerprints

### 4. DispatchError
Represents a failure in the dispatch process.

Fields:
- `submission_id`
- `reason` — generic error category (e.g., invalid_destination, envelope_error)
- `notes` — minimal internal notes (never content‑related)

Rules:
- Must not reveal internal routing logic
- Must not include sensitive metadata

## Error Handling

The Dispatch module is designed to fail safely, deterministically, and without revealing internal logic, network metadata, or user‑related information. Any inconsistency, invalid state, or unexpected condition must result in a controlled, non‑diagnostic rejection.

### 1. Invalid Destination
Triggered when the destination specified in `RoutingDecision` is missing, malformed, or unsupported.

Examples:
- Unknown destination value
- Destination not defined in static configuration
- Corrupted routing metadata

Handling:
- Route to `internal_review`
- Do not attempt to infer or correct the destination

### 2. Envelope Construction Failure
Triggered when the DispatchEnvelope cannot be created safely.

Examples:
- Missing payload
- Missing structural summary
- Invalid or conflicting dispatch_id
- Envelope contains forbidden metadata

Handling:
- Route to `internal_review`
- Do not attempt to reconstruct or sanitize the envelope

### 3. Delivery Channel Failure
Triggered when the outbound delivery channel cannot accept the envelope.

Examples:
- Channel unavailable
- Channel configuration mismatch
- Channel rejects metadata‑free envelope format

Handling:
- Route to `internal_review`
- Do not retry adaptively
- Do not switch channels dynamically

### 4. Non‑Deterministic Dispatch State
Triggered when dispatch behavior is not reproducible.

Examples:
- Inconsistent delivery outcomes across runs
- Non‑stable envelope construction
- Conflicting upstream metadata

Handling:
- Route to `internal_review`
- Do not apply heuristics or fallback logic

### 5. Integrity or Deduplication Drift
Triggered when upstream validation states appear inconsistent at dispatch time.

Examples:
- Integrity summary mismatch
- Deduplication flags inconsistent with canonical state

Handling:
- Route to `internal_review`
- Do not attempt to re‑validate upstream modules

### 6. Resource Exhaustion
Triggered when CPU, memory, or I/O limits are exceeded during dispatch.

Handling:
- Trigger safe failure path
- Route to `quarantine`
- Securely wipe intermediate data

### 7. Adversarial Inputs
Triggered when content or metadata appears intentionally crafted to manipulate dispatch behavior.

Examples:
- Destination spoofing
- Envelope poisoning attempts
- Repeated adversarial submissions

Handling:
- Route to `quarantine`
- Apply temporary rate limiting
- Log anonymized adversarial indicators only

## Security Notes

The Dispatch module enforces strict security guarantees to ensure that no sensitive information is introduced, inferred, or leaked during final delivery. All dispatch operations must preserve anonymity, avoid semantic interpretation, and maintain deterministic behavior.

### 1. No Identity or Origin Inference
- Dispatching must never infer user identity, authorship, device characteristics, or submission origin.
- Delivery behavior must not depend on network patterns, timing, or endpoint availability.
- No stylistic, linguistic, or behavioral analysis is permitted.

### 2. Deterministic Dispatch Behavior
- Dispatch outcomes must be fully deterministic and reproducible.
- Identical inputs must always produce identical dispatch results.
- Randomized delivery, adaptive retries, or ML‑based dispatch logic are prohibited.

### 3. Zero Metadata Introduction
- Dispatch must not introduce metadata that could reveal timing, location, or user behavior.
- Delivery envelopes must remain minimal and metadata‑free.
- No timestamps, network identifiers, or diagnostic fields may be added.

### 4. Isolation from Raw Content
- The module never receives raw, unsanitized content.
- All payloads must originate from validated upstream modules.
- Any attempt to bypass sanitization must result in quarantine routing.

### 5. Minimal Logging
- Logs may contain only anonymized operational metrics.
- No payloads, structural summaries, or routing decisions may be logged.
- Logs must be purgeable and non‑persistent in hostile environments.

### 6. Defense Against Correlation Attacks
- Dispatch behavior must not enable cross‑submission correlation.
- Delivery acknowledgments must be non‑identifying and free of timing metadata.
- No fine‑grained delivery categories or semantic‑based routing paths are permitted.

### 7. Static Channel Configuration
- Outbound channels must be static and environment‑independent.
- Channel selection must not depend on content semantics or user behavior.
- Channel configuration must not reveal internal thresholds or logic.

These safeguards ensure that the Dispatch module maintains strong anonymity guarantees and prevents any form of identity inference, behavioral profiling, or cross‑submission linkage during final delivery.
