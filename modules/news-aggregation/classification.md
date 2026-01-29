# Classification and Source Scoring

The classification subsystem assigns topic labels, categories, and metadata to normalized articles.  
It also evaluates source reliability and content quality, producing scores that influence downstream publishing and emergency workflows.

This document describes the topic classification pipeline, scoring mechanisms, and integration with other modules.

---

## Objectives

The classification subsystem provides:

- Topic classification (politics, world, economy, tech, etc.)
- Multi-label tagging
- Language-aware classification
- Source reliability scoring
- Content quality scoring
- Cluster-level classification for deduplicated articles

These capabilities ensure that the aggregation pipeline produces structured, meaningful, and prioritized content.

---

## Classification Pipeline

The classification process consists of four stages:

### 1. Preprocessing
    - Receives canonical articles from deduplication
    - Normalizes text for classification
    - Removes boilerplate and noise
    - Detects language and applies language-specific models

### 2. Topic Classification
    - Assigns one or more topic labels
    - Uses statistical, rule-based, or ML-based classifiers
    - Supports multi-label classification

### 3. Tag Extraction
    - Extracts keywords from titles and body text
    - Uses TF-IDF, RAKE, or embedding-based keyword extraction
    - Normalizes tags across languages

### 4. Source and Content Scoring
    - Computes reliability and quality scores
    - Adjusts scores based on historical performance
    - Produces final ranking for downstream modules

---

## Topic Classification

The system supports a standard set of topics:

    - Politics
    - World
    - Economy
    - Technology
    - Society
    - Health
    - Environment
    - Culture
    - Science
    - Breaking News

Classification methods include:

### Rule-Based Classification
    - Keyword matching
    - Title heuristics
    - Source-specific patterns

### Statistical Models
    - TF-IDF + logistic regression
    - Naive Bayes classifiers

### Embedding-Based Models
    - Sentence embeddings
    - Cosine similarity to topic vectors

The system selects the appropriate method based on region and language.

---

## Tag Extraction

Tags provide fine-grained metadata:

### Extraction Methods
    - TF-IDF keyword extraction
    - RAKE (Rapid Automatic Keyword Extraction)
    - Embedding-based keyword scoring
    - Named entity recognition (NER)

### Tag Normalization
    - Lowercasing
    - Removing duplicates
    - Mapping synonyms to canonical tags
    - Language normalization

Tags are used by publisher and emergency-channel modules for content routing.

---

## Source Reliability Scoring

Source reliability is updated based on:

### Availability
    - Fetch success rate
    - Mirror performance
    - Response time

### Content Quality
    - Parsing success rate
    - Metadata completeness
    - Duplicate ratio

### Historical Stability
    - Frequency of outages
    - Consistency of update intervals

Scores range from 0.0 to 1.0 and influence:

    - Source prioritization
    - Canonical article selection
    - Emergency publishing decisions

---

## Content Quality Scoring

Each article receives a quality score based on:

    - Body length
    - Metadata completeness
    - Parsing confidence
    - Language detection confidence
    - Duplicate cluster size

High-quality articles are prioritized for publishing.

---

## Cluster-Level Classification

For deduplicated clusters:

    - The canonical article receives full classification
    - Cluster-level tags are aggregated
    - Topic labels are propagated to all cluster members
    - Source reliability influences cluster ranking

This ensures consistent classification across related articles.

---

## Integration with Other Modules

The classification subsystem integrates with:

- **parser.md**  
  Receives normalized article text and metadata.

- **deduplication.md**  
  Receives canonical articles for classification.

- **publisher/**  
  Uses topic labels and tags for routing and prioritization.

- **emergency-channel/**  
  Uses classification to identify urgent or high-impact content.

- **sources.md**  
  Updates source reliability scores.

---

## Summary

The classification subsystem assigns topics, extracts tags, and computes reliability and quality scores for aggregated news content.  
By combining rule-based, statistical, and embedding-based methods, it produces structured metadata that drives publishing, prioritization, and emergency workflows.
