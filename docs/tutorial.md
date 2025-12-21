---
title: Tutorial
nav_order: 4
has_children: true
nav_fold: false
---

# Tutorial

This tutorial walks through the complete workflow for performing COJO analysis with our software. While a detailed understanding of the COJO methodology is not required to follow the tutorial, we recommend referring to the original [publication](https://www.nature.com/articles/ng.2213) and [documentation](https://yanglab.westlake.edu.cn/software/gcta/#COJO) of single-ancestry COJO for readers who wish to explore in more depth.

## Step 1: Download the data

You can either download the tutorial data manually by clicking [here](https://github.com/light156/multi-ancestry-COJO-docs/releases/download/tutorial_data/tutorial_data.tar.gz),  
or download and extract it from the command line:

```bash
wget https://github.com/light156/multi-ancestry-COJO-docs/releases/download/tutorial_data/tutorial_data.tar.gz
tar -xzf tutorial_data.tar.gz
```

After extraction, you should see the following files:
- GWAS summary statistics file: `Height.sumstat`  
  **Must be in GCTA-COJO format**, with a header like: `SNP A1 A2 freq b se p N`, and the first 8 columns should be in this order.
- PLINK binary files[^1] [^2]: `1KGPhase3.w_hm3.bed` `1KGPhase3.w_hm3.bim` `1KGPhase3.w_hm3.fam`
- Our software (Linux): `manc_cojo`  
  (Replace with the corresponding one if you are using other systems, see [Installation](https://light156.github.io/multi-ancestry-COJO-docs/installation/))
- Example output for validation: `Height_analysis.jma.cojo.example_output`

{: .highlight } 
GWAS summary statistics come in various formats. To convert them to the GCTA-COJO format, please refer to a dedicated [tutorial](https://github.com/zlintian/SBayesRC_pipeline?tab=readme-ov-file#cojo-format) by Dr. Tian Lin.


## Step 2: Run COJO analysis for SNP selection

> Some tips before you start:
> - On **Linux** and **macOS**, run `chmod +x manc_cojo` to make sure the software has execution permission. 
> - On **macOS**, you may need to allow the software in Privacy & Settings.
> - On **Windows**, for all the following commands, replace `\` to `^` in cmd.exe or `` ` `` in PowerShell, or run all commands on one line.


Run the following command to perform COJO SNP selection:

```bash
./manc_cojo \
--bfile 1KGPhase3.w_hm3 \
--cojo-file Height.sumstat \
--out Height_analysis \
--cojo-slct
```

The analysis takes approximately 3-5 minutes, processing chromosomes 1 through 22 sequentially. 
After completion, you should obtain:
- a log file: `Height_analysis.log`
- an output file: `Height_analysis.jma.cojo`, which looks like this:
![jma_cojo_screenshot](https://light156.github.io/multi-ancestry-COJO-docs/jma_cojo_screenshot.png)

<br>
 
In the output file, each row corresponds to a single SNP: 
- `Chr SNP bp A1 A2` denote the chromosome, SNP ID, base-pair position, the effect allele, and the other allele specified in the `.bim` file. 
- `freq b se p` are taken from the input GWAS summary statistics file. `n` is the estimated effective sample size for analysis (different from `N` in GWAS summary statistics). 
- `bJ bJ_se pJ` represent the joint effect size, standard error, and p-value from a joint analysis of all the selected SNPs. 

{: .note }
Your output file `Height_analysis.jma.cojo` should be identical to the provided example output file `Height_analysis.jma.cojo.example_output`. If so, this confirms that the software is running correctly on your machine.

{: .highlight }
People are often interested in these three columns `SNP`, `A1`, and `bJ`, which can be directly used to compute polygenic scores with the [PLINK `--score` function](https://www.cog-genomics.org/plink/1.9/score). `SNP`, `A1`, and `bJ` correspond to [variant ID col.], [allele col.], and [score col.], respectively.

## Step 3: Run conditional or joint analysis

In some cases, users may wish to perform conditional analysis or joint analysis separately. This requires providing a list of SNPs of interest. As an example, we use the SNPs selected in the previous step.

Run the following command to perform joint analysis:

```bash
./manc_cojo \
--bfile 1KGPhase3.w_hm3 \
--cojo-file Height.sumstat \
--out Height_analysis_joint \
--extract Height_analysis.jma.cojo 2 header \
--cojo-joint
```

{: .highlight}
`--extract Height_analysis.jma.cojo 2 header` indicates that only SNPs listed in the file `Height_analysis.jma.cojo` are included in the analysis, SNP IDs are read from the second column of the file, and the file contains a header line.

Because the joint effects are computed using the same input files and the same set of selected SNPs, the output file 
`Height_analysis_joint.jma.cojo` should be **identical** to `Height_analysis.jma.cojo` generated in Step 2. 

<br>

Run the following command to perform conditional analysis:

```bash
./manc_cojo \
--bfile 1KGPhase3.w_hm3 \
--cojo-file Height.sumstat \
--out Height_analysis_cond \
--cojo-cond Height_analysis.jma.cojo 2 header 
```

This will produce an output file named `Height_analysis_cond.cma.cojo`. Note that `.cma.cojo` files are often substantially large.

## Step 4: Extend to multi-ancestry analysis

To extend the analysis to multiple ancestries, simply provide multiple GWAS summary statistics and LD reference panels. 

As a minimal example, suppose we have two cohorts with identical input files. Multiple cohorts are specified by appending additional file paths after ``--bfile`` and ``--cojo-file``:

```bash
./manc_cojo \
--bfile 1KGPhase3.w_hm3 1KGPhase3.w_hm3 \
--cojo-file Height.sumstat Height.sumstat \
--out Height_analysis_two_cohorts \
--cojo-slct
```

In the output file, you will now see a header like:  
`Chr	SNP	bp	A1	A2	freq.1	b.1	se.1	p.1	n.1	bJ.1	bJ_se.1	pJ.1	freq.2	b.2	se.2	p.2	n.2	bJ.2	bJ_se.2	pJ.2	bJ.ma	bJ_se.ma	pJ.ma`

- `Chr SNP bp A1 A2` have the same meanings as described above, where `A1` and `A2` are unified according to the `.bim` file of cohort 1.
- `freq.1	b.1	se.1	p.1	n.1	bJ.1	bJ_se.1	pJ.1`, and `freq.2	b.2	se.2	p.2	n.2	bJ.2	bJ_se.2	pJ.2` have the same meanings as described above, with the suffixes `.1` and `.2` indicating the cohort index (_i.e._, cohort 1 and cohort 2). 
- `bJ.ma	bJ_se.ma	pJ.ma` report the joint effect size, standard error, and p-value from **multi-cohort joint analysis** of all selected SNPs across both cohorts.

---

[^1]: Publicly available from [1000 Genomes](https://figshare.com/articles/dataset/HapMap3-1KG_1000_Genomes_processed_genotypes_in_PLINK_bed_bim_fam_format/20802700?file=37059184). We use PLINK to merge the 22 chromosomes into a single bfile for convenience.

[^2]: Documentation for PLINK binary files can be found [here](https://www.cog-genomics.org/plink/1.9/formats#bed).