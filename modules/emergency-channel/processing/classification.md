## Classification Module Overview

The Classification module is responsible for determining the semantic category of sanitized content. It operates exclusively on metadata‑free inputs and uses deterministic, lightweight heuristics to assign a normalized classification tag.

The module must never infer identity, origin, authorship, or device characteristics. Its sole purpose is to categorize content for downstream processing, routing, and distribution.

## Classification Categories

The Classification module assigns one of four normalized categories to each sanitized submission. These categories are intentionally broad to prevent over‑fitting, identity inference, or stylistic fingerprinting.

### 1. text
Represents plain text content.

Examples:
- User‑submitted reports
- Transcribed statements
- Short notes or descriptions

Characteristics:
- UTF‑8 encoded
- No embedded media
- No formatting metadata

### 2. image
Represents still images.

Examples:
- Photos
- Screenshots
- Scanned documents (treated as images unless OCR is explicitly enabled downstream)

Characteristics:
- Deterministic pixel data
- No EXIF or embedded metadata
- Validated dimensions and color profile

### 3. video
Represents moving‑image content.

Examples:
- Short clips
- Screen recordings
- Camera footage

Characteristics:
- Valid container structure
- Deterministic frame count and duration
- No embedded metadata or sidecar files

### 4. document
Represents structured documents.

Examples:
- PDF/A files
- Sanitized multi‑page reports
- Text‑heavy submissions requiring pagination

Characteristics:
- PDF/A compliant
- Deterministic page count
- No embedded metadata, annotations, or revision history

## Classification Heuristics

The Classification module uses deterministic, lightweight heuristics to assign a category to sanitized content. These heuristics rely exclusively on structural properties and never on stylistic, behavioral, or identity‑linked features.

### 1. Text Heuristics
- Input is valid UTF‑8.
- Contains no binary data or embedded media signatures.
- Line count and character distribution fall within expected ranges.
- MIME sniffing confirms plain text characteristics.

### 2. Image Heuristics
- Blob begins with a recognized image header (e.g., PNG, JPEG, WebP).
- Pixel matrix is structurally valid and deterministic.
- No EXIF, ICC, or embedded metadata blocks are present.
- Dimensions fall within safe, non‑pathological ranges.

### 3. Video Heuristics
- Container header matches a supported format (e.g., MP4, MKV).
- Frame count and duration fields are internally consistent.
- No sidecar metadata, subtitles, or embedded streams remain.
- Video and audio tracks (if present) follow deterministic encoding rules.

### 4. Document Heuristics
- File is PDF/A compliant after sanitization.
- Page count is deterministic and extractable.
- No annotations, embedded files, or revision history remain.
- Structural markers (catalog, pages, objects) follow normalized patterns.

### 5. Ambiguity Handling
If heuristics produce conflicting signals:
- Apply deterministic fallback classification.
- If ambiguity persists, reject the submission.
- Never infer classification based on content semantics or style.

## Data Contracts

The Classification module uses strict, versioned data contracts to ensure deterministic behavior and safe interoperability with upstream and downstream components. All fields are metadata‑free and must not contain any information that could compromise anonymity.

### 1. ClassificationInput
Represents sanitized content received from the Processing Pipeline.

Fields:
- `submission_id` — unique identifier for tracking
- `content_type_hint` — optional hint from Sanitizer (non‑binding)
- `sanitized_blob` — metadata‑free content
- `size_bytes` — size after sanitization

### 2. ClassificationResult
Represents the output of the classification process.

Fields:
- `submission_id`
- `classification_tag` — one of: text | image | video | document
- `confidence` — deterministic confidence indicator (low/medium/high)
- `warnings` — optional non‑diagnostic notes

### 3. ClassificationError
Represents a classification failure.

Fields:
- `submission_id`
- `reason` — generic, non‑diagnostic error category
- `notes` — minimal internal notes (never content‑related)

### 4. NormalizedClassificationTag
A standardized tag used across all downstream modules.

Values:
- `text`
- `image`
- `video`
- `document`

Rules:
- Tags must be lowercase.
- Tags must be deterministic.
- No subcategories or fine‑grained labels are permitted.

## Error Handling

The Classification module is designed to fail safely, deterministically, and without revealing internal logic. Any ambiguity, corruption, or structural inconsistency results in a controlled rejection.

### 1. Structural Errors
Triggered when sanitized content is malformed or incomplete.

Examples:
- Invalid UTF‑8 sequences
- Corrupted image headers
- Video container inconsistencies
- PDF/A structural anomalies

Handling:
- Reject with a generic “invalid structure” reason
- Do not attempt deep repair or inference

### 2. Ambiguous Classification
Triggered when heuristics cannot confidently determine the content type.

Examples:
- Extremely short text
- Images with ambiguous MIME signatures
- Videos missing non‑critical metadata
- PDF files with insufficient structural markers

Handling:
- Apply deterministic fallback classification
- If ambiguity persists, reject the submission

### 3. Unsupported Formats
Triggered when sanitized content uses a format outside the supported set.

Examples:
- Exotic image formats
- Unsupported video containers
- Non‑PDF/A documents

Handling:
- Reject with a generic “unsupported format” reason
- Do not attempt conversion or normalization

### 4. Resource Exhaustion
Triggered when CPU, memory, or I/O limits are exceeded.

Handling:
- Trigger safe failure path
- Do not return partial results
- Securely wipe intermediate data

### 5. Adversarial Inputs
Triggered when content appears intentionally crafted to confuse classifiers.

Examples:
- Header spoofing
- Mixed‑signal blobs (e.g., text disguised as image)
- Repeated adversarial submissions

Handling:
- Reject and apply temporary rate limiting
- Log anonymized adversarial indicators only

## Security Notes

The Classification module enforces strict security guarantees to ensure that no sensitive information is introduced, inferred, or leaked during the classification process. All operations must preserve anonymity and maintain deterministic behavior.

### 1. No Identity or Origin Inference
- The module must never infer user identity, authorship, origin, or device characteristics.
- Classification relies solely on structural properties of sanitized content.
- No stylistic, linguistic, or behavioral analysis is permitted.

### 2. Deterministic Classification
- Classification must be fully deterministic and reproducible.
- Identical sanitized inputs must always produce identical classification results.
- Randomized algorithms, adaptive heuristics, or ML‑based inference are prohibited.

### 3. Zero Metadata Introduction
- The module must not introduce any metadata that could reveal timing, location, or user behavior.
- Confidence indicators must be deterministic and non‑probabilistic.
- No hidden fields or diagnostic metadata may be added.

### 4. Isolation from Raw Content
- The module never receives raw, unsanitized content.
- All inputs must originate from the Sanitizer or Processing Pipeline.
- Any attempt to bypass sanitization must trigger immediate rejection.

### 5. Minimal Logging
- Logs may contain only anonymized operational metrics.
- No content, hashes, or derived metadata may be logged.
- Logs must be purgeable and non‑persistent in hostile environments.

### 6. Defense Against Correlation Attacks
- Classification tags must be broad and non‑specific.
- No fine‑grained categories or semantic labels are permitted.
- Classification results must not enable cross‑submission correlation.

These safeguards ensure that the Classification module maintains strong anonymity guarantees and safely prepares content for downstream processing.
