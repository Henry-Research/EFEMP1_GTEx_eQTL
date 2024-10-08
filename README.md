# EFEMP1_GTEx_eQTL

This repository contains the raw data, instructions, and output of an analysis identifying SNPs in **EFEMP1** associated with herniation and ocular pathologies.

It also includes all necessary raw data for analyzing additional traits linked to **EFEMP1** beyond herniation and ocular pathologies. The method is adaptable to other genes available in the publicly available GWAS Catalog database.

## AIM
To generate a list of SNPs in the appropriate format for input into the GTEx eQTL Calculator at [GTEx Portal](https://gtexportal.org/home/testyourown).

## Software Requirements
- Excel 2016+
- Tabix (optional)

## Data Files
- Raw data files can be found in the `raw_data/` directory.
- **NCBI_SNP_ALL.vcf.gz**: The file is too large to be deposited on GitHub (16 GB). You have two options:
  1. Download the file from the [NCBI SNP FTP site](https://www.ncbi.nlm.nih.gov/snp/) > FTP Download > organisms > human_9606_b151_GRCh38p7 > VCF > 00-common_all.vcf.gz.
  2. Use the provided **NCBI_EFEMP1_SNP.vcf**, which contains the extracted region for **EFEMP1**.

## Data Sources
- **GWAS_Catalog_EFEMP1.tsv**: Downloaded from the [EBI GWAS Catalog](https://www.ebi.ac.uk/gwas/). It contains all reported associations for the searched gene.
- **NCBI_SNP_ALL.vcf.gz**: Obtainable from the [NCBI SNP FTP site](https://www.ncbi.nlm.nih.gov/snp/).

## GTEx
- **GTEx** (Genotype-Tissue Expression project) is a publicly available database that correlates genetic variants with gene expression levels across 838 post-mortem donors.
- The eQTL Calculator can be accessed at [GTEx Portal](https://gtexportal.org/home/testyourown).

## Results
- The final Excel file, **EFEMP1_eQTL_Output.xlsx**, can be found in the `Results/` directory.
- The complete list of **EFEMP1** SNPs with tissue annotations, **EFEMP1_GTEx_Input**, is also in the `Results/` directory and can be directly inputted into the GTEx eQTL calculator.

## Instructions
For detailed step-by-step instructions on how to perform the analysis, see **StepByStep_guide.md** in the `Instructions/` directory.
