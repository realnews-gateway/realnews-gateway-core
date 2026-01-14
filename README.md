# RealNews Gateway Core

RealNews Gateway is an open-source information freedom infrastructure designed to help users in highly censored regions securely access independent news, global information, and public-interest content.  
The project integrates a censorship-resistant network access layer, a resilient news aggregation system, a lightweight anonymous discussion board, and a secure uploader to strengthen media freedom and democratic resilience.

---

## Mission

Access to reliable information is essential for informed decision-making and civic participation.  
RealNews Gateway aims to reduce the impact of censorship by providing a secure, resilient, and user-friendly pathway to independent news and public-interest media—especially for non-technical users who currently lack safe and accessible tools.

---

## Why This Project Matters

### **1. Modern protocols exist, but ordinary users cannot use them**

State-of-the-art anti-censorship protocols such as **Hysteria2, Reality, VLESS, Trojan, XTLS, and XHTTP** offer strong obfuscation, high performance, and excellent resistance to active probing and traffic identification.  
However, despite their technical advantages, these protocols remain inaccessible to most ordinary users:

- Configuration is complex and inconsistent  
- Tools are fragmented across different ecosystems  
- No unified onboarding experience exists  
- Documentation is highly technical  
- Each protocol requires different deployment logic  
- No mainstream client integrates all modern protocols together  

As a result, most users rely on commercial proxy services (“airports”), which still deploy outdated protocols such as **Vmess** or basic TLS tunnels—protocols that are easier to detect, easier to block, and offer weaker security guarantees.

**RealNews Gateway closes this gap** by integrating **Hysteria2, Reality, VLESS, Trojan, XTLS, and XHTTP** into a single, coherent, beginner-friendly access layer.  
This ensures that the most secure and modern technologies become accessible to the people who need them most.

---

### **2. Airport subscription models are rigid and user‑unfriendly**

The current airport ecosystem relies heavily on:

- Monthly subscriptions  
- Fixed traffic packages  
- Hard limits and throttling  
- No adaptation to censorship intensity  
- No intelligent protocol switching  

These models are inflexible and often exploitative:

- Light users overpay  
- Heavy users are throttled or banned  
- Users cannot dynamically switch protocols based on network conditions  
- No system optimizes for censorship evasion  

**RealNews Gateway introduces a flexible strategy layer**, including:

- Automatic protocol selection across all modern protocols  
- Multi-path routing and fallback  
- On-demand usage instead of fixed subscriptions  
- Support for NGO‑ or community‑hosted nodes  
- Resilience against targeted blocking  

This frees users from the limitations of commercial subscription-based airports.

---

### **3. Airport services frequently disappear, leaving users with losses**

A major structural problem in the airport ecosystem is instability:

- Providers frequently shut down or “run away”  
- Users lose prepaid funds  
- No transparency or accountability  
- No recourse for non-technical users  

This disproportionately harms people in high-risk environments who rely on these services for essential access to information.

**RealNews Gateway provides a sustainable, transparent, and auditable alternative** that does not depend on any single commercial provider.  
By enabling NGOs, independent media, and community groups to host nodes, the project offers a trustworthy, long-term solution for users who urgently need reliable access to uncensored information.

---

## Core Components

### **1. VPN Access Layer**
A censorship-resistant network layer built on modern transport protocols:

- Hysteria2  
- Reality  
- VLESS  
- Trojan  
- XTLS  
- XHTTP  

Features include:

- Multi-path routing  
- Automatic fallback  
- Traffic obfuscation  
- High performance under active blocking  

---

### **2. News Aggregation Module**
A resilient system that:

- Aggregates RSS and open news feeds  
- Mirrors independent media sources  
- Provides a consistent, censorship-resistant reading experience  
- Supports offline and low-bandwidth environments  

---

### **3. Lightweight Anonymous BBS**
A minimal, privacy-preserving discussion board enabling:

- Anonymous posting  
- Basic moderation tools  
- Low-risk community interaction  
- Optional decentralized storage  

---

### **4. Secure Uploader**
A safe channel for transmitting text, images, and media to:

- External platforms  
- Journalists or NGOs  
- Decentralized storage networks (IPFS, Arweave)

With metadata protection and optional encryption.

---

## Project Status

RealNews Gateway is in its initial development phase.  
This repository contains the open-source core architecture, documentation, and module definitions.  
Code will be released progressively as components reach stability.

---

## Roadmap Highlights

- News Aggregation MVP  
- VPN Access Layer integration  
- Anonymous BBS prototype  
- Secure Uploader with decentralized storage support  

See `roadmap.md` for details.

---

## License

This project is licensed under the **GNU Affero General Public License v3.0 (AGPLv3)**.  
This ensures that improvements to the software—especially when deployed as a network service—remain open and benefit the public.

---

## Collaboration

RealNews Gateway welcomes collaboration with:

- Independent media organizations  
- Civil society groups  
- Researchers and technologists  
- Digital rights advocates  

Please open an issue or reach out through the organization page.
