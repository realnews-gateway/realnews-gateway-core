# Emergency Channel — Video Sanitizer

## 1. Purpose

The Video Sanitizer ensures that all incoming video content is safe, valid, and compliant with the Emergency Channel’s publishing requirements.  
It removes harmful metadata, validates container and codec integrity, enforces duration and resolution limits, and prepares videos for downstream processing such as storage and distribution.

The sanitizer must operate deterministically and avoid altering the semantic meaning of the video unless required for safety or policy compliance.

---

## 2. Responsibilities

The Video Sanitizer is responsible for:

### 2.1 Security Filtering

- Detect and block embedded executable payloads  
- Remove malicious or unsupported metadata fields  
- Validate container structure and codec signatures  
- Reject disguised or malformed video files  

### 2.2 Format Normalization

- Convert unsupported containers to MP4 (H.264/AAC)  
- Normalize frame rate and color space  
- Remove unsupported audio tracks or subtitles  
- Ensure deterministic encoding parameters  

### 2.3 Policy Enforcement

- Enforce maximum duration  
- Enforce maximum resolution and bitrate  
- Validate aspect ratio constraints  
- Reject corrupted or partially downloaded videos  

### 2.4 Metadata Sanitization

- Remove EXIF-like metadata (GPS, device info, timestamps)  
- Remove embedded thumbnails or preview frames  
- Remove application-specific metadata blocks  
- Strip subtitle tracks unless explicitly allowed  

Metadata removal protects user privacy and reduces fingerprinting risks.
