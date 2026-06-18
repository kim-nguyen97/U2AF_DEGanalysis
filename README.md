# About the project

This repo contains the analysis pipeline behind a study of MATR3 (Matrin-3), an RNA-binding protein, in mouse cardiomyocytes.


## Biological background

HyperTRIBE works by fusing MATR3 to a hyperactive ADAR catalytic domain. This fusion protein introduces adenosine-to-inosine (A-to-I) editing marks on RNA near genuine MATR3 binding sites, and these marks serve as a sequencing-detectable readout of where MATR3 binds.


## Aim

Identify MATR3's high-confidence mRNA targets and characterize their downstream effects on cardiomyocyte gene expression.

## Experimental design

Three datasets are integrated:

- **HyperTRIBE** – RNA editing-based detection of candidate MATR3 binding targets

- **iCLIP** – independent validation of MATR3 binding sites

- **Bulk RNA-seq** – gene expression changes after *Matr3* knockout



| Condition | Description |

|---|---|

| **MATR3** | Wild-type MATR3–ADAR fusion (experimental) |

| **ΔRRM control** | RRM-domain-depleted MATR3–ADAR fusion (tests RRM-independent binding) |

| **ADAR control** | ADAR domain alone, without MATR3 (negative control / background) |



## Reference data
### Genome annotation

GENCODE Mouse Release M37, GRCm39 (mm39), May 2025 — https://www.gencodegenes.org/mouse/release_M37.html



### Principal isoform annotation

APPRIS — https://appris.bioinfo.cnio.es



## Software requirements

- R (version: *add*)

- RStudio

## Repository structure

1. **Highconfsites_M3_clean.qmd** – Identification of reproducible editing sites occurring specifically in the MATR3-HyperTRIBE dataset

2. **Top-tx-M3.qmd** – Hierarchical ranking of candidate MATR3 target genes into confidence tiers

3. **PureCLIP_analysis.qmd** – Peak calling and binding-site analysis from iCLIP data, for cross-validation against HyperTRIBE editing sites

4. **DESeq2_analysis.qmd** – Differential gene expression analysis of bulk RNA-seq data after cardiomyocyte-specific *Matr3* knockout



## Analysis pipeline



To distinguish genuine MATR3-driven editing events from background noise inherent to the HyperTRIBE assay, I developed sequential quality filters and discarded signal also present in negative controls.



### Technical global filtering
```
Raw HyperTRIBE calls (.bg + .txt)
        │
        ▼
1. Coverage filter       ≥ 5 reads
        │
        ▼
2. Editing score filter  ≥ 5% (G/C count ÷ total coverage × 100)
        │
        ▼
3. Reproducibility       site detected in ≥ 2 of 4 replicates
        │
        ▼
4. Background subtraction  remove sites shared with ADARcontrol
        │
        ▼
5. MATR3-specific sites  unique to MATR3 WT
        │
        ▼
6. Annotation + enrichment
   - Gene names (GENCODE vM37 / mm39)
   - Transcript region (5'UTR, CDS, 3'UTR, Intron)
   - GO enrichment (clusterProfiler)
```
