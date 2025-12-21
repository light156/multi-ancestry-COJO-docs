---
title: Using on UKB-RAP
parent: Tutorial
nav_order: 2
---

This section walks through the complete workflow for running **Manc-COJO** on **UKB-RAP**.

## Step 1: Prepare inputs 

- Create a folder named `WORK_FOLDER` for the COJO analysis.
- Download our software `manc_cojo` into `WORK_FOLDER`.
- Prepare your GWAS summary statistics file at `/SUMSTAT_FOLDER/Your_sumstat_file`.
- Generate your PLINK binary files to be used as LD references, with paths like `/BFILE_FOLDER/Plink_bfile_chr${chr}` (one per chromosome).

## Step 2: Run Manc-COJO on UKB-RAP

Modify the paths and filenames as needed, then run the following command using DNAnexus Platform dx tools. 
Installation instructions and usage guidelines are available in the [official DNAnexus documentation](https://documentation.dnanexus.com/downloads#dnanexus-platform-sdk).

```bash
for chr in {1..22}; do

run_Manc_COJO="
cp manc_cojo \$HOME/manc_cojo && chmod +x \$HOME/manc_cojo

\$HOME/manc_cojo \\
--bfile \"/mnt/project/BFILE_FOLDER/Plink_bfile_chr${chr}\" \\
--cojo-file \"Your_sumstat_file\" \\
--out output_name \\
--cojo-slct \\
--thread-num 5
"
 
dx run swiss-army-knife \
-iin="Your_Project_ID:/WORK_FOLDER/manc_cojo" \
-iin="Your_Project_ID:/SUMSTAT_FOLDER/Your_sumstat_file" \
-icmd="${run_Manc_COJO}" \
--priority high \
--cost-limit 10 \
--ignore-reuse \
--instance-type mem3_ssd1_v2_x8 \
--destination="Your_Project_ID:/WORK_FOLDER" \
--tag Manc-COJO \
--tag chr${chr} \
-y \
--brief

done
```

Note that we set `--thread-num 5` to speed up computation; this value can be adjusted depending on the compute resources available on **UKB-RAP**.