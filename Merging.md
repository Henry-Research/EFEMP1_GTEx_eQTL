## Prerequisites 

	• Please ensure the following software is downloaded : 
		○ Excel 2016+ 

	• Please ensure that the NCBI_EFEMP1_Entire.vcf file has been downloaded, either through : 
		○ The 'Extracting_Gene_Region' instructions
			§ Or 
		○ Downloaded from 'raw_data'


## Step 1

	A) In excel, navigate to Data > Get Data > From File
	B) Import NCBI_EFEMP1_SNP
	C) Select Transform Data
		a. Split columns via Split Column by Delimiter 
		b. Remove columns 
			i. 1.1
			ii. 1.7
			iii. 1.8
		c. Rename Columns 
			i. 1.2 - NCBI_CHR_POS
			ii. 1.3 - NCBI_RS
			iii. 1.4 - NCBI_Reference_Allele 
	D) Select Close and Load

## Step 2

	A) In a new sheet, navigate to Data > Get Data > From File
	B) Import GWAS_CATALOG_EFEMP1.tsv
	C) Select Transform Data
		a. Remove columns
			i. DATA ADDED TO CATALOG
			ii. PUBMEDID
			iii. FIRST AUTHER
			iv. DATE
			v. JOURNAL
LINK
			vi. STUDY
			vii. DISEASE/TRAIT
			viii. INITIAL SAMPLE SIZE
			ix. REPLICATION SAMPLE SIZE
			x. REGION
			xi. CHR_ID
			xii. REPORTED GENE(S)
			xiii. UPSTRESM_GENE_ID
			xiv. DOWNSTREAM_GENE_ID
			xv. SNP_GENE_IDS
			xvi. UPSTREAM_GENE_DISTANCE
			xvii. DOWNSTREAM_GENE_DISTANCE
			xviii. MERGED
			xix. SNP_ID_CURRENT
			xx. INTERGENIC
			xxi. PVALUEMLOD
			xxii. P-VALUE(TEST)
			xxiii. OR or BETA
			xxiv. 95% CI(TEXT)
			xxv. PLATFOWM[SNP PASSINGQC]
			xxvi. CNV
			xxvii. MAPPED TRAIT URI
			xxviii. STUDY ACCESSION
			xxix. GENOTYPING TECHNOLOGY 
		b. Rename Columns
			i. CHR_POS - GWAS_CHR_POS
			ii. MAPPED_GENE - GWAS_MAPPED_GENE 
			iii. STRONGEST SNP-RISK ALLELE - GWAS_RS_SNP
			iv.  RISK ALLELE FREQUENCY - GWAS_RISK_ALLELE_FREQ
			v. CONTEXT - GWAS_CONTEXT
			vi. P-VALUE - GWAS_P-VALUE
			vii. MAPPED_TRAIT - GWAS_MAPPED_TRAIT
		c. Select Remove Columns > Errors
		d. Select Close and Load

## Step 3 
	A) Create a new sheet called "GWAS_CATALOG_EFEMP1_1"
	B) Navigate to Data > Get Data > Launch Power Query Editor
	C) Select GWAS_CATALOG_EFEMP1 Queries > Merge Queries 
	D) Select the GWAS_RS column from the GWAS_CATALOG_EFEMP1 sheet, and the NCBI_RS column from the NCBI_SNP_EFEMP1 sheet 
	E) Perform a "Full Outer" Join Kind
		a. If using the provided data files, The selection should match all 584 rows from the the GWAS_CATALOG_EFEMP1 sheet
	F) Expand All of the NCBI_RS Columns
	G) Select Close and Load 

## Step 4 
	A) Convert both sheets to Ranges via navigating to Table > Convert to Range
	B) On the newly merged GWAS_CATALOG_EFEMP1 sheet select Data > Filter
	C) Filter GWAS_CHR_POS column for "Blanks"
	D) Select and delete all the blank rows
	E) Re-expand the GWAS_CHR_POS column 


## Step 5
	A) In the GWAS_CATALOG_EFEMP1 sheet in column M, put in the formula :
		a. =MID(C2,FIND("-", C2)+1,1)
			i. C2 = GWAS_RS_SNP
		b. This code splits the GWAS SNP from the RS string 
		c. Rename the column to GWAS_SNP
	B) In column N, put in the formula : 
		a. ="ENSG00000115380.19,chr2_"&I2&"_"&K2&"_"&M2&"_b38
			i. ENSG00000115380.19 = Gene ID for EFEMP1 from www.ensembl.org
			ii. I2 = NCBI_CHR_POS
			iii. K2 = NCBI_REFERENCE_ALLELE
			iv. M2 = GWAS_REFERENCE_ALLELE
	C) This code puts each SNP into the correct format for the GTEx eQTL dashboard

## Step 6

	A) In column O, put in the formula : 
		a. IF(K2<>M2, "Different", "Same")
			i. K2 = NCBI_Reference_Allele 
			ii. M2 = GWAS_SNP
		b. This code identifies if the reference and alternative allele are distinct ("different") or the same ("Same") as one another

## Step 7

	A) Create a new sheet called "GTEx Tissues" 
	B) Insert the valid, case sensitive, Tissues sampled in the GTEx data base
		a. A list of the GTEx tissues can be found at www.gtexportal.org/home/testyourown under "allowable tissue names. Alternatively, the list of 48 tissues is under "GTEx Tissues"
	C) Navigate back to GWAS_CATALOG_EFEMP1
	D) In cell P1 put in the formula : 
		a. ='GTExTissues'!A1
		b. Drag the formula across to BK1 to capture all of the tissues 
	E) 
	F) In cell P2, put in the formula : 
		a. =GWAS_CATALOG_EFEMP1!$J2&",ENSG0000115380.19,"&'GTEx Tissues'!A$1
			i. GWAS_CATALOG_EFEMP1!$J2 = NCBI_RS
			ii. GTEx Tissues'!A$1 = GTEx Tissues
		b. Drag the formula across to BK, and down the entire table
		c. This code puts each SNP into the correct format for the GTEx eQTL calculater

## Step 8 

	A) Create a new sheet called "GTEx_Calculater_Input"
	B) Filter the GWAS_CATALOG_EFEMP1 sheet for 
		a. Column O (REF/ALT Filtering) for "Different"
		b. Column M (GWAS_SNP) for "A, G, C and T"
		c. Column K (NCBI_REFERENCE_ALLELE) for "A, G, C and T"
		d. Column H (GWAS_MAPPED_TRAIT) for "diaphragmatic hernia, femoral hernia, Hernia, Hiatus hernia, Inguinal hernia, pelvic organ prolapse, uterine prolapse"
		e. Column G (GWAS_P-VALUE) for number filters > less than 0.00000005
	C) Navigate to View > Custom Views > Add, save the filtering criteria as "Hernia"
	D) Refilter column GWAS_MAPPED_TRAIT for All
	E) Navigate to View > Custom Views > Add, save the filtering criteria as "All"

## Step 9 

	A) Navigate to View > Custom Views > Hernia > Show 
	B) Select and copy the GTEx Tissue output (P to BK collumns)
	C) In cell A2 Paste > 123 Values of the selection into the GTEx_Calculater_Input sheet 
	D) In cell CS2 put in the formula : 
		a. TOCOL(A2:AV246)
		b. This turns the range across multiple collumns, into a single column
	E) Navigate to View > Custom Views > All > Show 
	F) Select and copy the GTEx Tissue ouput (P to BK columns)
	G) In cell AX2 Paste 123 Values of the selection into the GTEx_Calculater_Input sheet 
	H) In cell CT2 put in the formula : 
		a. TOCOL(A2:AV246)
		b. This turns the range across multiple columns, into a single column
	I) Copy and Paste > 123 Values of all the values in the Hernia (CS2) column into the column CV2
	J) Copy and Paste > 123 Value of all the values in the All (CT2) column underneath the final value from the Hernia list 
	K) Select column CT2 and select Data > Remove Duplicates 
	L) Split (Via Copy and pasting) the CT2 column into separate lists of Hernia and All SNPs into Columns CV and CW respectively 
		a. The Hernia list should contain 1270 values 
		b. The All list should contain 2433 values 

##  Step 10 

	A) Copy and paste sets of the 400 strings from column CV into the GTEx eQTL Calculator available at  https://www.gtexportal.org/home/testyourown
	B) Consolidate the output for the Hernia Strings in a new sheet called "Hernia_All"
	C) Consolidate the output for the All strings in a new sheet called "All_All"
	D) In Hernia Strings, cell J2, put in the formula : 
		a. ROUND(G2,2)
		b. This rounds the NES value in column G to 2 decimal points 
	E) Repeat Step D for the All_All strings

## Step 11 

	A) On the Hernia_All sheet navigate to Data > Filter and filter column I (tissue) for : 
		a. Cell - Cultured fibroblasts 
		b. Skin - Not Sun Exposed (Suprapubic)
		c. Skin - Sun Exposed (Lower leg)
	B) Copy  and Paste Values 123 the output into a new sheet called Hernia_Skin 
	C) Repeat Step A and B for All_All, calling the new sheet All_Skin 
	D) On the Hernia_All sheet navigate to Data > Filter and filter Column I (tissue) for everything except for : 
		a. Cell - Cultured fibroblasts 
		b. Skin - Not Sun Exposed (Suprapubic)
		c. Skin - Sun Exposed (Lower leg)
	E) Copy and Paste Value 123 the output into a new sheet called Hernia_Other
	F) Repeat Step D and E for All_All, calling the new sheet All_Other

## Step 12

	A) Create a new sheet called GTEx_Calculater_Output
	B) In cells A1, the column "Bins" and a list of 1 to -1 down the column in 0.01 increments
	C) Label 6 columns from B1 to G1 the following :
		a. All_All
		b. All_Other
		c. All_Skin
		d. Hernia_All
		e. Hernia_Other
		f. Hernia_Skin 
	D) In cell B2 input the formula :
		a. ==COUNTIFS(All_All!J:J, "<=" & A2, All_All!J:J, ">" & A3)
			i. All_AllJ:J = The entire list of All_All NES values 
			ii. A2 and A3, loop for placing the NES values into the correct bin 
		b. This code is an excel loop which places the NES strings into the correct bin 
	E) Repeat step D for all other sheets made in step C 

## Step 13
	
	A) Name column K "Statistical Test Pairs" 
	B) Label the columns K2 to K9 
		a. All_All vs Hernia_All
		b. All_All vs Hernia _Skin 
		c. All_All vs Hernia_Other 
		d. Hernia_All vs Hernia_Skin
		e. Hernia_Other vs Hernia_Skin 
		f. Hernia_Skin vs All_Skin
	C) Name column L P-value 
	D) In cell L2 input the formula : 
		a. =T.TEST(Hernia_All!J2:J1693, All_All!J2:J1881,1,3)
		b. Hernia_All!J2:J1693 = Complete list of rounded NES values from the Hernia_All sheet 
		c. All_All!J2:J1881 = Complete list of rounded NES values from the All_All sheet 
		d. 1 = Assuming single tail 
		e. 3 = Assuming unequal variances
		f. This formula calculates pair-wise, single tail, assuming equal variance P-values 
	E) Repeat step D for the other pair-wise T.tests 

## Step 14

	A) Select a cell in the table 
	B) Navigate to Insert > scatter plot > lines
	C) Hide the columns 
		a. Hernia_Other
		b. Hernia_Skin
		c. All_Other
		d. All_Skin
	D) This figure compares the Hernia eQTLs to all other Non-hernia eQTLs 
	E) Unhide all columns, then hide the columns 
		a. Hernia_All
		b. All_Skin
		c. All_Other
	F) This figures displays the differential effect of Hernia eQTLs on Skin, compared to other Hernia eQTLs, and non-hernia eQTLs 

## Step 15 

	A) Ensure the P-values are Bonferroni Corrected based on the number of tests run. Here the Bonferroni Correction is 0.05 / 6 = 0.0083 

	B) Analysis Complete 
![image](https://github.com/user-attachments/assets/418ac2b8-dc77-4d12-9a2d-b3209a80be44)



Note: The results from our GTEx analysis can be found in the 'Results/' directory. This analysis was perfomed on the 17th July 2024, and the GTEx database is under countinual update. 
Note: GTEx data is collated from 838 individuals (as of 17th July 2023), thus, rare variants may not be present
