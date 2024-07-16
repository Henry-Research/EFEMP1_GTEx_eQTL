#EFEMP1_GTEx_eQTL 

This repository contains the raw data, raw ouput and step by step method for idenitfying SNPs in EFEMP1 asosciated with herniation and ocular pathologies using GWAS, NCBI and GTEx data.


This repository contains all the neccasary raw data for analysing other traits assocaited with EFEMP1 outside of herniation and ocular pathologies, and the method is applicable to other genes found within the publically avaliable GWAS Catalog data base. 


# AIM : To create a list of SNPs for a gene in the correct format which can then be fed into GTEX 


# Software Requirments 
- Python 3.8+
- pandas 1.3.0
- tabix 
- Excel 2016+ 


# Data Files 

- Raw data files can be found in the 'raw_data/' directory 

- IMPORTANT: The **NCBI_SNP_ALL.vcf** file is too large to be depostied in GITHUB (16gb). The reader has two options: 1. Download the 16gb file at the location highlighted below, or 2. perform the analysis with the provided **NCBI_EFEMP1_SNP.vcf** file, which is the extracted region for EFEMP1 from **NCBI_SNP_ALL.vcf**. 



# Data Sources 

- **GWAS_Catalog_EFEMP1.tsv** The GWAS catalog provides summary-level finding from genetic assocaition studies. Acess the catalog at [EBI GWAS Catalog](https://www.ebi.ac.uk/gwas/). Downloading the associations will download all the reported associations for the searched gene as a .tsv file

- **NCBI_SNP_ALL.vcf** The NCBI SNP data base provides all deposited SNPs for humans. Access the up-to-date repository for all known SNPs for humans at [NCBI nih](https://ftp.ncbi.nih.gov/snp/organisms/) and choose the relevant reference genome (human_9606_b151_GRCh38p7), followed by file type (VCF), followed by data type (00-All.vcf.gz)


# GTEX 

- GTEX (Genotype-Tissue Expression project) is a publically available database which collated RNA sequencing data from various tissues samples obtained from post-mortem donors. 

- Genotypes from the donors was also anlaysed to allow for the correlation of genetic variants with gene expresson levels, this is referred to as an eQTL (Expression quantitiatie tra loci)

- GTEX is publically avaliable at [GTExPortal](https://gtexportal.org/home/) 

- Access to the eQTL dashboard is publically avalibale at (https://gtexportal.org/home/eqtlDashboardPage) 


# Results 

- The final excel file from the step by step analysis can be found in the 'results/' directory as "EFEMP1_Hernia_Ocular_GITHUB.xlsx"

- The output from the GTEx eQTL analysis can be found in the 'results/' directory as "GTEx_eQTL_Hernia_Ocular"

# Instructions

- For detailed step-by-step instruction on how to perform the analysis, see 'StepByStep_guide.md' 
