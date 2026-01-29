## Sanitization Pipeline Overview

The Sanitizer Pipeline defines the full sequence of operations applied to incoming content to ensure complete metadata removal, fingerprint neutralization, and format normalization. Each stage runs inside an isolated sandbox and produces deterministic, correlation‑resistant output.

The pipeline is designed to handle text, images, videos, and documents using format‑specific routines while maintaining strict anonymity guarantees.

## Pipeline Stages

The Sanitization Pipeline consists of five deterministic stages. Each stage is isolated, reproducible, and designed to eliminate metadata, fingerprints, and format‑specific identifiers while preserving the semantic content.

### 1. Intake & Sandbox Initialization
- Receives validated content from the Validation module.
- Initializes a secure, ephemeral sandbox environment.
- Loads format‑specific sanitization routines.

### 2. Format Detection & Routing
- Identifies the true file type using magic‑byte inspection.
- Rejects ambiguous or unsupported formats.
- Routes content to the appropriate sanitization path:
  - Text
  - Image
  - Video
  - Document

### 3. Metadata Extraction & Removal
- Scans for EXIF, IPTC, XMP, PDF metadata, and embedded identifiers.
- Removes GPS coordinates, device signatures, encoder tags, and revision history.
- Normalizes headers and container structures.

### 4. Re‑Encoding & Fingerprint Neutralization
- Re‑encodes media using deterministic settings.
- Converts proprietary formats into safe, open formats.
- Removes latent fingerprints such as:
  - Encoder noise patterns
  - Watermarks
  - Deterministic compression artifacts

### 5. Output Normalization & Packaging
- Ensures consistent output structure across all sanitized content.
- Compresses or resizes media when required for safety.
- Produces a sanitized blob and a sanitization report for downstream modules.

## Format‑Specific Pipelines

Each content type follows a dedicated sanitization pipeline tailored to its metadata structures, fingerprint risks, and encoding characteristics. All pipelines are deterministic and executed inside an isolated sandbox.

### 1. Text Pipeline
Steps:
- Normalize encoding to UTF‑8.
- Remove hidden Unicode markers (BOM, RLO, LRO, ZWJ, ZWNJ).
- Strip embedded metadata blocks (front‑matter, editor tags).
- Collapse excessive whitespace and normalize line endings.

Output:
- Clean, UTF‑8 text with no hidden markers or structural identifiers.

### 2. Image Pipeline
Steps:
- Decode image into raw pixel buffer.
- Remove EXIF, IPTC, XMP, and embedded thumbnails.
- Normalize color profiles (sRGB only).
- Re‑encode using deterministic JPEG/PNG/WebP settings.

Output:
- Metadata‑free image with neutralized encoder fingerprints.

### 3. Video Pipeline
Steps:
- Extract raw frames and audio tracks.
- Remove container metadata, GPS tracks, and encoder signatures.
- Normalize frame rate, resolution, and color space.
- Re‑encode using deterministic MP4/WebM profiles.

Output:
- Clean, re‑encoded video with no device or encoder identifiers.

### 4. Document Pipeline
Steps:
- Convert DOCX/ODT to sanitized PDF/A.
- Remove embedded metadata, object streams, and revision history.
- Flatten layers, annotations, and embedded media.
- Rebuild the document using deterministic PDF/A settings.

Output:
- Sanitized PDF/A with no embedded metadata or hidden structures.

## Fallback Paths

The Sanitization Pipeline includes controlled fallback paths to ensure deterministic behavior even when primary routines fail. Fallbacks are strictly limited and must never compromise anonymity or metadata safety.

### 1. Format Conversion Fallback
Used when the original format cannot be safely sanitized.

Examples:
- Corrupted JPEG headers → convert to PNG
- Unsupported DOCX features → convert to PDF/A
- Non‑standard WebM profiles → convert to MP4 (deterministic profile)

Rules:
- Conversion must always produce a deterministic, metadata‑free format.
- Original format must be discarded after conversion.

### 2. Re‑Encoding Fallback
Used when the primary encoder fails due to corruption or unsupported profiles.

Examples:
- H.264 profile mismatch → fallback to baseline H.264
- PNG alpha channel issues → fallback to sanitized PNG without alpha
- WebP decoding errors → fallback to JPEG

Rules:
- Fallback encoders must use deterministic settings.
- No non‑deterministic compression routines are allowed.

### 3. Structural Repair Fallback
Used when minor structural inconsistencies can be safely corrected.

Examples:
- Missing EOF markers in PDFs
- Non‑critical container padding errors
- Minor timestamp inconsistencies in video containers

Rules:
- Repairs must not alter semantic content.
- If repair fails, the content must be rejected.

### 4. Safe Failure Path
If all fallback paths fail, the pipeline must reject the content.

Rules:
- Rejection must be generic and non‑diagnostic.
- No partial sanitization results may be returned.
- Unsafe blobs must be securely wiped from the sandbox.

## Resource Controls

The Sanitization Pipeline enforces strict resource controls to prevent denial‑of‑service attacks, sandbox escapes, and excessive computation. All limits are deterministic and applied uniformly across all content types.

### 1. CPU Limits
- Each sanitization task receives a fixed CPU quota.
- Long‑running operations are terminated deterministically.
- Re‑encoding routines must complete within predefined time windows.

### 2. Memory Limits
- The sandbox enforces strict memory ceilings.
- Large media files may be downscaled or rejected.
- No in‑memory caching of original content is permitted.

### 3. Disk I/O Limits
- Temporary files are stored in an ephemeral, isolated filesystem.
- Maximum temporary storage per task is capped.
- All temporary data is securely wiped after sanitization.

### 4. Execution Time Limits
- Each stage of the pipeline has a maximum execution time.
- Fallback paths must also respect global time limits.
- Tasks exceeding limits are terminated and marked as failed.

### 5. Parallelization Controls
- Only a limited number of sanitization tasks may run concurrently.
- High‑load conditions trigger queueing or rate limiting.
- No task may spawn additional subprocesses outside the sandbox.

### 6. Safety‑First Termination
- Any resource exhaustion triggers a safe failure path.
- No partial output is returned.
- The sandbox is destroyed immediately after termination.

## Security Notes

The Sanitization Pipeline enforces strict security guarantees to ensure that no metadata, fingerprints, or identifiers survive the sanitization process. These safeguards must remain enabled at all times.

### 1. Zero Metadata Retention
- No EXIF, IPTC, XMP, PDF metadata may remain after sanitization.
- All container headers must be rebuilt using deterministic templates.
- Hidden thumbnails, GPS tracks, and revision histories must be removed.

### 2. Deterministic Re‑Encoding
- All re‑encoding routines must use fixed, deterministic parameters.
- Identical inputs must always produce identical sanitized outputs.
- Non‑deterministic encoders or compression algorithms are prohibited.

### 3. Sandbox Isolation
- All sanitization operations must occur inside an isolated sandbox.
- No network access is permitted during sanitization.
- Temporary files must be securely wiped after use.

### 4. No Partial Sanitization
- If any metadata or fingerprint cannot be removed, the content must be rejected.
- Partial or best‑effort sanitization is not allowed.
- Rejection messages must not reveal internal sanitization logic.

### 5. Minimal Logging
- Only anonymized operational metrics may be logged.
- No content, metadata details, or hashes may appear in logs.
- Logs must be purgeable and non‑persistent in hostile environments.

### 6. Defense Against Correlation Attacks
- Re‑encoding must remove encoder signatures and deterministic artifacts.
- Proprietary formats must be converted to safe, open formats.
- Fingerprint neutralization must be applied consistently across all media.

These rules ensure that the Sanitization Pipeline provides strong anonymity guarantees and prevents any form of user traceability.
