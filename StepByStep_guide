# Step-by-Step Guide for GTEX_Herniation_Ocular_Analysis

This guide provides detailed instructions on how to replicate the bioinformatics analysis for identifying SNPs associated with herniation and Ocular traits using GWAS, NCBI and GTEx data.

## Prerequisites

Before starting, ensure you have the following software installed:
- Python 3.8+
- pandas 1.3.0
- tabix
- Excel 2016+


Before starting, ensure you have the raw data files downloaded within a known location:
- **GWAS_Catalog_EFEMP1.tsv** 
- **NCBI_SNP_EFEMP1.vcf**

- Raw data files can be found in the 'raw_data/' directory
- Refer to "READEME.md" for information on gathering up to date sources of the raw data



######## Steps



### Step 1: Data Extraction and Preparation

##IMPORTANT NOTE: Do not perform Step 1 if the reader is using the provided **NCBI_EFEMP1_SNP.vcf**, instead of the non-provided large **NCBI_SNP_ALL.vcf** file. Refer to README.md for more information. 


a. Navigate to Your Raw Data Directory:

cd path/to/your/raw_data_directory

# This sets your working directory for the macOS terminal to the location of the NCBI_SNP_EFEMP1.vcf file 


b. Extract the relevant SNP information for the EFEMP1 gene from the NCBI genome wide SNP data 

tabix path/to/your/raw_data_directory/NCBI_SNP_ALL.vcf.gz 2:55865967-55924139 > NCBI_SNP_EFEMP1.vcf

# This extracts the SNP information for EFEMP1 and deposits that information into a new .vcf file into the working directory location 

## IMPORTANT NOTE: The Genome region was identified from ENSEMBL avalibale at (https://asia.ensembl.org/Homo_sapiens/Gene/Summary?db=core;g=ENSG00000115380;r=2:55865967-55924139). The relevant gene region will depend on on the gene enquiring and the reference genome version. Here we chose the location for CRCh38. 



### Step 2: Importing and Cleaning Data in Excel 

a. Open Excel and import NCBI_SNP_EFEMP1.vcf:

- On a new sheet navigate to Data > Get Data > From File > From Text/CSV.
- Select NCBI_SNP_EFEMP1.vcf and import it 
- Select transform data
- Delete collumns 6 to 8
- Rename collumns from left to right to:
"NCBI Chromosome"
"NCBI Position"
"NCBI RS"
"NCBI Reference Allele"
"NCBI Alternative Allele"
- Select Close and Load and name the new sheet "NCBI_SNP_EFEMP1". 

b. Import GWAS Catalog Data: 
- Create a new excel sheet and navigate to Data > Get Data > From File > From Text/CSV
- Select "GWAS_CATALOG_EFEMP1.tsv" 
- Select transform data 
- Delete all of the collumn aside from: 
"PUBMEDID"
"DiSEASE/TRAIT"
"CHR_POS"
"REPORTED GENE(S)"
"MAPPED_GENE"
"STRONGEST SNP-RISK ALLEL"
"CONTEXT"
"RISKALLELE FREQUENCY"
"P-VALUE"
"PVALUE_MLOG"
"MAPPED_TRAIT"
- Rename the columns above in order from left to right in excel to:
"GWAS PubMedID"
"GWAS Trait"
"GWAS Chromosome"
"GWAS Reported Gene(s)"
"GWAS Mapped Gene"
"GWAS RS/SNP"
"GWAS Context"
"GWAS Risk Allele Freq"
"GWAS P-value"
"GWAS Pvalue Mlog"
"GWAS Mapped Trait"
- Select Close and Load and name the new sheet "GWAS_CATALOG_EFEMP1". 



### Step 4: Inserting a new table to merge later 

a. Create a new sheet and navigate to Data > Get Data > From File > From Excell

b. Navigate to the Excell workbook currently working on and import the sheet "GWAS_CATALOG_EFEMP1" 

c. Close and load, rename the sheet to "GTEx Input" 


## IMPORTANT NOTE: Choosing the "GWAS_CATALOG_EFEMP1" sheet and not the "NCBI_SNP_EFEMP1" sheet is essential. Excel still recognises the old "GWAS_CATALOG_EFEMP1" sheet as a .tsv file, which cannot be used in the next step. By reimporting it through "Get data" it removes the .tsv metadata, allowing it to be used in the next step. 



### Step 5: Merging data through PowerQuery 


a. While on the "GTEx Input" sheet, navigate to Data > Get Data > Launch Power Query Editor > Combine > Merge queries

b. Select the "GWAS position" column on the "GTEx Input" table and select the "NCBI Position" column on the "NCBI_SNP_EFEMP" sheet 

C. Select the "Full outer" join kind. If using the provided data, it should match 244 of 13,053 rows. 

D. Select OK

E. Expand the "NCBI_SNP_EFEMP1" collumns by selecting the bi-directional arrows on the table

F. Select Close and Load


### Step 6: Splitting the "GWAS RS/SNP" collumn into seperate entities 

a. Add a new collumn in the "GTEx Input" sheet called "RS Isolated" and input the code " =LEFT(F2, FIND("-", F2) - 1) "

# This code will split the RS value from the RS/SNP string from the GWAS RS/SNP Collumn located in Collumn F 

b. Add a new collumn in the "GTEx Input" sheet called "GWAS SNP Isolated" and input the code " =LEFT(F2, FIND("-", F2) - 1) " =MID(F2, FIND("-", F2) + 1, 1) "

# This code will split the SNP value from the RS/SNP string from the GWAS RS/SNP Collumn located in Collumn F 

## IMPORTANT NOTE: It is normal at this stage to have many #VALUE in many rows - this is all of the chromosome positions in the "NCBI SNP EFEMP1" table which did not have a match within the "GWAS_CATALOG_EFEMP1" sheet - which is expected since since the NCBI SNP EFEMP1 sheet covers all of the postitions of EFEMP1, whereas the "GWAS_CATALOG_EFEMP1" sheet only covers positions with a reported GWAS SNP


### Step 7: Creating a GTEx Format column 

a. Add a new collumn on the "GTEx Input" sheet called "GTEx Format" 

b. Input the excel code " " ="ENSG00000115380.19,chr2_" & [@[NCBI Position]] & "_" &[@[NCBI Reference Allele]]&"_"&[@[GWAS SNP Isolated ]]&"_b38"" " into the "GTEx Format" collumn

# This code creates the string that can be put into the GTEx eQTL dashboard



### Step 8: Creatng a duplication fitlering collumn 

a. Create a new collumn called "Duplicate Filtering"

b. Insert the excel code "=IF(COUNTIF($S:$S, S6) = 1, "Unique", IF(COUNTIF($S$2:S6, S6) = 1, "Duplicate representer", "Duplicate"))" into the "Duplicate Filtering" collumn

# This code labels the string in the "GTEx Format" Colummn as unique, Duplicate, or as a duplicate repsentor - which is the a single representive string from a duplicate group



### Step 9: Filtering process 

a. Convert all tables across all sheets to a range via Table > Convert to Range. 

b. Navigate back to the "GTEx Iput" sheet 

c. Filter column "GWAS SNP Isolated" to only "A" "G" "C" "T"

d. Filter column "GWAS Mapped Trait" for traits relating to herniation "Diphragmatic hernia" "femoral hernia" "Hernia" "Hiatus hernia" "Ingiunal hernia" "uterine prolapse" "pelic Organ Prolapse"

e. Filter column "GWAS Mapped Gene" to only "EFEMP1"

f. Filter collumn "Duplicate filtering" to "Unique" and "Duplicate representor" 

g. The collumn "GTEx Format" now contains the list of Strings which can be directly copied and pasted into eQTL GTEx 

g. Navigate to View > Custom Views and Save filtering criteria as "Herniation_Criteria". 

h. Re-filter collumn "GWAS Mapped Trait" for traits related to ocular pathologies "cup-to-disc ratio measurement" "optic cup area measurement" "open-angle glaucoma" "refractive error"

i. Navigate to View > Custom Views and Save filtering criteria as "Ocular_Criteria"

j. The collumn "GTEx Format" now contains the list of Strings which can be directly copied and pasted into eQTL GTEx 

k. Navigating to View > Custom Views, allows for easy switching and adding various filtering programs


## NOTE: "Custom Views" will be greyed out if ALL sheets across the entire Excel book are not converted to a Range 

## NOTE: Many inputs in the "GTEx Format" collumn will display identical Reference and Alternative alleles, these can be filtered out in another collumn with the code =IF(O21 <> R21, "Different", "Same"). This compares the "NCBI Reference Allele" in collumn O, and the "GWAS SNP Isolated" in collumn R. Its important to clear the filtering by navigating to Data > Filter > Clear before inserting the code. 



### Step 10: GTEx 

a. Go to the GTEx eQTL dashboard, avaliable at "https://gtexportal.org/home/eqtlDashboardPage". 

b. Copy and paste the filtered "GTEx format" rows into the dashboard and select the tissues of interest. For this analysis we selected all tissues. 

c. Select search. GTEx will display the expression of the gene of interest with the variant of interst within the tissues selected. Signficant searches will be highlighted in red. The analysis is now complete. 


# Note: GTEx only allows up to 30 inputs per search
# Note: GTEx data is collated from 1000 individuals (as of 17th July 2023), thus, rare variants may not be present
