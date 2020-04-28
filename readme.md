RNA-seq data analysis - human glioblastoma treatment using LMP400 and Olaparib
=======================

Data source
-----------
Jing Wu Lab at NIH.
44 patient-derived human glioblastoma mRNA samples sequenced at Sequencing Facility on NextSeq using Illumina TruSeq Stranded mRNA Library Prep and paired-end sequencing.


Folder structure
-----------

```
.
├── rawData                  # raw data
|   ├── RawCountFile         # raw count file from Sequencing Facility
|   ├── sample info          # sample metadata table from Madison Butler
│   ├── filteredCounts       # filtered gene counts generated using EdgeR filterByExpr 
│       ├── filtered         # keeps genes that have a CPM of 0.2 or more in at least three samples
|       ├── sampletable      # sampletable.txt has metadata including sampleName, fileName, condition and label.
├── analysis                 # analysis folder that contains script and results
│   ├── processedData        # Intermediate text result files
|       ├── DEGs             # Gene differential expression generated using Limma Voom. 
|       ├── normalized       # Voom quantile normalized, batch-corrected counts
|       ├── IPA              # DEG gene IDs, FC, P value tables, input for IPA
|       ├── ORA_GSEA         # Gene Set Enrichment Analysis using GO BP, KEGG, Reactome gene sets database
|           ├──.*csv         # significant pathways(Padjust < 0.05) and the genes involved in these pathways
|           ├──.*pdf         # dotplot of top 10 significant pathways
│   ├── results              # various plots                    
|       ├── glimma-plots     # Interactive differential expression gene plot
|       ├── PCA              # PCA plot for each contrast generated using normalized, batch-corrected counts
|       ├── heatmap          # DEGs for each contrast
|       ├── venn             # overlap of DEGs for each drug treatment
|       ├── volcano          # visualizing significant DEGs, log2 > 1 or < -1; -log10FDR > 1.3 (FDR<0.05)
|       ├── pathview         # visualizing significant DEGs in a KEGG pathway
│   ├── *.Rmd                # R scripts 
│   ├── *.html               # Rmarkdown html knit file
├── reports                  # Powerpoing presentation, MultiQC report from SF
├── ref                      # methods and references
├── readme.md                # analysis objectives and summary                       
└── ...
```



Objectives
--------

- Generate differential expression using Limma Voom.
- Perform over-representation analysis(ORA) and Gene Set Enrichment Analysis (GSEA) using Clusterprofiler.
- Difference in response between GSC cells vs. SF cells
- What response/results are consistent in both GSC923 and GSC827
- Investigate the genes and pathways regulated by drug combinations.
- identify potential genes/pathways that drive Tivozanib resistance.


---

Summary
--------

Our goal is to use RNA sequencing to investigate the expression changes in pathways such as DNA damage, DNA repair, cell cycle signaling, and apoptosis after treatment with a drug combination. We are using established and patient-derived human glioblastoma (brain tumor) cell lines, and treating with a drug combination that induces DNA damage, cell cycle arrest, and apoptosis. Additionally, RNA-Seq would also allow us to observe expression changes in additional pathways which could lead to identification of novel mechanisms of drug action, which we are interested in as well.

Cell lines:

- GSC923
- GSC827
- SF10417
- SF10602


Contrast:

- treated-Ctrl(Olaparib, LMP400, LMP400+Olaparib Vs. Control): GSC923(GSC827, SF10417, SF10602)
- Ola – Ctrl(Olaparib Vs. Control): GSC923(GSC827, SF10417, SF10602)
- LMP – Ctrl(LMP400 Vs. Control): GSC923(GSC827, SF10417, SF10602)
- LMP.Olap - Ctrl(LMP400+Olaparib Vs. Control): GSC923(GSC827, SF10417, SF10602)
- LMP.Olap – Ola(LMP400+Olaparib Vs. Olaparib): GSC923(GSC827, SF10417, SF10602)
- LMP.Olap – LMP(LMP400+Olaparib Vs. LMP400): GSC923(GSC827, SF10417, SF10602)


The Limma package was used to test for differential gene expression between experimental conditions. Batch ID
was added as a covariate in the linear modeling.Significant differentially expressed genes were identified with a false-discovery rate ≤ 0.05 and a log(fold change) > 1. 

DEGs:

- LMP_417-Ctrl_417 : 0
- LMP_602-Ctrl_602 : 41
- LMP_827-Ctrl_827 : 0
- LMP_923-Ctrl_923 : 38
- LMP.Olap_417-Ctrl_417 : 0
- LMP.Olap_417-LMP_417 : 0
- LMP.Olap_417-Ola_417 : 0
- LMP.Olap_602-Ctrl_602 : 88
- LMP.Olap_602-LMP_602 : 23
- LMP.Olap_602-Ola_602 : 82
- LMP.Olap_827-Ctrl_827 : 0
- LMP.Olap_827-LMP_827 : 0
- LMP.Olap_827-Ola_827 : 41
- LMP.Olap_923-Ctrl_923 : 415
- LMP.Olap_923-LMP_923 : 68
- LMP.Olap_923-Ola_923 : 222
- Ola_417-Ctrl_417 : 0
- Ola_602-Ctrl_602 : 15
- Ola_827-Ctrl_827 : 0
- Ola_923-Ctrl_923 : 1
- treated_417-Ctrl_417 : 0
- treated_602-Ctrl_602 : 1
- treated_827-Ctrl_827 : 0
- treated_923-Ctrl_923 : 0

Pathway enrichment was performed using pre-ranked Gene Set Enrichment Analysis (GSEA) with the KEGG, Reactome and BP genesets from the Molecular Signatures Database. 

For GSEA, the genes for each contrast are ordered in a ranked list based on gsea_ranking_score, -log10(pvalue)*sign(log2fc). GSEA analysis reveals significant inhibition or up-regulation of gene sets. On the x-axis, the genes are ranked from the most up-regulated (left end) to the most down-regulated (right end). The “barcode” indicates the position of the genes from the biological pathway. The y-axis shows a running enrichment score (ES), which goes up when genes are encountered in the pathway and goes down otherwise—leading to an assessment of the distribution within the set of all genes. ES reflects the degree to which a set S(e.g Apoptosis) is overrepresented at the extremes.The normalized enrichment score (NES) is inferred from permutations of the gene set and the false discovery rate (FDR). NES is normalized enrichment score which account for the size of the set. Negative NES means the down-regulation and positve value means up-regulation. (ref Subramanian et al. PNAS 2005) 


IPA was performed and anaysis results shared with collaborator using their Email addresses.


CCBR contact
--------
Da Yin: yind3@nih.gov
Vishal Koparde: vishal.koparde@nih.gov

