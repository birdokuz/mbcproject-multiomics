# mbcproject-multiomics
This portfolio refers to multiomics analysis about breast cancer data sets. 
# Integrative Multi-Omics Analysis of Metastatic Breast Cancer (MBCproject)

Analysis code for the manuscript **"Integrative Multi-Omics Analysis Reveals
Reproducible Copy-Number-to-Expression Coupling in Metastatic Breast Cancer"**
(secondary analysis of the Metastatic Breast Cancer Project, with independent
validation in TCGA-BRCA).

## Overview

This repository reproduces the multi-omics integration (DIABLO), validation, and
cross-omics convergence analyses reported in the manuscript. The primary axis is
molecular subtype (HER2-positive, HR-positive/HER2-negative, triple-negative);
metastatic biopsy site is analyzed as a reference only.

## Data availability

The analysis uses publicly available, de-identified data. Data are **not** included
in this repository and must be downloaded separately:

- **MBCproject** — cBioPortal study `brca_mbcproject_2022`
  (https://www.cbioportal.org/study/summary?id=brca_mbcproject_2022);
  also available via the NCI Genomic Data Commons (project CMI-MBC) and dbGaP (phs001709).
- **TCGA-BRCA** (external validation) — cBioPortal (e.g. `brca_tcga_pan_can_atlas_2018`).

Download each study as a single export into its **own** folder. Because cBioPortal
studies share identical file names (`data_mrna_seq_v2_rsem.txt`, etc.), never mix
files from different studies in the same folder.

Expected files per study folder: `data_clinical_patient.txt`,
`data_clinical_sample.txt`, `data_mutations.txt`, `data_cna.txt`,
`data_mrna_seq_v2_rsem.txt`.

## Repository structure

| File | Purpose |
|------|---------|
| `mbc_intake_qc.R` | Data intake / identifier-consistency check (run first) |
| `mbc_pipeline_subtype_primary.R` | Main pipeline: preprocessing, subtype definition, DIABLO integration, primary figures, site reference |
| `diablo_validation_adapted.R` | Validation: BER, patient-disjoint confusion matrix, PERMANOVA (site + subtype) |
| `convergence.R` | Cross-omics convergence: integration vs RNA-only, CNA–RNA cis-coupling, coupling score |
| `ext_val.R` | External validation in TCGA-BRCA (signature + cis-coupling reproducibility) |

## Requirements

- R (>= 4.3)
- CRAN: `tidyverse`, `ggplot2`, `ggrepel`, `patchwork`, `caret`, `vegan`,
  `permute`, `circlize`, `RColorBrewer`
- Bioconductor: `mixOmics`, `maftools`, `ComplexHeatmap`

Install:

```r
install.packages(c("tidyverse","ggplot2","ggrepel","patchwork","caret",
                   "vegan","permute","circlize","RColorBrewer"))
if (!requireNamespace("BiocManager", quietly = TRUE)) install.packages("BiocManager")
BiocManager::install(c("mixOmics","maftools","ComplexHeatmap"))
```

## How to run

Set the working directory to the MBCproject folder, then run in order:

```r
source("mbc_intake_qc.R")                 # 1. verify data consistency
source("mbc_pipeline_subtype_primary.R")  # 2. main pipeline + figures (objects kept in memory)
source("diablo_validation_adapted.R")     # 3. validation metrics
source("convergence.R")                   # 4. cross-omics convergence
# 5. external validation: set tcga_dir to the TCGA-BRCA folder, then:
source("ext_val.R")
```

All analyses use `set.seed(42)` for reproducibility. Figures are written to `output/`.

## Notes on reproducibility

- Sample counts differ by analysis (integrated set n=115; subtype set n=64;
  patient-level inferential analyses n=55–56; TCGA-BRCA n=945), as described in
  the manuscript. Each analysis reports its own n.
- TCGA subtype is approximated from PAM50 and does not perfectly match the
  IHC-based three-class definition used in the primary cohort.

## Citation

If you use this code, please cite the manuscript (details to be added upon
publication) and the archived release DOI (Zenodo).

## License

Released under the MIT License (see `LICENSE`).
