## Sanitizer Overview

The Sanitizer module is responsible for removing all metadata, fingerprints, and embedded identifiers from incoming content. It ensures that no device, location, software, or user trace remains before the content enters the processing pipeline.

Sanitization is mandatory for all submissions and operates in a secure sandbox to prevent leakage or correlation. The module supports text, image, video, and document formats, applying format‑specific routines to guarantee anonymity.
