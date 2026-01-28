# Client Profiles

The Client Profiles subsystem provides platform‑specific configuration templates for connecting to the VPN Access Layer.  
Each profile defines recommended transport preferences, fallback behavior, camouflage settings, and system‑level considerations for a specific operating system.

These profiles ensure consistent, censorship‑resistant access across mobile, desktop, and embedded environments.

---

## Contents

This directory includes platform‑specific profiles:

- **android.md**  
  QUIC‑first mobile profile optimized for unstable networks.

- **ios.md**  
  TLS‑first profile aligned with iOS system constraints and TLS behavior.

- **windows.md**  
  Desktop‑optimized profile with hybrid TLS/QUIC strategy.

- **linux.md**  
  Flexible profile for servers, desktops, and CLI environments.

- **mac.md**  
  TLS‑first profile optimized for macOS network stack and Safari/Chrome fingerprints.

- **embedded.md**  
  Lightweight profile for routers, IoT devices, and constrained hardware.

- **client-profiles.md**  
  High‑level specification of client profile behavior and integration.

---

## Purpose

Client Profiles enable:

- Platform‑optimized access configurations  
- Predefined protocol preferences (TLS, QUIC, HTTP, CDN)  
- Region‑aware fallback chains  
- Camouflage‑compatible TLS/QUIC fingerprints  
- Minimal user configuration for fast onboarding  
- Consistent behavior across all supported platforms  

They provide a standardized way to deploy the VPN Access Layer across diverse environments.

---

## Design Principles

All client profiles follow these principles:

- **Platform awareness**  
  Each OS has unique network behavior and constraints.

- **Stealth by default**  
  Profiles prioritize camouflage‑compatible transports.

- **Resilience**  
  Built‑in fallback chains ensure persistent connectivity.

- **Minimal configuration**  
  Users should be able to connect with minimal manual setup.

- **Extensibility**  
  New platforms can be added without breaking existing ones.

---

## Integration

Client Profiles integrate with:

- **entrypoints/**  
  Selecting the appropriate entrypoint type per platform.

- **camouflage/**  
  Ensuring TLS/QUIC fingerprints match platform expectations.

- **fallback/**  
  Referencing region‑specific fallback strategies.

- **session-init/**  
  Providing negotiation parameters for each OS.

Client Profiles form the user‑facing configuration layer of the VPN Access Layer.

---

## Summary

The Client Profiles subsystem provides platform‑specific templates that ensure reliable, censorship‑resistant access across mobile, desktop, and embedded environments.  
By tuning protocol preferences, fallback behavior, and camouflage settings for each OS—including macOS—it enables consistent and secure connectivity under hostile network conditions.
