# Emergency Channel — Node Selection

## 1. Purpose

Node selection is the core mechanism that determines which processing or storage node will handle a given piece of content.  
The goal is to choose the most reliable, efficient, and available node based on real-time system metrics and routing policies.  
Node selection must be deterministic, fast, and resilient to partial system failures.

---

## 2. Selection Criteria

Node selection is based on a combination of metrics and policy rules.  
Each node is evaluated according to the following criteria:

### 2.1 Availability

- Node must be online and responding to health checks  
- Recent failure history must be below acceptable thresholds  
- Node must not be in a degraded or maintenance state  

### 2.2 Latency

- Measured round-trip latency from the ingress node  
- Historical latency trends over short and long windows  
- Latency penalties applied to unstable or fluctuating nodes  

### 2.3 Load

- Current queue depth  
- CPU and memory utilization  
- Active request count  
- Load penalties for nodes nearing capacity  

### 2.4 Storage or Capability Constraints

- Node must support the required content type  
- Node must have sufficient storage capacity  
- Node must support required processing features (e.g., sanitizer strict mode)
