# Limitations

This study is intended as a computational research project and has several important limitations.

## 1. Case-level representation

The supplied PAVS file is a case-level genotype–phenotype representation. It is not a complete per-patient VCF, WES, or WGS candidate-variant space.

The experiment therefore evaluates phenotype–gene prioritization and transfer rather than full clinical variant interpretation.

## 2. Saudi and DDD cohort differences

The Saudi and DDD cohorts were collected under different ascertainment processes and have different disease compositions.

Matching the number of training cases does not remove all differences between the cohorts.

The Saudi-versus-DDD comparison should therefore be interpreted as evidence of an observed transfer difference in these datasets, not as a fully controlled causal estimate of a population effect.

## 3. Different gene coverage

The DDD model cannot rank a Saudi target gene if that gene is absent from its training cohort.

This is why jointly rankable cases are reported separately and why all-case and paired analyses are both retained.

The relatively small jointly rankable subset of 86 cases is an important limitation on direct paired inference.

## 4. Known-gene generalization

A gene must be represented in the training data for the phenotype-profile model to create a gene-level profile.

The study therefore does not establish de-novo discovery performance for genes that are completely absent from training.

## 5. Family structure

The supplied representation does not provide enough family metadata to guarantee a family-level split.

The five-fold split is stratified by causal gene rather than by family.

Related individuals therefore cannot be ruled out completely from appearing in different folds.

## 6. ROH and consanguinity

The supplied TSV representation does not contain the necessary genomic structure for a direct analysis of runs of homozygosity or family-level consanguinity.

No claim about ROH-based prioritization is made in this study.

## 7. HPO annotation differences

Phenotyping practices can differ between cohorts.

Differences in:

- number of HPO terms;
- choice of HPO terms;
- annotation depth;
- disease ascertainment;

may influence phenotype-based performance.

The sparse-phenotype analysis partly addresses annotation density but does not remove all annotation-related confounding.

## 8. Model simplicity

The TF-IDF/cosine model is deliberately transparent.

It is not intended to represent the current state of the art in phenotype–gene prioritization.

Likewise, the independent BMA/Resnik implementation is a sensitivity model, not a claim of superiority over established production tools.

## 9. Ontology-aware implementation

The BMA/Resnik sensitivity analysis is an independent implementation using the supplied HPO release.

It should not be described as a byte-for-byte reproduction of a published benchmark implementation.

## 10. No commercial benchmark

The study does not compare against commercial clinical interpretation systems.

It therefore cannot support claims of superiority over commercial clinical workflows.

## 11. No expert adjudication

The study has no blinded expert review or adjudication of whether the algorithmic top-ranked genes are clinically useful.

The causal gene labels in the source data are treated as the study's evaluation targets.

## 12. No clinical utility assessment

The study does not measure:

- diagnostic yield;
- time saved by clinicians;
- changes in patient management;
- clinical sensitivity/specificity;
- patient outcomes.

Those are separate questions requiring clinical and institutional validation.

## 13. External validity

The analysis is based on the supplied PAVS and DDD datasets.

The results may not transfer directly to:

- another hospital;
- another national cohort;
- another phenotype annotation system;
- another disease spectrum;
- another sequencing workflow.

Replication in independent datasets would strengthen the findings.

## 14. Interpretation of the main result

The most defensible conclusion is that the analyzed Saudi cohort contains a measurable phenotype–gene structure that supports case-held-out prioritization and that this signal is substantially stronger than the tested DDD reference under the specified analysis.

A stronger claim that the effect is uniquely caused by Saudi biology, ancestry, consanguinity, or population-specific mechanisms is not established by this study.
