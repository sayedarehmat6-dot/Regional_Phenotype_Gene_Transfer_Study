# Reproducibility

## Purpose

This document explains how the computational analysis was run and what information should be retained for a reproducible research record.

## Primary execution environment

The final analysis was designed to be run in Google Colab.

The main research record is:

```text
Regional_Phenotype_Gene_Transfer_Study.ipynb
```

The notebook should be saved after all cells have executed successfully.

## Input files

The analysis expects:

```text
PAVS_cases.zip
hp-base.obo
```

The PAVS ZIP should contain the TSV used for the analysis.

Do not commit restricted patient-level input data to a public repository unless redistribution is explicitly permitted.

## Fixed parameters

```text
seed = 42
cross-validation folds = 5
minimum Saudi cases per gene = 5
bootstrap repetitions = 20,000
paired permutation repetitions = 20,000
BMA label permutations per fold = 5,000
DDD size-matching repeats = 20
```

## Running the notebook

Run the notebook from top to bottom.

The notebook:

1. locates the input files;
2. loads and audits the PAVS dataset;
3. parses HPO annotations;
4. creates the eligible Saudi and DDD cohorts;
5. constructs the fixed five-fold split;
6. runs the TF-IDF phenotype–gene analysis;
7. calculates primary metrics;
8. performs paired inference;
9. performs sensitivity analyses;
10. runs the ontology-aware BMA/Resnik analysis;
11. runs the within-Saudi negative control;
12. writes output tables and figures;
13. records a run manifest.

## Output files

The primary output directory contains:

```text
aggregate_metrics.csv
case_level_results.csv
joint_rankable_metrics.csv
paired_bootstrap_controls.csv
paired_direction_and_permutation.csv
sparse_hpo_metrics.csv
ddd_size_match_sensitivity.csv
run_manifest.json
```

The ontology-aware subdirectory contains:

```text
bma_case_level_results.csv
bma_metrics.csv
bma_within_saudi_label_permutation_null.csv
bma_within_saudi_label_permutation_summary.csv
```

Figures are saved separately under:

```text
figures/
```

## Input integrity

The final run should record SHA-256 hashes for:

- the PAVS dataset file;
- the HPO ontology file.

These hashes are stored in:

```text
results/run_manifest.json
```

This allows a later researcher to determine whether a different input file was used.

## Expected cohort checks

The final analysis should produce:

```text
Saudi eligible cases = 511
retained Saudi genes = 57
jointly rankable cases = 86
```

If these values differ, stop and investigate before interpreting the results.

## Expected primary checks

The final analysis should approximately reproduce:

```text
Saudi regional Recall@1  = 53.03%
Saudi regional Recall@5  = 74.76%
Saudi regional Recall@10 = 83.56%
Saudi regional Recall@20 = 91.59%

Joint Saudi Recall@5     = 67.44%
Joint DDD Recall@5       = 27.91%

Paired Recall@5 difference = +39.53 percentage points
95% bootstrap CI            = +26.74 to +51.16 percentage points
paired permutation p        ≈ 0.00005

Saudi BMA/Resnik Recall@5  = 70.45%
BMA negative-control mean   ≈ 9.91%
```

The displayed p-value can vary only within the deterministic calculation specified by the fixed seed and permutation count.

## Research record

For a public GitHub repository, keep:

- the executed notebook;
- the final analysis source;
- methods;
- limitations;
- reproducibility documentation;
- generated result tables;
- generated figures.

Do not retain obsolete exploratory versions in the public repository.

## Recommended GitHub rule

The repository should represent one frozen scientific analysis.

Older scripts, abandoned experiments, temporary notebooks, debugging files, and alternative result versions should remain outside the public repository unless they are specifically needed to document scientific provenance.
