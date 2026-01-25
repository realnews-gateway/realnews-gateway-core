# Emergency Channel — Image Sanitizer

## 1. Purpose

The Image Sanitizer ensures that all incoming image content is safe, valid, and compliant with the Emergency Channel’s publishing requirements.  
It removes harmful metadata, validates formats, enforces size and resolution limits, and prepares images for downstream processing such as storage and distribution.

The sanitizer must operate deterministically and avoid altering the visual meaning of the image unless required for safety.

---

## 2. Responsibilities

The Image Sanitizer is responsible for:

### 2.1 Security Filtering

- Strip all embedded scripts or executable payloads  
- Remove malicious or unsupported metadata fields  
- Validate MIME type and reject disguised formats  
- Detect and block steganographic payloads when possible  

### 2.2 Format Normalization

- Convert unsupported formats to safe, standardized formats (e.g., PNG or JPEG)  
- Normalize color profiles  
- Remove alpha channels if not allowed  
- Ensure consistent encoding  

### 2.3 Policy Enforcement

- Enforce maximum file size  
- Enforce maximum resolution  
- Validate aspect ratio constraints  
- Reject corrupted or partially downloaded images  

### 2.4 Metadata Sanitization

- Remove EXIF data (GPS, device info, timestamps)  
- Remove ICC profiles unless required  
- Remove thumbnails and embedded previews  
- Remove application-specific metadata blocks  

Metadata removal protects user privacy and reduces fingerprinting risks.
