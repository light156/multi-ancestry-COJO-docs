---
title: Home
nav_order: 1
---

# Introduction

**Manc-COJO** is a software tool for multi-ancestry conditional and joint analysis (COJO) of GWAS summary statistics. COJO identifies independent association signals by iteratively conditioning on significant SNPs and jointly modeling their effects to account for linkage disequilibrium (LD). Our multi-ancestry extension exploits population-specific LD differences to improve the detection of independent association signals and reduce false positives compared to single-ancestry COJO (and _ad hoc_ adaptations for multi-ancestry use). 

{: .note }
Our program can also perform single-ancestry COJO and reproduce the result of [original GCTA COJO], but runs much faster.

For example, for the analysis of high-density lipoprotein (HDL) cholesterol with ~6.5 million SNPs and ~76,000 individuals in our paper, the per-chromosome running time of our program (using 1 thread) and GCTA (using 5 threads), is as follows.
![time_HDL.png](https://light156.github.io/multi-ancestry-COJO-docs/time_HDL.png)

Please see this documentation website for installation, usage tutorials, supported command-line options, and additional details.

## Citation

If you find our paper or software useful for your research, please consider citing our paper: (citation to be added).

## Support

Please contact Yong ([yong.wang@stats.ox.ac.uk](mailto:yong.wang@stats.ox.ac.uk)) for software-related enquries and bug reports, or Mark ([xiaotong.wang@psych.ox.ac.uk](mailto:xiaotong.wang@psych.ox.ac.uk)) for algorithm-related questions. 
We welcome GitHub issues, including usage feedback and new feature requests, so that discussions are visible to all users.


[original GCTA COJO]: https://yanglab.westlake.edu.cn/software/gcta/#COJO
