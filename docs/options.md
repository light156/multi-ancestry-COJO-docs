---
title: Command Options
nav_order: 4
---

# Supported Command-line Options

## Input/Output File Options

| Option      | Description                                                   |
|:-------------|:------------------|
| `--bfile`   | PLINK binary file prefix for each cohort `.bim .bed .fam`    |
| `--ld`      | PLINK LD file prefix for each cohort `.bim .ld (.frq)`       |
| `--cojo-file`  | GWAS summary statistics file for each cohort in GCTA-COJO format<br>(Header `SNP A1 A2 freq b se p N`)    | 
| `--out`        | Output file path prefix                                    |


## Original GCTA Options

These options and flags are functionally identical to those in the [original GCTA COJO](https://yanglab.westlake.edu.cn/software/gcta/#COJO).

| Program mode    | Type     | Description (exactly one of these three is required)          |
|:-------------|:------------------|:------|
| `--cojo-slct`   | *flag*   | Stepwise iterative selection of independently associated SNPs |
| `--cojo-joint`  | *flag*   | Calculate joint effects for provided SNPs and exit            |
| `--cojo-cond`   | *option* | Calculate conditional effects for provided SNPs and exit<br> Must provide a file with a list of SNPs |

| Option         | Description                                                    | Default          |
|:-------------|:------------------|:------|
| `--cojo-wind`      | SNP position window in Kb (`-1` disables windowing)            | `10000` (±1e7)   |
| `--cojo-p`         | Significance threshold for SNP selection (`0-1`)               | `5e-8`           |
| `--cojo-collinear` | Colinearity threshold for SNP inclusion (`0–0.9999`)           | `0.9`            |
| `--cojo-top-SNPs`  | Only select a fixed number of independently associated SNPs    | `10000`          |
| `--extract`        | File path of SNPs to be included                               |                  |
| `--exclude`        | File path of SNPs to be excluded                               |                  |
| `--extract-snp`    | A list of SNPs to be included                                  |                  |
| `--exclude-snp`    | A list of SNPs to be excluded                                  |                  |
| `--chr`            | Only include SNPs on a specific chromosome (`1-22`)            |                  |
| `--thread-num`     | Number of threads to use     | `1`           |

Below options are extended to multiple cohorts (please see [Advanced Usages](https://light156.github.io/multi-ancestry-COJO-docs/tutorial_usage/) for example).

| Option         | Description                                                    | Default          |
|:-------------|:------------------|:------|
| `--keep`           | File path of individuals to be included                        |                  |
| `--remove`         | File path of individuals to be excluded                        |                  |
| `--diff-freq`      | Frequency diff threshold between sumstat and genotype (`0-1`)  | `0.2`            |
| `--maf`            | Minor allele frequency threshold for genotype (`0-0.5`)        | `0`              |
| `--maf-sumstat`    | Minor allele frequency threshold for sumstat files (`0-0.5`)   | `0`              |
| `--geno`           | Missingness threshold in .bed files (`0-1`)                    | `1` (none)       |


## Manc-COJO Specific Options/Flags

| Option         | Description                                                      | Default       |
|:-------------|:------------------|
| `--fix`        | File path for fixed SNPs for iterative selection             |               |
| `--fix-snp`    | A list of fixed SNPs for iterative selection                 |               |
| `--fix-p`         | Significance threshold for fixed SNPs (`0-1`)               | `5e-8`           |
| `--fix-col` | Collinearity threshold for fixed SNPs (`0–0.9999`)           | `0.9`            |
| `--fix-cojo-col` | Collinearity threshold between fixed SNPs and selected SNPs (`0–0.9999`)           | `0.9`            |
| `--R2`         | R² incremental threshold for forward selection               | `-1` (none)   |
| `--R2back`     | R² incremental threshold for backward selection              | `-1` (none)   |

| Flag         | Description       |
|:-------------|:------------------|
| `--fix-drop`  | Drop fixed SNPs above `--fix-p` threshold before SNP selection |
| `--var-from-ld` | Estimate genotypic variance from LD reference instead of 2pq |
| `--hetero-report`  | Output Cochran's Q heterogeneity statistics (multi-cohort only) |
| `--output-all`    | Save all `.cma.cojo .jma.cojo .ldr.cojo .badsnps` files |


## Algorithm options

Manc-COJO implements three algorithms for both stepwise SNP selection (`--slct-mode`) and effect size estimation (`--effect-mode`). 
Available options include 

- `GCTA` (default, original GCTA behaviour)  
  Impute missing genotypes when computing pairwise LD, and additionally adjust for missingness in GWAS summary statistics
- `imputeNA`  
  Only impute missing genotypes when computing pairwise LD
- `removeNA`  
  Only exclude individuals with missing genotypes on a per–SNP-pair basis

Please refer to our paper for detailed methodological descriptions.

| Option | Allowed Values | Default | Description |
|:-------------|:------------------|:------|:------|
| `--slct-mode` | `GCTA`, `imputeNA`, `removeNA` | `GCTA` | Iterative SNP selection method |
| `--effect-mode` | `GCTA`, `imputeNA`, `removeNA` | `GCTA` | Effect size estimation method |
