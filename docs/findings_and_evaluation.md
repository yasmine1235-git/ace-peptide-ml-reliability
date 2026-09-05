# Evaluation Findings

## Executive Summary

This project evaluated quantitative prediction of ACE-inhibitory peptide potency (`log10(IC50)`) across two literature-derived datasets and several sequence representations.

Two findings are most important. First, on the duplicate-free Wang et al. (2020) dataset, the strongest verified regression signal was concentrated in dipeptides (R² = 0.267 ± 0.214), while length ≥3 showed no stable positive regression signal across the tested pipelines. Second, in AHTPDB, apparently positive signal at lengths 3–7 under ordinary row-level cross-validation disappeared under sequence-grouped evaluation, demonstrating that duplicate-sequence leakage can substantially inflate peptide bioactivity prediction performance.

The mechanism behind the remaining length-dependent pattern is unresolved.

---

## 1. Evaluation Setup

Two primary datasets were evaluated:

- **Wang et al. (2020):** 728 unique peptides, lengths 2–16, without exact duplicate sequences.
- **AHTPDB:** 3,364 raw records, reduced to 2,610 usable records after cleaning and unit harmonization, representing 964 unique sequences.

The target was quantitative `log10(IC50)`.

Representations included:

- aggregate physicochemical descriptors
- positional one-hot encoding
- frozen ProtBERT embeddings
- frozen ESM-2 embeddings

Models included Ridge, Lasso, ElasticNet, Random Forest, and Gradient Boosting. Validation used repeated cross-validation, nested CV where applicable, and a held-out test set for the dedicated dipeptide analysis.

For AHTPDB, naive row-level CV was explicitly compared with sequence-grouped CV to prevent identical sequences from appearing in both training and test folds.

---

## 2. Main Results

### 2.1 Length-Stratified Regression

Repeated CV using physicochemical descriptors + Ridge on Wang et al.:

| Subset | n | R² (mean ± SD) |
|---|---:|---:|
| All | 728 | 0.091 ± 0.056 |
| Length 2 | 133 | **0.267 ± 0.214** |
| Length 3 | 215 | -0.034 ± 0.111 |
| Length 4 | 90 | -0.074 ± 0.166 |
| Length 5 | 132 | -0.030 ± 0.155 |
| Length 6 | 114 | -0.070 ± 0.189 |
| Excluding length 2 | 595 | -0.004 ± 0.048 |

The defensible conclusion is not that peptide length itself causally determines predictability. Rather, within this dataset and the tested regression pipeline, positive signal is concentrated in dipeptides, while length ≥3 shows no stable positive regression signal.

### 2.2 Dipeptide Robustness

For the 133 dipeptides, Ridge, Lasso, ElasticNet, Random Forest, and Gradient Boosting produced overlapping CV performance (~0.18–0.31 R²); no algorithm was clearly superior.

Nested CV:

- Tuned Ridge: **0.269 ± 0.193**
- Tuned Random Forest: **0.227 ± 0.227**

Held-out test set:

- R² = **0.232**
- RMSE = **1.15**
- Spearman ρ = **0.53**
- Dummy baseline R² = **-0.014**

Stability selection over 200 bootstrap Lasso fits selected molecular weight in ~100% of resamples and instability index in ~85.5%; the remaining descriptors did not reach the conventional 80% threshold.

**Caveat:** two probable censored/detection-limit values (`EA` and `EG`) occur in this subset and were not handled using censored regression.

### 2.3 Representation Triangulation

The length ≥3 regression failure was observed across four representation families:

1. physicochemical descriptors
2. positional one-hot encoding
3. frozen ProtBERT embeddings
4. frozen ESM-2 embeddings

For tripeptides:

- physicochemical descriptors: R² = -0.034 ± 0.111
- positional one-hot: R² = 0.052 ± 0.161

The uncertainty overlaps, so this does not establish a meaningful representation difference.

Frozen PLM results:

| Model | L2 | L3 | L4 | L5 | L6 |
|---|---:|---:|---:|---:|---:|
| ProtBERT | -0.176 ± 0.219 | -0.047 ± 0.169 | -0.630 ± 0.609 | -0.259 ± 0.305 | -0.121 ± 0.255 |
| ESM-2 | 0.287 ± 0.217 | -0.089 ± 0.183 | -0.300 ± 0.560 | -0.151 ± 0.294 | -0.361 ± 0.320 |

These results apply only to the specific frozen embedding + pooling + PCA + linear regression pipelines tested.

They do **not** establish that:

- PLMs generally underperform physicochemical descriptors;
- PLMs generally fail for short peptides; or
- embedding collapse explains the observed pattern.

The cosine-similarity analysis did not establish the latter mechanism.

---

## 3. Leakage and Evaluation Validity

AHTPDB contains repeated measurements of identical peptide sequences. Under ordinary row-level CV, different records for the same sequence can appear in training and test folds.

| Length | Naive R² | Sequence-Grouped R² |
|---|---:|---:|
| Pooled | 0.064 ± 0.021 | 0.033 ± 0.055 |
| 2 | 0.341 ± 0.093 | 0.259 ± 0.149 |
| 3 | 0.100 ± 0.062 | **-0.070 ± 0.149** |
| 4 | 0.030 ± 0.100 | **-0.181 ± 0.171** |
| 5 | 0.086 ± 0.094 | **-0.022 ± 0.101** |
| 6 | 0.143 ± 0.109 | **-0.025 ± 0.108** |
| 7 | 0.151 ± 0.172 | **-0.066 ± 0.207** |

The apparent positive signal at lengths 3–7 disappears when sequences are kept within the same fold.

**This is the strongest methodological finding of the project:** duplicate-sequence leakage can substantially inflate apparent peptide bioactivity prediction performance. Sequence-level grouping is therefore important when compiled peptide datasets contain repeated measurements of the same sequence.

---

## 4. Measurement Heterogeneity

Among 964 AHTPDB sequences, 551 had at least two measurements. After excluding obvious >100× discrepancies attributable to data-entry or unit artifacts, 517 sequences remained.

Key observations:

- Median repeated-measurement disagreement: **0 log units**
- ~18% of retained pairs disagreed by >2×
- Length 2 had the highest mean disagreement (~0.50 log units, ~3×)
- Length 3–6 showed smaller mean disagreement (~0.06–0.19 log units)
- Disagreement represented ~59% of model RMSE for length 2 but only ~7–20% for lengths 3–6

This is descriptive evidence, not a formal causal test. It does not prove that measurement noise is irrelevant. However, the observed disagreement does not align with the lengths showing the strongest regression failure, suggesting that measurement disagreement alone is unlikely to explain the length ≥3 result.

Censored measurements were discarded rather than modeled, so this analysis is not a complete estimate of the underlying measurement-noise ceiling.

---

## 5. Learning-Curve Correction

An early learning-curve analysis compared dipeptides and tripeptides at equal raw training sample sizes and concluded that sample size did not explain their performance difference.

This interpretation was later rejected as an invalid comparison.

The two sequence spaces differ substantially:

- Dipeptides: `20² = 400` possible sequences
- Tripeptides: `20³ = 8,000` possible sequences

Therefore, equal raw sample sizes correspond to radically different sequence-space coverage. The appropriate comparison is coverage-aware rather than simply matching the number of training examples.

The coverage-matched interpretation remains suggestive but has not been re-verified with the strongest repeat protocol. The original claim that "raw sample size does not explain the gap" is therefore **not retained**.

This correction is included as part of the project's methodological self-audit.

---

## 6. External Dataset Check

A separate 150-tripeptide dataset from Jiao et al. (2025) produced:

**R² = 0.035 ± 0.189**

The result remained near zero despite the dataset appearing more consistent in provenance.

However, 83/150 sequences overlapped with AHTPDB, with close agreement for most shared values. It should therefore not be treated as fully independent validation.

An apparent train/test result of R² = 0.228 was compared with 500 random splits of the same sizes and fell around the 93rd percentile, indicating that the particular split was favorable rather than strong evidence of generalizable predictability.

This dataset therefore provides supporting evidence for the tripeptide regression result, but not a fully independent replication.

---

## 7. What the Evidence Supports

The current evidence supports the following conclusions:

- Dipeptides show **modest, reproducible regression signal** in the Wang et al. dataset.
- No tested algorithm clearly outperformed the others on the dipeptide subset.
- Length ≥3 shows **no stable positive regression signal across the tested representation pipelines**.
- The same broad pattern persists across physicochemical, positional, ProtBERT, and ESM-2 representations under the specific pipelines evaluated.
- In AHTPDB, apparent positive signal at lengths 3–7 under naive row-level CV disappears under sequence-grouped evaluation.
- Duplicate-sequence leakage can therefore materially inflate peptide bioactivity prediction metrics.
- The observed measurement disagreement does not descriptively track the lengths with the strongest regression failure.
- The coverage-vs-noise interpretation remains an open question rather than a resolved result.

---

## 8. What the Project Does Not Establish

This project does **not** establish that:

- peptide length itself causally determines predictability;
- PLMs generally underperform simpler descriptors;
- measurement noise is proven to be the cause, or proven not to be the cause, of regression failure;
- sequence-space undersampling has been proven to explain the length-dependent pattern;
- any specific ML algorithm is universally superior;
- the findings generalize to all peptide bioactivity datasets;
- a new biological mechanism has been discovered.

The negative results should therefore be interpreted as **evaluation- and dataset-specific**, not as universal statements about peptide bioactivity prediction.

---

## 9. Remaining Question

The central unresolved question is **why** the positive regression signal observed for dipeptides does not persist for longer peptides in these datasets.

Two plausible explanations remain particularly relevant:

1. **Sequence-space coverage:** tripeptides occupy a much larger combinatorial space than dipeptides, so the available observations may provide substantially lower coverage.
2. **Measurement and dataset heterogeneity:** differences in assay conditions, provenance, censoring, and measurement quality may limit learnability.

The current analyses provide evidence against several simpler explanations and expose a major evaluation artifact, but they do not cleanly disentangle these mechanisms.

The appropriate conclusion is therefore not that the mechanism has been solved, but that **coverage, measurement heterogeneity, and representation/model limitations remain competing explanations requiring further controlled evaluation.**