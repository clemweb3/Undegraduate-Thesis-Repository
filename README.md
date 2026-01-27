# Regional Financial Vulnerability in the Philippines  
**Labor Force Survey–Based Preprocessing, Factor Analysis, and Index Construction**

## Repository Overview

This repository contains the full data processing and analytical pipeline developed for the undergraduate thesis:

**_Towards the Determination of Regional Financial Vulnerability in the Philippines Using Labor Force Indicators_**

The thesis was conducted *inside this repository*. All scripts, intermediate outputs, diagnostics, and analytical results are versioned through Git commits to ensure transparency, reproducibility, and methodological auditability.
Rather than serving as a generic data analysis project, this repository functions as a **methodological record** of how the approved thesis design was operationalized in practice, from raw data ingestion to the construction of the Regional Financial Vulnerability Index (RFVI).

---

## Research Context

The study uses publicly available microdata from the **Philippine Statistics Authority (PSA) Labor Force Survey (LFS)** to examine regional patterns of financial vulnerability. Financial vulnerability is treated as a **latent, multidimensional construct**, informed by the framework of **sensitivity, resilience, and exposure** (Voith and Mauser, 2024), and operationalized through factor analysis and index construction.
The repository reflects a **diagnostic-first philosophy** of data preprocessing. Instead of imposing uniform cleaning rules or fixed thresholds, preprocessing decisions are guided by dataset-specific diagnostics, metadata validation, and empirical missingness assessment.

---

## Methodological Scope Implemented in This Repository

The repository implements the complete pipeline described in **Chapter 3 (Methodology)** of the approved thesis proposal, covering:

- Metadata-driven preprocessing and survey decoding  
- Diagnostic assessment of data integrity and missingness  
- Factor formation and factor analysis  
- Construction of a regional-level composite index (RFVI)  
- Exploratory clustering and visualization for interpretive support  

No analytical steps beyond those approved in the proposal were introduced. Refinements focus on robustness rather than redesign.

---

## Preprocessing Philosophy

### Diagnostic-First Data Preparation

Preprocessing begins with structured diagnostics inspired by Maharana et al. (2022), recognizing that LFS microdata varies across survey waves and cannot be treated uniformly.

Key diagnostic layers include:

- **Metadata-driven checks**  
  Validation of variable presence, naming consistency, and coding alignment across survey waves.

- **Quantitative completeness reviews**  
  Identification of missingness patterns without presupposing exclusion or imputation thresholds.

- **Structural consistency checks**  
  Detection of duplicate records, implausible household structures, and relational inconsistencies.

No assumptions are made about preprocessing requirements prior to exploratory analysis.

### Missingness Assessment and Imputation

Rather than relying on raw missingness percentages, the study uses the **Fraction of Missing Information (FMI)** as a central diagnostic indicator (Chen et al., 2021). This allows imputation decisions to account for:

- The contribution of auxiliary variables  
- The plausibility of missing-at-random (MAR) assumptions  
- The impact of missingness on inferential reliability  

Imputation procedures are refined iteratively and documented through commit history.

---

## Factor Formation and Analysis

### Conceptual Grounding

Variables are initially mapped to three theoretically informed dimensions:

- **Sensitivity**: susceptibility to financial shocks  
- **Resilience**: capacity to absorb and adapt to stress  
- **Exposure**: structural and locational vulnerability  

These groupings serve as *starting points*, not fixed assumptions.

### Empirical Validation

Factor analysis is used to validate whether these dimensions emerge from the data. The pipeline includes:

- Bartlett’s Test of Sphericity and KMO diagnostics  
- Factor extraction via Principal Axis Factoring or Maximum Likelihood  
- Orthogonal or oblique rotations as appropriate  
- Evaluation of loadings, communalities, and model fit  

The approach balances theoretical guidance with empirical evidence, avoiding mechanically driven factor interpretation.

---

## Regional Financial Vulnerability Index (RFVI)

The RFVI is constructed using factor scores derived from validated dimensions:

1. Factor scores are computed for sensitivity, resilience, and exposure  
2. Resilience is inversely transformed to reflect its protective role  
3. Scores are aggregated at the regional level  
4. The RFVI is computed as the mean of normalized dimension scores  

This approach prioritizes transparency, interpretability, and comparability across regions.

### Clustering and Interpretation

K-means clustering is applied to regional factor scores as an **exploratory interpretive tool**, not as a replacement for the index. Clusters highlight structural vulnerability profiles and support comparative policy insights without imposing categorical labels.

---

## Project Phases and Execution

The repository documents the following completed and ongoing phases:

1. **Data Collection and Management**  
2. **Metadata Structuring and Survey Decoding**  
3. **Data Integrity and Structural Diagnostics**  
4. **Missingness Diagnostics and Imputation Refinement**  
5. **Factor Analysis and RFVI Construction**  
6. **Clustering, Visualization, and Interpretation**  
7. **Writing and Finalization (in progress)**  

Each phase is reflected in commit logs, scripts, and intermediate outputs to provide a traceable development history.

---

## Ethical Considerations

All data used are publicly available and anonymized LFS datasets from the PSA.  
No personally identifiable information is accessed or disclosed.

The study is conducted strictly for academic and policy-oriented purposes. Results are presented to support regional understanding and comparative analysis, not to rank, label, or stigmatize regions.

---

## Repository Purpose

This repository is intended to:

- Demonstrate methodological fidelity to an approved thesis design  
- Provide a reproducible and auditable analytical pipeline  
- Serve as a transparent record of preprocessing and analytical decisions  
- Support thesis evaluation, defense, and potential extension  

It should be read alongside the thesis manuscript, particularly Chapter 3 (Methodology) and Chapters 4–5 (Results and Discussion).
