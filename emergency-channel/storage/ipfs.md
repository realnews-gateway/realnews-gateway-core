# Emergency Channel — IPFS Storage

## 1. Purpose

IPFS (InterPlanetary File System) serves as the primary distributed storage backend for the Emergency Channel.  
It provides decentralized, content-addressed, and censorship-resistant storage, ensuring that published content remains accessible even during regional outages or targeted disruptions.  
IPFS complements Local Storage by offering long-term durability and global availability.

---

## 2. Responsibilities

IPFS Storage is responsible for:

### 2.1 Distributed Content Persistence

- Store content across multiple IPFS nodes  
- Ensure redundancy through replication  
- Provide long-term durability beyond local storage limits  

### 2.2 Content Addressing

- Generate immutable content identifiers (CIDs)  
- Guarantee that identical content maps to the same CID  
- Prevent tampering by using cryptographic hashing  

### 2.3 Global Availability

- Allow content retrieval from any IPFS gateway or peer  
- Support multi-region access with minimal latency  
- Enable fallback retrieval when local or cloud storage is unavailable  

### 2.4 Integration with the Pipeline

- Accept content from Local Storage for replication  
- Provide CIDs back to the Distributor and Analytics modules  
- Support verification and integrity checks during retrieval

---

## 3. Architecture

IPFS Storage integrates with the Emergency Channel through a lightweight, modular architecture designed for reliability and decentralization.

### 3.1 Node Interaction Model

The system interacts with IPFS nodes using:

- Local IPFS daemons (preferred for performance)  
- Remote IPFS gateways (fallback mode)  
- Clustered IPFS nodes for redundancy  
- Optional private IPFS networks for sensitive deployments  

This flexible model ensures compatibility across diverse environments.

### 3.2 Content Pinning

To guarantee persistence, the system uses pinning:

- Newly added content is pinned automatically  
- Multiple nodes may pin the same CID for redundancy  
- Pinning status is tracked in metadata  
- Unpinned content may be garbage-collected by IPFS  

Pinning ensures that critical content remains available.

### 3.3 Replication Strategy

Replication is performed asynchronously:

1. Content is added to a local IPFS node  
2. CID is generated and returned to the pipeline  
3. Background tasks replicate the CID to additional nodes  
4. Replication success is verified periodically  

This strategy avoids blocking the main publishing pipeline.

### 3.4 Gateway Fallback

If local IPFS nodes are unavailable:

- The system switches to public or private gateways  
- Gateway selection is based on latency and availability  
- Gateway failures trigger automatic retries  
- Local nodes are rechecked periodically for recovery  

Fallback ensures uninterrupted access to IPFS content.

---

## 4. Failure Handling

IPFS is decentralized, but individual nodes, gateways, or network paths may still fail.  
The system includes robust fallback and retry mechanisms to ensure reliability.

### 4.1 Node Failure

If a local IPFS node becomes unreachable:

- Switch to another local or clustered node  
- Retry operations with exponential backoff  
- Temporarily mark the node as degraded  
- Recheck node health periodically  

Node failures must not block the publishing pipeline.

### 4.2 Gateway Failure

If a gateway returns errors or times out:

- Switch to an alternative gateway  
- Prefer gateways with lower latency  
- Record failure metrics for analytics  
- Retry with increasing intervals  

Gateways are treated as interchangeable access points.

### 4.3 Replication Failure

If replication to additional nodes fails:

- Retain content in Local Storage  
- Retry replication in the background  
- Escalate to monitoring if failures persist  
- Avoid dropping or overwriting unreplicated content  

Replication failures must never result in data loss.

---

## 5. Security and Integrity

IPFS provides strong integrity guarantees through content addressing, but additional safeguards are implemented.

### 5.1 Hash Verification

- Verify CID matches the content before publishing  
- Reject mismatched or corrupted data  
- Recompute hashes when necessary  

### 5.2 Access Control

While IPFS is public by default, the system may use:

- Private IPFS networks  
- Encrypted content blobs  
- Access-controlled gateways  

These options support deployments requiring confidentiality.

### 5.3 Anti-Tampering

- Immutable CIDs prevent unauthorized modification  
- Metadata includes integrity checks  
- Retrieval paths validate content before returning it  

Integrity is enforced at every stage.

---

## 6. Summary

IPFS Storage provides decentralized, content-addressed, and censorship-resistant storage for the Emergency Channel.  
Through pinning, replication, fallback gateways, and strong integrity checks, it ensures long-term durability and global accessibility for published content.
