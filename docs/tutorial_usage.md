---
title: Advanced Usages
parent: Tutorial
nav_order: 1
---

The usage of Manc-COJO is largely consistent with the original GCTA COJO, with extensions to support multiple cohorts and PLINK LD matrix inputs. This section summarizes the key extensions, behavioural differences, and common advanced usage patterns. 

## Extension 1: Multi-cohort support

Options `--bfile`, `--cojo-file`, `--keep`, `--remove` can accept multiple values, one per cohort.

- File paths must be provided in the same order across cohorts.
- Empty strings ("") can be used when an option is not applied to a given cohort.
- Both combined-chromosome bfiles (chromosomes 1–22 together) and per-chromosome bfiles are supported.
- Only chromosomes 1–22 are currently supported.

For example, for two cohorts, if you want to keep certain individuals in `list1.fam` for cohort 1 but remove individuals in `list2.fam` for cohort 2, use the following command:

```bash
./manc_cojo \
--bfile path1 path2 \
--cojo-file GWAS_sumstat_path1 GWAS_sumstat_path2 \
--keep list1.fam "" \
--remove "" list2.fam \
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

Options `--extract`, `--exclude`, `--fix` and `--cojo-cond` require a file containing a list of SNPs.
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


## Differences

The following behaviours differ intentionally from GCTA:

- **Output control**  
  By default, our software does not generate `.cma.cojo` and `.ldr.cojo` files, as these outputs can be very large and are not required for most use cases. Use `--output-all` to enable all output files. 
  
  This option will also record unqualified SNPs, such as those failing MAF thresholds, in the corresponding `.badsnps` files.

- **Allele reporting**  
  Both **A1** and **A2** are reported for each SNP in output files. **A1** corresponds to **refA** in the original GCTA output.

- **Collinearity handling**  
  When collinearity issues arise among user-provided SNPs during conditional analysis (`--cojo-cond`) or joint analysis (`--cojo-joint`), GCTA terminates without output. In contrast, our software iteratively removes problematic SNPs until the issue is resolved. Removed SNPs are recorded in the `.log` file.

{: .highlight }
For a complete list of supported command-line options, please refer to the [Command Options](https://light156.github.io/multi-ancestry-COJO-docs/options/) section.
