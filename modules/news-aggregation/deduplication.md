# Deduplication and Content Clustering

The deduplication subsystem identifies duplicate or near-duplicate articles across heterogeneous news sources.  
Because many sources syndicate, mirror, or rewrite the same content, deduplication is essential for reducing noise, improving classification accuracy, and ensuring that downstream modules receive clean, non-redundant data.

This document describes the hashing, similarity detection, and clustering strategies used by the system.

---

## Objectives

The deduplication subsystem provides:

- Exact duplicate detection using hashing
- Near-duplicate detection using similarity metrics
- Clustering of related articles
- Canonical article selection
- Integration with classification and publisher modules

These capabilities ensure that the aggregation pipeline produces clean, non-redundant content.

---

## Deduplication Pipeline

The deduplication process consists of three stages:

### 1. Exact Duplicate Detection
    - Uses normalized article text
    - Computes stable content hashes
    - Detects identical or trivially modified content

### 2. Near-Duplicate Detection
    - Uses similarity metrics (Jaccard, cosine similarity)
    - Detects rewritten or partially modified articles
    - Handles syndicated content and mirrored sources

### 3. Clustering and Canonicalization
    - Groups related articles into clusters
    - Selects a canonical representative
    - Assigns cluster IDs for downstream processing

---

## Exact Duplicate Detection

Exact duplicates are detected using:

### Hashing Strategy
    - SHA-256 hash of normalized body text
    - Optional secondary hash for metadata
    - Hash collisions are extremely unlikely

### Normalization Steps
    - Lowercasing
    - Removing punctuation
    - Removing boilerplate
    - Normalizing whitespace

If two articles share the same hash, they are considered exact duplicates.

---

## Near-Duplicate Detection

Near-duplicate detection identifies articles that:

- Are rewritten versions of the same content  
- Are syndicated across multiple sources  
- Differ only in formatting or minor edits  

### Similarity Metrics

The system uses multiple metrics:

    - Jaccard similarity (token sets)
    - Cosine similarity (TF-IDF vectors)
    - MinHash signatures (LSH for scalability)
    - Sentence-level embeddings (optional)

### Thresholds

Typical thresholds:

    - Jaccard > 0.6 → likely duplicate
    - Cosine > 0.75 → likely duplicate
    - Embedding similarity > 0.85 → near-identical

Thresholds are region-aware and source-aware.

---

## Clustering

Articles are grouped into clusters using:

### 1. LSH Bucketing
    - Efficiently groups similar articles
    - Reduces pairwise comparisons

### 2. Hierarchical Clustering
    - Merges similar buckets
    - Produces stable cluster IDs

### 3. Canonical Article Selection
    - Selects the highest-quality article in each cluster
    - Criteria include:
        - Source reliability
        - Metadata completeness
        - Body length
        - Parsing quality

The canonical article is passed to classification and publishing modules.

---

## Handling Multi-Language Content

When multiple languages appear in the same cluster:

    - Articles are grouped by language
    - Each language group receives its own canonical article
    - Cross-language similarity is optional and disabled by default

This avoids false positives across translations.

---

## Performance Considerations

The deduplication subsystem is optimized for:

### Scalability
    - LSH reduces comparisons from O(n²) to O(n log n)
    - Hashing is constant-time

### Memory Efficiency
    - Uses compact MinHash signatures
    - Stores only normalized text for hashing

### Incremental Updates
    - New articles are compared only against recent clusters
    - Old clusters are archived

---

## Integration with Other Modules

The deduplication subsystem integrates with:

- **parser.md**  
  Receives normalized article text.

- **classification.md**  
  Receives canonical articles for topic classification.

- **publisher/**  
  Receives deduplicated content for distribution.

- **emergency-channel/**  
  Receives only canonical, verified articles.

- **sources.md**  
  Uses source reliability to prioritize canonical selection.

---

## Summary

The deduplication subsystem ensures that the news aggregation pipeline produces clean, non-redundant content.  
By combining hashing, similarity detection, and clustering, it identifies both exact and near-duplicate articles and selects canonical representatives for downstream processing.
