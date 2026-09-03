# Methods

## 1. Study question

The study evaluates whether phenotype–gene information learned from a Saudi rare-disease cohort transfers to held-out Saudi cases more effectively than a size-matched DDD reference cohort.

The analysis was designed around a simple question that can be tested using the supplied case-level genotype–phenotype representation:

> Does a phenotype profile learned from Saudi cases contain useful information for prioritizing the causal gene of another Saudi case?

## 2. Input data

The supplied PAVS dataset contained 7,510 records and 31 columns.

Source counts in the supplied file were:

- PAVS-Saudi: 5,132
- DDD: 1,856
- PAVS-mixed: 522

The analysis used the `PAVS-Saudi` and `DDD` subsets.

The HPO ontology file  was used for the ontology-aware BMA/Resnik sensitivity analysis.

## 3. HPO parsing

HPO annotations were stored as semicolon-separated entries of the form:

```text
HP:0001263|Global developmental delay
```

The parsing procedure:

1. split the annotation string on `;`;
2. take the token before the first `|`;
3. retain only identifiers matching `HP:\d{7}`;
4. remove duplicate identifiers within a case.

The number of valid HPO terms per case was then calculated.

## 4. Eligibility criteria

A case was eligible when:

- `solved_status` was `SOLVED`;
- `gene_symbol` was present and non-empty;
- at least two valid HPO terms were available.

For the Saudi cohort, genes with fewer than five Saudi cases were excluded.

The resulting analysis cohort contained:

- **511 Saudi cases**
- **57 retained causal genes**

The DDD reference contained **1,437 eligible cases** after the same basic case-level eligibility rules.

## 5. Cross-validation design

The Saudi cohort was divided into five folds using stratified cross-validation.

Stratification was performed by causal gene.

Parameters:

```text
number of folds = 5
shuffle = True
random seed = 42
```

Each Saudi test fold was held out completely during model fitting.

All fold-comparable analyses reused the same split assignments.

## 6. TF-IDF phenotype–gene model

The main model is a transparent phenotype-profile baseline.

For each training case, the HPO identifiers were converted into a text-like representation containing the HPO IDs.

A TF-IDF vectorizer was fitted using the training cases only.

For each gene, the TF-IDF vectors of its training cases were summed to form a gene phenotype profile.

Each gene profile was L2-normalized.

For a held-out case, its phenotype vector was compared with every available training-gene profile using cosine similarity.

Genes were ranked from highest to lowest similarity.

The causal gene rank was recorded for each test case.

## 7. DDD comparison

For each Saudi training fold, a DDD training set was randomly sampled without replacement to the exact size of the Saudi training fold.

The same TF-IDF model was then trained on the sampled DDD cases.

This gives:

```text
Saudi training fold
        vs
size-matched DDD training set
```

using the same algorithm and the same held-out Saudi test cases.

The size matching controls for one obvious explanation of an apparent advantage: having more training cases.

## 8. Gene-frequency controls

Two frequency-only controls were included.

### Saudi gene-frequency control

Genes were ranked only by their frequency in the Saudi training fold.

### DDD gene-frequency control

Genes were ranked only by their frequency in the DDD training sample.

These controls test whether phenotype-based performance can be explained simply by how common the causal genes are in the corresponding training data.

## 9. Evaluation metrics

For each condition, the following were calculated:

- Recall@1
- Recall@5
- Recall@10
- Recall@20
- mean reciprocal rank (MRR)
- median causal rank

A target gene was considered retrieved at Recall@K when its rank was ≤ K.

When a target gene was not present in a comparison model's training gene set, the target could not be ranked and was reported as non-rankable for that condition.

## 10. Jointly rankable analysis

For direct paired comparisons between conditions, cases were restricted to those where all comparison models could rank the target gene.

This produced a paired analysis set of:

**86 cases**

The same cases are used for both conditions within each paired comparison.

## 11. Bootstrap confidence intervals

Paired bootstrap resampling was used to estimate 95% confidence intervals for differences in Recall@K.

The bootstrap was performed at the case level with:

```text
20,000 bootstrap replicates
seed = 42
```

The reported effect is the mean within-case difference in hit status multiplied by 100, expressed in percentage points.

## 12. Paired permutation test

For the main Saudi-versus-DDD phenotype comparison, condition labels were randomly exchanged within each paired test case.

The null hypothesis is that the two paired conditions are exchangeable.

For Recall@5:

```text
20,000 permutations
seed = 42
```

The empirical p-value uses the standard +1 correction:

```text
(1 + number of null statistics >= observed statistic)
/
(1 + number of permutations)
```

## 13. Sparse-phenotype sensitivity

A sensitivity analysis was performed on Saudi held-out cases with at most five valid HPO terms.

This checks whether the main phenotype signal disappears when phenotypic descriptions are relatively sparse.

## 14. Repeated DDD training-size sensitivity

The DDD training sample was independently re-drawn 20 times.

For every repeat and fold, the DDD training set had the same number of cases as the Saudi training fold.

Recall@5 was calculated using the same all-case denominator convention as the primary analysis.

This was treated as a sensitivity analysis rather than a separate primary endpoint.

## 15. Ontology-aware BMA/Resnik sensitivity

An independent ontology-aware implementation was used as a sensitivity analysis.

The HPO ontology was parsed for:

- `is_a` parent relationships;
- alternative HPO identifiers.

For each fold, information content was estimated from the Saudi training cases only.

For each gene, the union of HPO terms observed in its training cases was used as the gene phenotype profile.

Resnik similarity was calculated as the maximum information content among shared ancestors.

The final BMA score was the mean of the two directed best-match averages:

```text
case → gene
gene → case
```

The causal gene was then ranked by BMA score.

No test-case information was used to calculate training-fold information content.

## 16. Within-Saudi BMA negative control

To test whether the BMA result depends on the phenotype-to-target relationship, a negative control was performed entirely within the Saudi cohort.

For each fold:

1. the BMA score matrix was calculated normally;
2. the held-out causal-gene labels were randomly permuted among test cases;
3. the fixed score matrix was evaluated against the permuted labels.

The experiment used:

```text
5,000 label permutations per fold
seed = 42
```

This preserves the score structure while destroying the original case-to-causal-gene assignment.


 

## 17. Statistical interpretation

The study emphasizes effect size, rank-based performance, paired confidence intervals, and permutation testing rather than relying only on a single p-value.

The results are interpreted as evidence for an observed phenotype–gene transfer signal within the analyzed data, not as proof of clinical effectiveness.

## 18. Scope of inference

The experimental design evaluates cases whose causal genes are represented in the training cohort.

Therefore the study tests:

```text
generalization to new cases for represented genes
```

rather than:

```text
discovery of a completely unseen causal gene
```

This distinction is central to interpretation.
