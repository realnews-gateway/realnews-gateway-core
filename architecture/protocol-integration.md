# Protocol Integration

RealNews Free Publish Core integrates six advanced anti‑censorship protocols into a unified access layer:

- **Hysteria2** — QUIC‑based, high‑performance, resistant to active probing  
- **Reality** — TLS 1.3 camouflage with minimal fingerprint  
- **VLESS** — Flexible, modern, widely supported  
- **Trojan** — Pure TLS camouflage  
- **XTLS** — Low‑fingerprint, high‑performance transport  
- **XHTTP** — HTTP/3‑based, indistinguishable from normal web traffic

These protocols are abstracted into a single user‑friendly interface. The system automatically selects the optimal protocol based on network conditions, censorship level, and user location.

Protocol modules are isolated for independent testing and future extensibility. Legacy protocols such as **vmess** are explicitly excluded due to fingerprinting risks and poor performance.
