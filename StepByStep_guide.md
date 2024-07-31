# Step-by-Step Guide for GTEX_Herniation_Ocular_Analysis

This guide provides detailed instructions on how to replicate the bioinformatics analysis for identifying SNPs associated with herniation and Ocular traits using GWAS, NCBI and GTEx data.

# Prerequisites

Before starting, ensure you have the following software installed:
- Excel 2016+
- tabix (optional) - not required if the reader is using the provided **NCBI_EFEMP1_SNP.vcf**, instead of the non-provided **NCBI_SNP_ALL.vcf.gz** file


Before starting, ensure you have the raw data files downloaded within a known location:
- **GWAS_Catalog_EFEMP1.tsv** 
- **NCBI_SNP_EFEMP1.vcf**

- Raw data files can be found in the 'raw_data/' directory
- Refer to "READEME.md" for information on gathering up to date sources of the raw data



### Steps



### Step 1: Data Extraction and Preparation

IMPORTANT NOTE: Do not perform Step 1 if the reader is using the provided **NCBI_EFEMP1_SNP.vcf**, instead of the non-provided **NCBI_SNP_ALL.vcf.gz** file. Refer to README.md for more information. 


a. Navigate to Your Raw Data Directory:

cd path/to/your/raw_data_directory

This code sets your working directory on the macOS terminal to the location of the NCBI_SNP_EFEMP1.vcf file on your comnputer


b. Extract the relevant SNP information for the EFEMP1 gene from **NCBI_SNP_ALL.vcf.gz**

tabix path/to/your/raw_data_directory/NCBI_SNP_ALL.vcf.gz 2:55865967-55924139 > NCBI_SNP_EFEMP1.vcf

This extracts the SNP information for EFEMP1 and deposits that information into a new .vcf file into the working directory location 

IMPORTANT NOTE: The Genome region for EFEMP1 was identified from ENSEMBL, avalibale at [EnsemblEFEMP1](https://asia.ensembl.org/Homo_sapiens/Gene/Summary?db=core;g=ENSG00000115380;r=2:55865967-55924139). The relevant gene region will depend on on the gene enquiring and the reference genome version. Here we chose the location for the human reference genome CRCh38. 



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


IMPORTANT NOTE: Choosing the "GWAS_CATALOG_EFEMP1" sheet and not the "NCBI_SNP_EFEMP1" sheet is essential. Excel still recognises the old "GWAS_CATALOG_EFEMP1" sheet as a .tsv file, which cannot be used in the next step. By reimporting it through "Get data" it removes the .tsv metadata. 



### Step 5: Merging data through PowerQuery 


a. While on the "GTEx Input" sheet, navigate to Data > Get Data > Launch Power Query Editor > Combine > Merge queries

b. Select the "GWAS position" column on the "GTEx Input" table and select the "NCBI Position" column on the "NCBI_SNP_EFEMP" sheet 

C. Select the "Full outer" join kind. If using the provided data, it should match 244 of 13,053 rows. 

D. Select OK

E. Expand the "NCBI_SNP_EFEMP1" collumns by selecting the bi-directional arrows on the table

F. Select Close and Load

NOTE: Thus far, the goal has been to merge data from the **NCBI_SNP_ALL.vcf.gz** or the ••NCBI_SNP_EFEMP1.vcf•• to the the **GWAS_CATALOG_EFEMP1** file. This merging is essential because the GTEx eQTL dashboard requires both the reference and alternative alleles for a specific gene at a specific location. The GWAS_CATALOG_EFEMP1 table contains the alternative allele and the associated trait but lacks the reference allele. On the other hand, the NCBI table contain both the reference and alternative alleles but lack information on associated traits. Individually, these files are incomplete; however, combined, they provide all the necessary information. We have successfully merged these two files based on the common chromosome position information for each alternative/reference allele.

NOTE: It is normal to at this stage to have many empty cells. This is because the **GWAS_CATALOG_EFEMP1** is a catalog of alleles within EFEMP1 that have been assocaited with a trait -  it does not contain every single position along EFEMP1. Whereas the NCBI file(s) spans the entire gene at every single position. Thus, when the merge is performed, many sites within the NCBI table do not have a match, leaving the cells empty. 



### Step 6: Splitting the "GWAS RS/SNP" collumn into seperate entities 

a. Add a new collumn in the "GTEx Input" sheet called "RS Isolated" and input the code " =LEFT(F2, FIND("-", F2) - 1) "

This code will split the RS value from the RS/SNP string from the GWAS RS/SNP Collumn located in Collumn F 

b. Add a new collumn in the "GTEx Input" sheet called "GWAS SNP Isolated" and input the code " =MID(F2, FIND("-", F2) + 1, 1) "

This code will split the SNP value from the RS/SNP string from the GWAS RS/SNP Collumn located in Collumn F 

IMPORTANT NOTE: It is normal at this stage to have many "Value" errors in many rows - this is due to the phenoomenon outlined above, in which every single postions along the NCBI table does not have a match in the GWAS_CATALOG_EFEMP1 table. 



### Step 7: Creating a GTEx Format column 

a. Add a new collumn on the "GTEx Input" sheet called "GTEx Format" 

b. Input the excel code ="ENSG00000115380.19,chr2_" & [@[NCBI Position]] & "_" &[@[NCBI Reference Allele]]&"_"&[@[GWAS SNP Isolated]]&"_b38" into the "GTEx Format" collumn

This code creates the string that can be put into the GTEx eQTL dashboard



### Step 8: Creatng a duplication fitlering collumn 

a. Create a new collumn called "Criteria Filtering"

b. Insert the excel code "=IF(COUNTIF($S:$S, S2) = 1, "Unique", IF(COUNTIF($S$2:S2, S2) = 1, "Duplicate representer", "Duplicate"))" into the "Criteria Filtering" collumn

This code labels the string in the "GTEx Format" Colummn (S) as unique, Duplicate, or as a duplicate repsentor (Duplicate representor is a representive string from a group of duplicates). It then also filters the "GWAS" mapped trait collumn" (K) to see if the mapped trait is the same or distinct. This filter ensures that the same chromosome postions and reference alleles but distinct mapped traits are not filtered out further down the pipeline. 



### 9: Creating a second duplication filtering collumn 

a. Create a anew collumn called "Control Filtering"

b. Insert the excel code "=IF(COUNTIFS($S:$S, S6, $K:$K, K6) = 1, "Unique", IF(COUNTIFS($S$2:S6, S6, $K$2:K6, K6) = 1, "Duplicate representer", "Duplicate"))" into the "Control Filtering" collumn

This code labels the string in the "GTEx Format" Colummn (S) as unique, Duplicate, or as a duplicate repsentor (Duplicate representor is a representive string from a group of duplicates). Unlike the "Duplication filtering" collumn it does filter for traits.



### Step 10: Filtering process 

a. Convert all tables across all sheets to a range via Table > Convert to Range. 

b. Navigate back to the "GTEx Iput" sheet 

c. Ensure filtering header icons are active under Data > Filter 

c. Filter column "GWAS SNP Isolated" to only "A" "G" "C" "T"

d. Filter column for traits related to herniation pathologies: "GWAS Mapped Trait" for traits relating to herniation "Diphragmatic hernia" "Femoral hernia" "Hernia" "Hiatus hernia" "Ingiunal hernia" "Uterine prolapse" "Pelvic Organ Prolapse"

e. Filter column "GWAS Mapped Gene" to only "EFEMP1"

f. Filter collumn "Duplicate filtering" for "Unique" and "Duplicate representor" 

g. The collumn "GTEx Format" now contains the list of Strings which can be directly copied and pasted into eQTL GTEx 

g. Navigate to View > Custom Views and Save filtering criteria as "Herniation_Criteria". 

h. Re-filter collumn "GWAS Mapped Trait" for traits related to ocular pathologies: "Corneal resistance factor" "Central corneal thickness" "cup-to-disc ratio measurement" "optic cup area measurement" "open-angle glaucoma" "refractive error"

i. The collumn "GTEx Format" now contains the list of Strings which can be directly copied and pasted into eQTL GTEx 

j. Navigate to View > Custom Views and Save filtering criteria as "Ocular_Criteria"

k. Refilter the collumn "GWAS Mapped Trait" for all of the traits not chosen for the herniation or ocular pathologies criteria. 

l. Filter the collumn "Control filtering" for "Duplicate representor" and "Unique". This is important, as some loci with the same alternative allele, will be associated with multiple traits. Thus, cluttering the output.

m. This output serves as a control. ie. looking at changes in expression within loci not associated with herniation or ocular pathologies. 

n. Navigate to View > Custom Views and Save filtering criteria as "Control" 

o. Navigating to View > Custom Views, allows for easy switching between the two saved filtering criteria 


NOTE: "Custom Views" will be greyed out if ALL sheets across the entire excell book are not converted to a range via Table > Convert to Range

NOTE: Many inputs in the "GTEx Format" collumn will display identical reference and elternative alleles, these can be filtered out in another collumn with the code =IF(T2<> V2, "Different", "Same"). This compares the "NCBI Reference Allele" in collumnTO, and the "GWAS SNP Isolated" in collumn V. Its important to clear the filtering by navigating to Data > Filter > Clear before inserting the code. 

#IMPORTANT NOTE: Some variants in the "control" criteria will have have loci which have been associated with herniaiton/ocular pathologies AND other pathologies. Being aware of these is crucial to avoid confusion upon analysis. 



### Step 10: GTEx 

a. Go to the GTEx eQTL dashboard, avaliable at [GTExeQTL]"https://gtexportal.org/home/eqtlDashboardPage". 

b. Copy and paste the filtered "GTEx format" rows into the dashboard and select the tissues of interest. For this analysis we selected all tissues and then performed a selected anaylsis, only selecting tissues with significant changes in expression for ease of viualisation. Select Search. 

c. GTEx will display the expression of the gene of interest with the variant of interst within the tissues selected. Signficant searches will be highlighted in red. 

D. The analysis is now complete. 


Note: The results from our GTEx analysis can be found in the 'Results/' directory. This analysis was perfomed on the 17th July 2024, and the GTEx database is under countinual update. 
Note: GTEx only allows up to 30 inputs per search
Note: GTEx data is collated from 1000 individuals (as of 17th July 2023), thus, rare variants may not be present
