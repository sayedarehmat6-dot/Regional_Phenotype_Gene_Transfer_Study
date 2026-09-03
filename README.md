# Regional Phenotype–Gene Transfer Study

## Overview

This repository contains an independent computational bioinformatics study of phenotype–gene prioritization in a Saudi rare-disease cohort.

The main question was:

> Does phenotype–gene information learned from Saudi rare-disease cases generalize better to held-out Saudi cases than a size-matched DDD reference?

The study was designed as a case-held-out computational analysis rather than a clinical validation study. The goal was to test whether there is a measurable phenotype–gene signal in the Saudi cohort and to examine how robust that signal is to simple controls and sensitivity analyses.

## Data

The analysis used a supplied case-level PAVS dataset containing:

- 7,510 records
- 31 columns
- 5,132 PAVS-Saudi records
- 1,856 DDD records
- 522 PAVS-mixed records

The phenotype annotations were represented using Human Phenotype Ontology (HPO) identifiers.

The HPO ontology file used for the ontology-aware analysis was the supplied `hp-base.obo` release. 

Patient-level data are not redistributed in this repository.

## Analysis

The main analysis used:

1. HPO parsing and cohort filtering.
2. Five-fold case-held-out cross-validation stratified by causal gene.
3. A transparent TF-IDF/cosine phenotype-to-gene model.
4. A DDD phenotype model trained with the same number of training cases as the corresponding Saudi fold.
5. Saudi and DDD gene-frequency controls.
6. Paired bootstrap confidence intervals.
7. A paired condition-label permutation test.
8. A sparse-phenotype sensitivity analysis.
9. Repeated DDD training-size matching.
10. An ontology-aware BMA/Resnik sensitivity analysis.
11. A within-Saudi causal-gene-label permutation negative control.

## Main result

On the jointly rankable subset of 86 cases:

- Saudi phenotype model Recall@5: **67.44%**
- DDD phenotype model Recall@5: **27.91%**
- paired difference: **+39.53 percentage points**
- 95% bootstrap CI: **+26.74 to +51.16 percentage points**

The paired condition-label permutation test for Recall@5 gave an empirical p-value of approximately **0.00005** using 20,000 permutations.

The ontology-aware BMA/Resnik sensitivity analysis achieved Recall@5 of **70.45%** on the full held-out Saudi cohort.

The within-Saudi BMA label-permutation negative control had a mean Recall@5 of approximately **9.91%**, compared with the observed **70.45%**.

## Interpretation

The results provide exploratory evidence of a regional phenotype–gene transfer signal in the analyzed Saudi cohort.

The result should not be interpreted as proof that the Saudi cohort has universally better biological knowledge, nor as evidence of clinical utility or superiority over clinical interpretation software.

The study evaluates held-out cases for genes that are represented in the training cohort. It is therefore not a de-novo unseen-gene discovery benchmark.

## Repository structure

```text
Regional_Phenotype_Gene_Transfer_Study/
├── README.md
├── METHODS.md
├── LIMITATIONS.md
├── REPRODUCIBILITY.md
├── Regional_Phenotype_Gene_Transfer_Study.ipynb
│
│
├── results/
│   ├── aggregate_metrics.csv
│   ├── case_level_results.csv
│   ├── joint_rankable_metrics.csv
│   ├── paired_bootstrap_controls.csv
│   ├── paired_direction_and_permutation.csv
│   ├── sparse_hpo_metrics.csv
│   ├── ddd_size_match_sensitivity.csv
│   └── bma_ontology_aware/
│
└── figures/
```

## How to use the repository

The primary analysis record is the executed Google Colab notebook.

Run the notebook from top to bottom using the permitted input files. The notebook shows the data checks, cohort construction, model fitting, evaluation, statistical tests, sensitivity analyses, and figures in sequence.


## Research status

This is a research and methods project intended for scientific review and discussion. It is not a clinical diagnostic tool.
