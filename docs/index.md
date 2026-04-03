---
title: Home
nav_order: 1
---

# Introduction

**Manc-COJO** is a software tool for multi-ancestry conditional and joint analysis (COJO) of GWAS summary statistics. 

GWAS tests for association between a trait and SNPs one at a time, giving marginal SNP effect estimates. 
However, associations detected in GWAS are often not independent because of linkage disequilibrium (LD) between SNPs. 
To address this challenge, [COJO](https://www.nature.com/articles/ng.2213) has been proposed and widely used for single-ancestry analyses, where it identifies independent association signals through iterative conditioning on significant SNPs while jointly modelling their effects to account for LD. Building upon COJO, our multi-ancestry extension exploits population-specific LD differences to improve the detection of independent association signals and reduce false positives compared to single-ancestry COJO (and _ad hoc_ adaptations for multi-ancestry use).


{: .note }
Our software can also perform single-ancestry COJO and reproduce the result of [GCTA-COJO](https://yanglab.westlake.edu.cn/software/gcta/#COJO), but runs **substantially faster**.

For example, for the analysis of high-density lipoprotein (HDL) cholesterol in our study, involving ~6.5 million SNPs and 76,000 individuals, we compared the per-chromosome runtime of Manc-COJO (using 1 thread) with GCTA-COJO (using 5 threads), as shown below. 
According to the [Green Algorithms calculator](https://calculator.green-algorithms.org/), this translates into ~250× reduction in both carbon footprint (1.16 gCO₂e vs 289.08 gCO₂e) and energy consumption (5.02 Wh vs 1.25 kWh).

![time_HDL.png](https://light156.github.io/multi-ancestry-COJO-docs/time_HDL.png)

Please see this documentation website for installation, usage tutorials, supported command-line options, and additional details.

## Citation

If you find our paper or software useful for your research, please consider citing our paper:

> Multi-ancestry conditional and joint analysis (Manc-COJO) applied to GWAS summary statistics. Xiaotong Wang, Yong Wang, Peter M Visscher, Naomi R Wray, Loic Yengo. bioRxiv 2026.01.30.702783; doi: [https://doi.org/10.64898/2026.01.30.702783](https://doi.org/10.64898/2026.01.30.702783)


## Support

- Contact Mark ([xiaotong.wang@psych.ox.ac.uk](mailto:xiaotong.wang@psych.ox.ac.uk)) for algorithm-related questions.
- Contact Yong ([yong.wang@psych.ox.ac.uk](mailto:yong.wang@psych.ox.ac.uk)) for software-related enquries and bug reports. 
- We welcome GitHub issues, including usage feedback and new feature requests, so that discussions are visible to all users.
