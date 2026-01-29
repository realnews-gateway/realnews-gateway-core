# System Overview

The Emergency Publishing System is a censorship‑resistant communication architecture designed to deliver critical information under hostile network conditions.  
It is built around a single core subsystem — the **Emergency Channel** — supported by multiple ingestion, access, and content‑generation modules.

This document provides a high‑level overview of the system’s structure, major components, and operational model.

---

## Core System: Emergency Channel

The **Emergency Channel** is the backbone of the entire architecture.  
It is responsible for:

- Multi‑protocol message delivery  
- Multi‑hop routing  
- Region‑aware distribution  
- Sanitization and metadata minimization  
- Secure storage of published content  
- Fallback transports for high‑risk environments  

All content — whether aggregated news, pseudonymous posts, or emergency alerts — ultimately flows into and through the Emergency Channel.

The Emergency Channel defines the system’s runtime behavior and ensures resilience under censorship.

---

## Supporting Modules

Supporting subsystems are located under `modules/` and operate independently while integrating seamlessly with the core system.

### **1. vpn-access-layer/**
Provides covert access to the system using VPN‑like transports.  
Includes protocol obfuscation, fallback routing, and region‑aware entry points.

### **2. news-aggregation/**
Ingests external news sources, normalizes content, removes duplicates, and classifies topics.  
Feeds curated content into the Emergency Channel for distribution.

### **3. anonymous-bbs/**
Provides pseudonymous posting with account‑based access control.  
Users authenticate with lightweight accounts, but all public identity is system‑generated (name + avatar).  
Posts are sanitized and forwarded into the Emergency Channel.

---

## High-Level Architecture

The system is composed of four layers:

### **1. Access Layer**
- vpn-access-layer/  
- Provides covert entry points and fallback transports  
- Ensures users can reach the system even under active blocking

### **2. Ingestion Layer**
- news-aggregation/  
- anonymous-bbs/  
- Converts external sources and user posts into normalized internal messages

### **3. Core Processing Layer**
- emergency-channel/  
- Routing, sanitization, storage, multi-hop delivery  
- Applies censorship‑resistant strategies and metadata minimization

### **4. Distribution Layer**
- Part of emergency-channel/  
- Region‑aware delivery  
- Multi‑protocol fallback  
- Offline and low‑bandwidth formats

This layered design ensures modularity, extensibility, and operational resilience.

---

## Data Flow Summary

Content enters the system through one of two ingestion paths:

### **News Aggregation Path**
Source registry → Fetcher → Parser → Deduplication → Classification  
→ Emergency Channel → Distribution

### **Pseudonymous BBS Path**
Account login → Pseudonym generation → Sanitization → Storage  
→ Emergency Channel → Distribution

Both paths converge in the Emergency Channel, which handles all routing and delivery.

---

## Design Principles

The system is built on the following principles:

- **Resilience under censorship**  
  Multi‑protocol transports, fallback chains, and region‑aware routing.

- **Separation of concerns**  
  Access, ingestion, core processing, and distribution are clearly separated.

- **Modularity and extensibility**  
  Supporting modules evolve independently without breaking the core system.

- **Stealth and indistinguishability**  
  Traffic patterns mimic legitimate usage to evade detection.

- **Security by design**  
  Metadata minimization, encryption, and trust‑boundary isolation.

- **Operational simplicity**  
  Deployable in constrained environments with minimal configuration.

---

## Summary

The Emergency Publishing System is centered around the **Emergency Channel**, which provides secure, censorship‑resistant delivery of critical information.  
Supporting modules — vpn-access-layer, news-aggregation, and anonymous-bbs — feed content into the core system, enabling a unified, resilient communication ecosystem.

This overview establishes the conceptual foundation for understanding the system’s architecture and runtime behavior.
