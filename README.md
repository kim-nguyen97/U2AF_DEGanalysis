# MATR3-specific RNA Editing Site Identification

Identifies high-confidence MATR3-dependent A-to-I RNA editing sites from
 HyperTRIBE data in mouse (*Mus musculus*, mm39).

## Overview

This analysis distinguishes true MATR3-driven editing events from backgr
ound noise by applying sequential quality filters and subtracting signal
 present in negative controls. Three experimental conditions are compare
d across four replicates each:

| Condition | Description |
|-----------|-------------|
| **MATR3** | Wild-type MATR3-ADAR fusion (experimental) |
| **ΔRRMcontrol** | RRM-domain-depleted MATR3-ADAR (tests RRM-independen
t binding) |
| **ADARcontrol** | ADAR alone, no MATR3 (negative control / background)
 |

## Analysis Pipeline
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
