## Validation Overview

The Validation module ensures that all incoming submissions meet the minimum structural, security, and format requirements before entering the main processing pipeline. It acts as the first defensive layer, preventing malformed, dangerous, or suspicious payloads from reaching downstream components.

Validation is intentionally strict to protect the system from adversarial inputs, corrupted files, and metadata‑bearing formats that could compromise user anonymity.

## Validation Stages

The Validation module performs four sequential checks to ensure that all submissions entering the pipeline are structurally sound, safe to process, and free of identifiable metadata.

### 1. Structural Validation
- Confirms that the payload conforms to the SubmissionPayload contract.
- Verifies required fields: encrypted_blob, submission_token, timestamp.
- Rejects malformed or incomplete payloads.

### 2. Cryptographic Validation
- Validates the submission token using a time‑bound signature.
- Ensures the encrypted blob has not been tampered with.
- Applies rate‑limit checks to prevent abuse.

### 3. Format Validation
- Detects file type using magic‑byte inspection (not user‑provided extensions).
- Rejects unsupported or dangerous formats (e.g., executables, archives).
- Normalizes ambiguous formats into a safe intermediate representation.

### 4. Metadata Pre‑Check
- Performs a shallow scan for EXIF, XMP, IPTC, PDF metadata.
- Flags content that requires deep sanitization.
- Rejects files containing unremovable or suspicious metadata structures.

## Rejected Payload Categories

The Validation module rejects any submission that poses structural, security, or anonymity risks. Rejections are intentionally conservative to protect users and downstream components.

### 1. Malformed Payloads
Submissions that fail basic structural or cryptographic checks.
Examples:
- Missing required fields
- Corrupted encrypted blobs
- Invalid or expired submission tokens

### 2. Dangerous File Formats
Files that cannot be safely sanitized or pose execution risks.
Examples:
- Executables (EXE, ELF, MACH‑O)
- Archives (ZIP, RAR, 7Z)
- Scripts (JS, SH, BAT, PS1)

### 3. Unsupported Media Types
Formats that the sanitization module cannot reliably process.
Examples:
- Proprietary camera RAW formats
- Encrypted PDFs
- DRM‑protected documents

### 4. Metadata‑Heavy Files
Files containing metadata structures that cannot be fully removed.
Examples:
- PDF files with embedded object streams
- Images with proprietary EXIF blocks
- Videos with unremovable GPS tracks

### 5. Oversized Submissions
Payloads exceeding safe processing limits.
Examples:
- Extremely large video files
- Multi‑gigabyte documents
- Payloads exceeding configured thresholds

### 6. Suspicious or Adversarial Inputs
Content exhibiting patterns associated with attacks or deanonymization attempts.
Examples:
- Steganographic payloads
- Files with mismatched magic bytes and extensions
- Payloads designed to trigger sanitization failures.

## Validation Data Contracts

The Validation module exchanges data with upstream and downstream components using strict, versioned data contracts. These contracts ensure that only safe, well‑structured submissions enter the main pipeline.

### 1. ValidationInput
Represents the raw submission received from the access layer.

Fields:
- `encrypted_blob` — Raw encrypted payload.
- `submission_token` — Time‑bound authentication token.
- `timestamp` — Client‑provided submission time.
- `client_metadata` — Transport‑layer metadata (removed after validation).

### 2. ValidationResult
Represents the outcome of the validation process.

Fields:
- `is_valid` — Boolean indicating whether validation succeeded.
- `reason` — Optional rejection reason (generic, non‑diagnostic).
- `normalized_type` — Detected file type (text/image/video/document).
- `requires_sanitization` — Whether deep sanitization is required.
- `safe_blob` — Normalized content passed to the sanitization module.

### 3. RejectionRecord
Represents a rejected submission for internal auditing.

Fields:
- `timestamp` — Time of rejection.
- `category` — Rejection category (malformed, dangerous_format, etc.).
- `anonymized_hash` — Hash of encrypted blob (cannot identify user).
- `notes` — Minimal operational notes for debugging.

### 4. ValidationReport
Represents a detailed internal report used only for system monitoring.

Fields:
- `validation_time_ms` — Total time spent validating.
- `detected_format` — File type detected via magic‑byte inspection.
- `metadata_flags` — Flags indicating presence of metadata.
- `risk_score` — Internal heuristic score for anomaly detection.

## Error Handling

The Validation module is designed to fail safely and predictably. Any ambiguity, corruption, or suspicious behavior results in immediate rejection with minimal information disclosed to the client.

### 1. Structural Errors
Triggered when the payload does not match the expected SubmissionPayload contract.

Examples:
- Missing encrypted_blob
- Invalid timestamp format
- Malformed submission_token

Handling:
- Reject with a generic “invalid submission” response
- Do not expose which field failed

### 2. Cryptographic Errors
Triggered when authentication or integrity checks fail.

Examples:
- Expired submission token
- Signature mismatch
- Tampered encrypted blob

Handling:
- Reject immediately
- Regenerate a new token only when safe
- Log anonymized failure metadata

### 3. Format Errors
Triggered when the file type is unsupported or dangerous.

Examples:
- Executable or script detected
- Archive file detected
- Magic‑byte mismatch

Handling:
- Reject without attempting sanitization
- Do not reveal detected file type to the client

### 4. Metadata Errors
Triggered when metadata structures cannot be safely removed.

Examples:
- Embedded GPS tracks
- Proprietary EXIF blocks
- PDF object streams

Handling:
- Reject with a generic “unsafe content” response
- Flag for internal monitoring

### 5. Size Errors
Triggered when payload exceeds configured limits.

Examples:
- Oversized video files
- Multi‑gigabyte documents

Handling:
- Reject and instruct client to reduce size (generic wording)
- Do not reveal system thresholds

### 6. Adversarial Behavior
Triggered when content appears intentionally crafted to bypass validation.

Examples:
- Steganographic payloads
- Files designed to crash sanitization
- Repeated malformed submissions

Handling:
- Reject and apply temporary rate limiting
- Log anonymized adversarial indicators.

## Security Notes

The Validation module enforces strict security guarantees to protect user anonymity and prevent malicious inputs from entering the system. These safeguards must never be bypassed or weakened.

### 1. Zero Identity Leakage
- No client identifiers may be stored or forwarded.
- Transport metadata must be discarded immediately after validation.
- Validation logs must never include IPs, device IDs, or session tokens.

### 2. Strict Format Enforcement
- Only explicitly approved formats may proceed to sanitization.
- Ambiguous or proprietary formats must be rejected by default.
- Magic‑byte detection must override user‑provided file extensions.

### 3. Metadata Sensitivity
- Any presence of unremovable metadata requires immediate rejection.
- Validation must not attempt partial sanitization.
- Metadata flags must be passed to downstream modules for deep cleaning.

### 4. Fail‑Closed Behavior
- Any uncertainty or anomaly results in rejection.
- No partially validated content may enter the pipeline.
- Error messages must remain generic to avoid leaking internal logic.

### 5. Minimal Logging
- Only anonymized operational metrics may be recorded.
- No content, hashes, or metadata details may appear in logs.
- Logs must be purgeable and non‑persistent in hostile deployments.

### 6. Defense Against Adversarial Inputs
- Validation must detect and reject steganographic or malformed payloads.
- Repeated adversarial submissions must trigger temporary rate limiting.
- Internal risk scoring must remain isolated from client‑visible behavior.

These rules ensure that the Validation module remains a robust, anonymous, and attack‑resistant entry point for the Emergency Channel.
