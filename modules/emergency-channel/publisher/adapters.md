# Publisher Module — Delivery Adapters

## Overview

Delivery adapters are the execution layer of the Publisher module.  
While channels define *where* content should be delivered, adapters define *how* the delivery actually happens.  
Each adapter implements a consistent interface, allowing the Publisher pipeline to send content through diverse mechanisms without knowing the underlying protocol.

Adapters are modular, isolated, and replaceable.  
Deployments may enable, disable, or override adapters based on operational needs.

---

## Adapter Responsibilities

Each delivery adapter is responsible for:

- **Payload transmission**  
  Sending formatted content to an external endpoint.

- **Protocol handling**  
  Managing HTTP, email, IPFS, P2P, or other transport mechanisms.

- **Error reporting**  
  Returning structured errors to the fallback subsystem.

- **Health signaling**  
  Providing status information for channel health checks.

- **Security enforcement**  
  Ensuring TLS, signing, or encryption requirements are met.

Adapters do *not* modify content; they only deliver it.
