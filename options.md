---
title: Command options
nav_order: 3
---

# Supported Command-line Options

### Input Data Format Options (exactly one required)

| Option      | Description                                                        |
|:-------------|:------------------|:------|
| `--bfile`   | PLINK binary file prefix for each cohort `.bim .bed .fam`    |
| `--ld`      | PLINK LD file prefix for each cohort `.bim .ld (.frq)`       |

### Original GCTA Options

These options and flags are functionally identical to those in [original GCTA](https://yanglab.westlake.edu.cn/software/gcta).

| Program mode    | Type     | Description (exactly one of these three is required)          |
|:-------------|:------------------|:------|
| `--cojo-slct`   | *flag*   | Stepwise iterative selection of independently associated SNPs |
| `--cojo-joint`  | *flag*   | Calculate joint effects for provided SNPs and exit            |
| `--cojo-cond`   | *option* | Calculate conditional effects for provided SNPs and exit<br> Must provide a file with a list of SNPs |

| Option         | Description                                                    | Default          |
|:-------------|:------------------|:------|
| `--cojo-file`      | GWAS summary statistics file for each cohort in GCTA-COJO format<br>(Header `SNP A1 A2 freq b se p N`)    | `Required`       |
| `--out`            | Output file path prefix                                        | `Required`       |
| `--cojo-wind`      | SNP position window in Kb (`-1` disables windowing)            | `10000` (±1e7)   |
| `--cojo-p`         | Significance threshold for SNP selection                       | `5e-8`           |
| `--cojo-collinear` | Colinearity threshold for SNP inclusion (`0–0.999`)            | `0.9`            |
| `--diff-freq`      | Frequency diff threshold between sumstat and PLINK (`0-1`)     | `0.2`            |
| `--maf`            | Minor allele frequency threshold (`1e-5-0.5`)                  | `0.01`           |
| `--extract`        | File path of SNPs to be included                               |                  |
| `--exclude`        | File path of SNPs to be excluded                               |                  |
| `--extract-snp`    | A list of SNPs to be included                                  |                  |
| `--exclude-snp`    | A list of SNPs to be excluded                                  |                  |
| `--keep`           | File path of individuals to be included                        |                  |
| `--remove`         | File path of individuals to be excluded                        |                  |
| `--chr`            | Only include SNPs on a specific chromosome (`1-22`)            |                  |
| `--thread-num`     | Number of threads to use     | `1`           |

### Multi-ancestry COJO Options/Flags

| Option         | Description                                                      | Default       |
|:-------------|:------------------|
| `--fix`        | File path for fixed SNPs (non-removable in selection)            |               |
| `--fix-snp`    | A list of fixed SNPs (non-removable in selection)                |               |
| `--R2`         | R² threshold for forward selection                               | `-1` (none)   |
| `--R2back`     | R² threshold for backward selection                              | `-1` (none)   |
| `--iter`       | Maximum number of iterations                                     | `10000`       |

| Flag         | Description                                                       |
|:-------------|:------------------|
| `--freq-mode-and`  | Only keep SNPs that reach MAF threshold in sumstat of all cohorts<br>By default, keep SNPs that reach threshold in at least one cohort |
| `--MDISA`    | Run single-ancestry analysis after multi-ancestry COJO<br> By default, only run COJO selection on multiple cohorts and exit |
| `--output-all`     | Save all .cma.cojo, .jma.cojo and .ldr.cojo results to file    |


### Algorithm options

| Option | Allowed Values | Default | Description |
|:-------------|:------------------|:------|:------|
| `--slct-mode` | `GCTA`, `removeNA`, `imputeNA` | `GCTA` | Iterative SNP selection method |
| `--effect-size-mode` | `GCTA`, `removeNA`, `imputeNA` | `GCTA` | Effect size estimation method |
