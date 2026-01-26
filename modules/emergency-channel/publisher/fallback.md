# Publisher Module — Fallback & Resilience

## Overview

The Publisher module includes a robust fallback system designed to ensure that content remains deliverable even under severe network degradation, censorship, or partial infrastructure failure.  
Fallback mechanisms operate automatically and transparently, without requiring upstream modules to change their behavior.

The fallback layer is designed to be modular, allowing deployments to customize or extend fallback strategies based on operational needs.

---

## Fallback Triggers

Fallback is activated when any of the following conditions occur:

- **Primary channel failure**  
  The main publishing channel returns an error or becomes unreachable.

- **High latency or timeouts**  
  Delivery exceeds configured thresholds.

- **Censorship indicators**  
  Repeated connection resets, TLS interference, or content filtering patterns.

- **Network instability**  
  Packet loss, intermittent connectivity, or degraded bandwidth.

- **Channel health check failures**  
  Scheduled probes indicate that a channel is unhealthy.

Fallback triggers are logged for later analysis but do not interrupt the publishing pipeline.
