---
title: Advanced Usages
parent: Usage & Tutorial
nav_order: 1
---

The usage of Manc-COJO is largely consistent with the original GCTA COJO, with extensions to support multiple cohorts and PLINK LD matrix inputs. This section summarizes the key extensions, behavioural differences, and common advanced usage patterns. 

## Extension 1: Multi-cohort support

Many options can now accept multiple values. Specifically,

- For `--bfile / --cojo-file`, make sure the file paths are provided in the same order across cohorts for meaningful results.
- For `--keep / --remove`, empty strings ("") can be used when an option is not applied to a given cohort.
- For `--diff-freq / --maf / --maf-sumstat / --geno`, either provide one value for all cohorts, or one value per cohort.

For example, for two cohorts, if you want to keep certain individuals in `list1.fam` for cohort 1 but remove individuals in `list2.fam` for cohort 2, exclude SNPs with frequency differences > 0.2 between genotype and sumstat file in cohort 1, and exclude all SNPs with MAF < 0.005 in genotypes for both cohorts, use the following command:

```bash
./manc_cojo \
--bfile path1 path2 \
--cojo-file GWAS_sumstat_path1 GWAS_sumstat_path2 \
--keep list1.fam "" \
--remove "" list2.fam \
--diff-freq 0.2 1 \
--maf 0.005 \
--cojo-slct 
```

## Extension 2: LD input formats

Manc-COJO supports two alternative LD input formats:
- Genotype data in PLINK binary format via `--bfile`  
  Requires `.bed`, `.bim`, and `.fam` files.
- Precomputed LD matrices in PLINK LD format via `--ld`  
  Requires `.bim` and `.ld` files. To filter SNPs with large allele-frequency discrepancies, `.frq` files should also be provided.

Just replace `--bfile` with `--ld`:

```bash
./manc_cojo \
--ld path \
--cojo-file GWAS_sumstat_path \
--out Output_path_name \
--cojo-slct
```

## Extension 3: Allow users to fix SNPs during selection

While bioinformatics analyses often serve as an upstream step for wet-lab validation, our software enables a “two-way street”: 
causal variants confirmed experimentally can be incorporated into the model—even if they are not the most statistically significant (due
to sampling variability) or do not meet genome-wide significance thresholds (due to limited
sample sizes)—and will not be removed during subsequent variable selection. 
Incorporating wet-lab–validated variants directly into the modelling framework may further enhance the
detection of independent association signals.

This functionality is enabled via the options `--fix` and `--fix-snp`:

- `--fix`: provide a file containing a list of SNP IDs (one per line)
- `--fix-snp`: provide SNP IDs directly on the command line

For example, for three cohorts, if you want to exclude two SNPs and fix three SNPs for iterative selection, and use 5 threads to speed up computation, use:

```bash
./manc_cojo \
--bfile path1 path2 path3 \
--cojo-file GWAS_sumstat_path1 GWAS_sumstat_path2 GWAS_sumstat_path3 \
--cojo-slct \
--exclude-snp rs10001 rs10002 \
--fix-snp rs10003 rs10004 rs10005 \
--thread-num 5
```

## Extension 4: User-friendly SNP file operations

1. Both combined-chromosome bfiles (chromosomes 1–22 together) and per-chromosome bfiles are supported. Only chromosomes 1–22 are currently supported.

2. Options `--extract / --exclude / --fix / --cojo-cond` require a file containing a list of SNPs.

In practice, users often have an existing file in which SNP IDs appear in a specific column rather than a dedicated SNP list file. 
In GCTA, these SNPs must be manually extracted into a separate file, which can be inconvenient.

In contrast, our software allows users to directly specify the column index (starting from 1) containing SNP IDs, as well as whether the input file includes a header line. By default, the first column is read and no header is assumed. Taking `--extract` as an example, the following inputs are all valid:

- `--extract SNP_file`  
  Read the first column, no header
- `--extract SNP_file header`  
  Read the first column, skip the header
- `--extract SNP_file 2`  
  Read the second column, no header
- `--extract SNP_file 2 header`  
  Read the second column, skip the header

You can find an example usage in the [Tutorial](https://light156.github.io/multi-ancestry-COJO-docs/tutorial/#step-3-run-conditional-or-joint-analysis) section.


## Differences from GCTA-COJO 

Functional differences:
1. GCTA does not guard against numeric underflow, with p-values smaller than $1.7 \times 10^{-308}$ stored as zero, which may occasionally lead to suboptimal SNPs being selected and influence subsequent steps. In comparison, our software uses absolute z-score instead of p-value for selection, which is mathematically equivalent but avoids numerical underflow. 
2. Multiallelic SNPs sharing the same SNP ID are excluded due to the biallelic assumptions of the current model. If you really want to include them, you need to manually rename them to distinct SNP IDs in the input files. 
3. SNPs with identical genotypes across all individuals are excluded.
4. When collinearity issues arise among user-provided SNPs during conditional analysis (`--cojo-cond`) or joint analysis (`--cojo-joint`), GCTA terminates without output. In contrast, our software iteratively removes problematic SNPs until the issue is resolved. Removed SNPs are recorded in the `.log` file.

Output format differences:
1. In output files, both **A1** and **A2** are reported for each SNP. **A1** corresponds to **refA** in GCTA outputs.  
2. By default, our software does **not** generate `.cma.cojo` and `.ldr.cojo` files, as they can be very large and are not required for most use cases. Use `--output-all` to enable all output files, which will also record unqualified SNPs in the corresponding `.badsnps` files.

{: .highlight }
For a complete list of supported command-line options, please refer to the [Command Options](https://light156.github.io/multi-ancestry-COJO-docs/options/) section.
