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

 
