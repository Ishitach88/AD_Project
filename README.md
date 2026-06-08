# 🧠 Single-Nucleus RNA-Seq Analysis of Alzheimer's Disease
### A Multi-Modal Transcriptomic Study of the Human Entorhinal Cortex

<div align="center">

![Brain](https://img.shields.io/badge/Tissue-Entorhinal%20Cortex-blue?style=for-the-badge)
![Disease](https://img.shields.io/badge/Disease-Alzheimer's%20Disease-red?style=for-the-badge)
![Method](https://img.shields.io/badge/Method-snRNA--seq-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Analysis%20Complete-brightgreen?style=for-the-badge)

**Authors:** Sri Sathya Sandilya Garemilla · Ishita Chopra

**Source Data:** [PRJNA577618](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA577618) · Grubman et al., *Nature Neuroscience* (2019)

</div>

---

## What is this project about?

Alzheimer's disease doesn't affect all brain cells equally — some populations are devastated early, some compensate, some actively drive the disease forward. Understanding *which* cells do *what* and *how they talk to each other* during disease is essential for finding better treatments.

This project is a comprehensive re-analysis of a landmark snRNA-seq dataset from human entorhinal cortex — the brain region that fails earliest in Alzheimer's disease — using analytical tools that weren't applied in the original 2019 paper. Instead of just describing transcriptional changes, we reconstructed the full intercellular disease circuit: which cells send what signals to whom, how specific cell populations transition from healthy to diseased states over time, and where the key molecular bottlenecks are.

The entorhinal cortex is important because it is the gateway between the hippocampus (memory formation) and the rest of the cortex, and it accumulates tau tangles before any other brain region. Understanding what goes wrong here, at single-cell resolution, gives us a window into the earliest stages of the disease.

---

## Table of Contents

- [Dataset](#dataset)
- [Key Findings](#key-findings)
- [Project Structure](#project-structure)
- [Analysis Pipeline](#analysis-pipeline)
- [How to Reproduce](#how-to-reproduce)
- [Software Requirements](#software-requirements)
- [Results Overview](#results-overview)
- [What Makes This Different from the Original Paper](#what-makes-this-different-from-the-original-paper)
- [Limitations](#limitations)
- [Citation](#citation)
- [Contact](#contact)

---

## Dataset

| Property | Details |
|---|---|
| **BioProject** | [PRJNA577618](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA577618) |
| **Original Paper** | Grubman et al. (2019), *Nature Neuroscience* 22:2087–2097 |
| **DOI** | [10.1038/s41593-019-0539-4](https://doi.org/10.1038/s41593-019-0539-4) |
| **Tissue** | Human entorhinal cortex (post-mortem) |
| **Sequencing** | 10x Chromium single-nucleus RNA-seq |
| **Individuals** | 16 Alzheimer's disease · 16 age-matched controls (32 total) |
| **Cell types** | 8 broad · 17 fine subtypes |

The raw FASTQ files are available from NCBI SRA under PRJNA577618. Processed count matrices can be downloaded and immediately used to reproduce all analyses from the clustering step onward.

---

## Key Findings

These are the things we found that haven't been reported before for this dataset:

### 🔴 The SPP1–CD44 axis: how microglia tell astrocytes to become inflammatory
Using CellChat, we found a single ligand-receptor interaction — **SPP1 from microglia binding CD44 on astrocytes** — that is completely absent in healthy controls and appears de novo in Alzheimer's disease. This is the molecular handshake by which disease-activated microglia instruct astrocytes to switch into a pro-inflammatory state. It's not just that both cell types are activated in AD; one is actively driving the other.

### 🔴 Layer 2/3 neurons are being flagged for immune elimination
When we looked at excitatory neurons separated by cortical layer, we found that superficial layer (L2/3) neurons — the most abundant and the first to be lost in AD — are upregulating a very specific set of genes: **MHC-I antigen presentation, NK cell immunity, and leukocyte cytotoxicity programs**. This is the transcriptional signature of cells being marked for immune-mediated destruction. Deeper layer neurons (L5/6) show stress and metabolic failure instead. Two different mechanisms of neuronal death in the same tissue.

### 🔴 Oligodendrocytes try to repair but get stuck
OPCs (progenitor cells that normally become myelin-producing oligodendrocytes) are proliferating in AD — but they're not successfully completing the journey to mature oligodendrocytes. Our pseudotime trajectory shows cells accumulating at the **committed OPC (COP) stage**, unable to cross the final differentiation checkpoint. The mechanism: **NF-κB inflammatory signalling** is activating COPs while simultaneously **Notch-HES5 signalling is blocking the myelination program**. The cells are being held in a trapped inflammatory state.

### 🔴 HES5 links two apparently unrelated glial problems
**HES5**, a Notch pathway effector, rises at late pseudotime in both the astrocyte reactivity trajectory AND the oligodendrocyte differentiation trajectory. The same signalling pathway is simultaneously blocking myelination and driving astrocyte reactivity. This makes Notch-HES5 a pan-glial convergence point — two different pathologies, one shared molecular driver.

### 🔴 APP changes who it talks to
Neurons normally signal to microglia through **APP–SORL1** — a healthy interaction involved in non-amyloidogenic APP processing. In AD, this switches to **APP–CD74** — an interaction that activates microglial inflammation. The same gene (APP, the Alzheimer's gene) is sending a qualitatively different message to the immune cells of the brain.

### 🔴 The blood-brain barrier fails at its cognitive function programs
Endothelial cells (the cells lining brain blood vessels) show the highest proportional gene dysregulation of any cell type — nearly 90% of tested genes are significantly changed. More specifically, they lose the gene programs for **"Learning or Memory"**, **"Cognition"**, and **"Cell Junction Organization"** — the molecular machinery that couples blood flow to neural activity and maintains barrier integrity. This may be an early event in disease rather than a secondary consequence of neuronal loss.

---

## Project Structure

```
AD_snRNAseq_EntorhinalCortex/
│
├── 📁 data/
│   ├── raw/                    # Raw FASTQ files (downloaded from SRA)
│   ├── processed/              # Count matrices post-alignment
│   │   ├── spliced/            # For RNA velocity / CellRank
│   │   └── unspliced/
│   └── metadata/               # Sample metadata, clinical info
│
├── 📁 analysis/
│   ├── 01_QC/                  # Quality control and filtering
│   │   ├── QC_02_violin.R
│   │   ├── QC_04_scatter.R
│   │   └── QC_05_density.R
│   │
│   ├── 02_clustering/          # Integration, clustering, UMAP
│   │   ├── integration_RPCA.R
│   │   ├── clustering_leiden.R
│   │   └── umap_plots.R
│   │
│   ├── 03_annotation/          # Cell type annotation (broad + fine)
│   │   ├── broad_annotation.R
│   │   ├── fine_annotation.R
│   │   └── marker_genes.R
│   │
│   ├── 04_DEG/                 # Differential gene expression
│   │   ├── DESeq2_pseudobulk.R
│   │   └── volcano_plots.R
│   │
│   ├── 05_GSEA/                # Gene set enrichment analysis
│   │   ├── GSEA_broad.R
│   │   ├── GSEA_fine.R
│   │   └── barplots.R
│   │
│   ├── 06_CellChat/            # Cell-cell communication
│   │   ├── cellchat_AD.R
│   │   ├── cellchat_Control.R
│   │   ├── comparison.R
│   │   └── LR_bubble_plots.R
│   │
│   └── 07_Pseudotime/          # Trajectory analysis
│       ├── astrocyte_trajectory.R
│       ├── microglia_trajectory.R
│       ├── oligo_trajectory.R
│       └── DAM_signature_scoring.R
│
├── 📁 figures/                 # All output figures
│   ├── QC/
│   ├── UMAP/
│   ├── Volcano/
│   ├── GSEA/
│   ├── CellChat/
│   └── Pseudotime/
│
├── 📁 results/                 # Saved R objects and data tables
│   ├── seurat_integrated.rds
│   ├── DEG_tables/
│   ├── GSEA_tables/
│   └── cellchat_objects/
│
├── 📄 README.md                # This file
├── 📄 environment.yml          # Conda environment
├── 📄 requirements_R.txt       # R package versions
└── 📄 paper/
    └── AD_snRNAseq_Garemilla_Chopra.docx
```

---

## Analysis Pipeline

Here's exactly what we did, step by step, in plain language:

```
Raw FASTQ files (SRA: PRJNA577618)
        │
        ▼
  ┌─────────────┐
  │  STARsolo   │  ── Alignment + gene counting
  │  alignment  │     (spliced + unspliced for velocity)
  └─────────────┘
        │
        ▼
  ┌─────────────────────────────────────────────────────┐
  │  1. QUALITY CONTROL (Seurat)                        │
  │     • Filter: genes 200–5000, UMIs 500–12500,       │
  │       mitochondrial reads < 10%                     │
  │     • Remove mitochondrial genes post-filter        │
  └─────────────────────────────────────────────────────┘
        │
        ▼
  ┌─────────────────────────────────────────────────────┐
  │  2. INTEGRATION (Seurat RPCA)                       │
  │     • Normalise: LogNormalize (scale 10,000)        │
  │     • Variable features: 2,000 HVGs                 │
  │     • RPCA integration across 32 samples            │
  │     • PCA → UMAP → Leiden clustering (res 0.5)      │
  └─────────────────────────────────────────────────────┘
        │
        ▼
  ┌─────────────────────────────────────────────────────┐
  │  3. CELL TYPE ANNOTATION                            │
  │     • 8 broad types from 20 clusters                │
  │     • Sub-clustering → 17 fine subtypes             │
  │       (layer-specific neurons, COPs, reactive       │
  │        astrocytes, inhibitory subtypes)             │
  └─────────────────────────────────────────────────────┘
        │
        ▼
  ┌─────────────────────────────────────────────────────┐
  │  4. DIFFERENTIAL EXPRESSION (DESeq2)                │
  │     • Pseudobulk: aggregate counts per sample       │
  │     • Wald test, BH correction                      │
  │     • Run independently per fine cell subtype       │
  │     • Threshold: padj < 0.05, |log2FC| > 0.25      │
  └─────────────────────────────────────────────────────┘
        │
        ▼
  ┌─────────────────────────────────────────────────────┐
  │  5. GSEA (clusterProfiler, GO Biological Process)  │
  │     • Pre-rank by DESeq2 test statistic             │
  │     • Run at broad resolution (8 types)             │
  │     • Run at fine resolution (16 subtypes)          │
  │     • 1000 permutations, q < 0.05 threshold         │
  └─────────────────────────────────────────────────────┘
        │
        ▼
  ┌─────────────────────────────────────────────────────┐
  │  6. CELL-CELL COMMUNICATION (CellChat v1.6)         │
  │     • Run separately on AD and Control objects      │
  │     • Compare: count, strength, information flow    │
  │     • LR bubble plots for key biological axes       │
  │     • Identify AD-specific and Control-enriched LRs │
  └─────────────────────────────────────────────────────┘
        │
        ▼
  ┌─────────────────────────────────────────────────────┐
  │  7. PSEUDOTIME TRAJECTORIES (Monocle3)              │
  │     • Astrocyte reactivity trajectory               │
  │     • Microglia activation / DAM trajectory         │
  │     • Oligodendrocyte lineage trajectory            │
  │     • DAM + homeostatic signature scoring           │
  │     • Top trajectory-varying genes (Moran's I)      │
  └─────────────────────────────────────────────────────┘
```

---

## How to Reproduce

### Step 1 — Get the raw data

```bash
# Install SRA Toolkit first: https://github.com/ncbi/sra-tools
# Then download all runs from PRJNA577618

# List of SRR accessions (AD and Control samples)
# AD samples: AD_1 through AD_16
# Control samples: WT_1 through WT_16

prefetch --option-file SRR_accessions.txt
fasterq-dump --split-files SRR*.sra
```

### Step 2 — Align and quantify

```bash
# We used STARsolo with splicing quantification
# (needed for RNA velocity / CellRank downstream)

STAR \
  --soloType CB_UMI_Simple \
  --soloCBwhitelist 3M-february-2018.txt \
  --soloFeatures Gene Velocyto \
  --genomeDir /path/to/genome_index \
  --readFilesIn sample_R2.fastq.gz sample_R1.fastq.gz \
  --readFilesCommand zcat \
  --outSAMtype BAM SortedByCoordinate \
  --outSAMattributes NH HI nM AS CR UR CB UB GX GN sS sQ sM \
  --runThreadN 16 \
  --outFileNamePrefix ./sample_output/
```

### Step 3 — Run the analysis pipeline

```r
# All analyses are in R. Run scripts in order:

# Quality control
source("analysis/01_QC/QC_02_violin.R")

# Integration and clustering
source("analysis/02_clustering/integration_RPCA.R")

# Cell type annotation
source("analysis/03_annotation/broad_annotation.R")
source("analysis/03_annotation/fine_annotation.R")

# Differential expression
source("analysis/04_DEG/DESeq2_pseudobulk.R")

# GSEA
source("analysis/05_GSEA/GSEA_broad.R")
source("analysis/05_GSEA/GSEA_fine.R")

# CellChat
source("analysis/06_CellChat/cellchat_AD.R")
source("analysis/06_CellChat/cellchat_Control.R")
source("analysis/06_CellChat/comparison.R")

# Pseudotime
source("analysis/07_Pseudotime/astrocyte_trajectory.R")
source("analysis/07_Pseudotime/microglia_trajectory.R")
source("analysis/07_Pseudotime/oligo_trajectory.R")
```

> **Note:** The full pipeline takes several hours on a standard workstation. The most computationally intensive steps are RPCA integration and CellChat. We recommend at least 64GB RAM and running on a computing cluster if possible.

---

## Software Requirements

### R packages

```r
# Core analysis
install.packages(c("Seurat", "dplyr", "ggplot2", "patchwork"))

# Differential expression
BiocManager::install("DESeq2")

# GSEA
BiocManager::install(c("clusterProfiler", "org.Hs.eg.db"))
install.packages("msigdbr")

# Cell-cell communication
devtools::install_github("jinworks/CellChat")

# Trajectory analysis
devtools::install_github("cole-trapnell-lab/monocle3")
```

Full version-locked package list is in `requirements_R.txt`.

### Python (for scVelo / CellRank, optional)

```bash
conda create -n scvelo python=3.9
conda activate scvelo
pip install scvelo cellrank scanpy anndata
```

### Environment

```bash
# Reproduce the full conda environment
conda env create -f environment.yml
conda activate AD_snRNAseq
```

### System requirements
- **RAM:** minimum 64GB recommended (128GB for CellChat on full dataset)
- **Storage:** ~50GB for raw FASTQs, ~10GB for processed objects
- **OS:** Linux or macOS (Windows via WSL2)
- **R version:** 4.3+
- **Python version:** 3.9+ (for optional velocity analysis)

---

## Results Overview

### Cell types identified

| Category | Subtypes |
|---|---|
| Excitatory Neurons | Ex_L2_3, Ex_L5_6, Ex_L6, Ex_L6b |
| Inhibitory Neurons | In_SST, In_PVALB, In_VIP, In_LAMP5, In_LAMP5_PAX6 |
| Astrocytes | Astrocytes, Astrocytes_Reactive |
| Oligodendrocyte lineage | OPCs, COPs, Oligodendrocytes |
| Other | Microglia, Endothelial, Cajal_Retzius |

### Differential expression summary (AD vs Control)

| Cell Type | Up in AD | Down in AD | Notable genes |
|---|---|---|---|
| Ex_L2_3 | 1,052 | 2,174 | IRF1↑, BCL2↓, CRY1↓ |
| Astrocytes | 757 | 1,092 | HES5↑, GBP2↑, NRXN1↓ |
| OPCs | 302 | 651 | APOE↑, HES5↑, CSPG4↓ |
| COPs | 596 | 297 | SPP1↑, NFKBIA↑, LINGO3↓ |
| Oligodendrocytes | 542 | 372 | APOE↑, LINGO3↓, LMNA↑ |
| Microglia | 347 | 511 | HSPA5↑, CLU↓, FCGR3A↓ |
| Endothelial | 213 | 296 | SLC12A2↓, CRYAB↓ |
| Cajal-Retzius | 37 | 72 | SNCA↑, DAB1↓ |

### The disease circuit (simplified)

```
Amyloid/Tau accumulation
        │
        ▼
Neurons: APP–SORL1 → APP–CD74 switch
        │
        ▼
Microglia: Homeostatic → DAM (SPP1 production)
        │
        ▼ SPP1 → CD44
Astrocytes: Homeostatic → Reactive (HES5, IFI6, CCL2)
        │
        ▼ (simultaneously)
Oligodendrocytes: COPs stall (NF-κB + Notch-HES5 block)
        │
        ▼
Neurons: Loss of astrocytic trophic support
         Loss of NRG3-ERBB4, NRXN1 signals
         MHC-I induction in L2/3
        │
        ▼
Synaptic failure + neuronal loss
```

---

## What Makes This Different from the Original Paper

The original Grubman et al. (2019) paper was the first to create a single-cell atlas of the AD entorhinal cortex — a landmark contribution. Our re-analysis uses the same dataset but asks different questions with methods that weren't applied in 2019:

| What they did | What we added |
|---|---|
| Transcription factor regulon analysis (SCENIC/TFEB) | Fine-resolution GSEA at 16+ subtypes |
| Broad cell-type DEGs | Layer-specific neuronal analysis (Ex_L2_3 vs Ex_L5_6) |
| GWAS risk gene integration | Cell-cell communication (CellChat) |
| Cluster-level transcriptional characterisation | Pseudotime trajectories (Monocle3) for 3 lineages |
| – | DAM signature scoring along microglia pseudotime |
| – | SPP1–CD44 intercellular relay identification |
| – | HES5 pan-glial convergence finding |
| 6 AD + 6 Control (13,214 nuclei) | 16 AD + 16 Control (larger sample) |

These are complementary analyses — we cite and build on the original paper throughout. This is not a replacement of their work; it's an extension using the analytical toolkit that has developed since 2019.

---

## Limitations

We believe in being upfront about what this analysis cannot tell you:

**This is a re-analysis of published data.** We did not generate new sequencing data. All findings are computational and require experimental validation before clinical conclusions can be drawn.

**Cross-sectional data cannot establish causality.** Post-mortem tissue gives us a snapshot at one timepoint. Our pseudotime trajectories infer a temporal order from a population of cells, but they are not a direct measurement of time. We cannot definitively say which events come first.

**Single brain region.** The entorhinal cortex is the most relevant region for early AD, but findings may not generalise to other affected areas like the prefrontal cortex or hippocampus.

**No orthogonal validation.** The SPP1–CD44 finding, HES5 trajectory convergence, and other key results are computational. They need protein-level confirmation (immunofluorescence, Western blot) or cross-dataset replication to be considered established.

**Nuclear RNA, not whole-cell.** snRNA-seq captures the nucleus. Rapidly turned-over cytoplasmic transcripts may be underrepresented. This particularly affects interpretation of synaptic gene expression changes.

**Rare cell types.** Cajal-Retzius cells and some inhibitory interneuron subtypes are rare. Statistical power for these populations is limited, and some DEG findings may be driven by a small number of samples.

---

## Future Directions

The things we're planning to do next:

- **CellRank** — fate probability analysis using RNA velocity to quantify how much less likely a COP is to complete differentiation in AD versus Control
- **SCENIC / pySCENIC** — transcription factor regulon analysis to complement and compare with the original paper's TFEB findings
- **Cross-dataset validation** — replicate key findings in the Mathys et al. (2019) and SEA-AD datasets
- **Spatial transcriptomics** — validate the SPP1–CD44 microglia-astrocyte communication spatially using published MERFISH or Visium EC datasets
- **Cell proportion analysis** — formal statistical testing of cell-type abundance differences between AD and Control (MASC or propeller)

---

## Citation

If you use this analysis or any code from this repository, please cite:

**This work:**
```
Garemilla SSS, Chopra I. Cell-Type-Specific Transcriptional Dysregulation in 
Alzheimer's Disease: A Comprehensive Single-Nucleus RNA-Sequencing Analysis of 
the Human Entorhinal Cortex. 2024. [Preprint]
```

**The original dataset:**
```
Grubman A, Chew G, Ouyang JF, et al. A single-cell atlas of entorhinal cortex 
from individuals with Alzheimer's disease reveals cell-type-specific gene 
expression regulation. Nature Neuroscience. 2019;22(12):2087–2097.
doi: 10.1038/s41593-019-0539-4
```

**Key tools used:**
```
Hao Y, et al. Integrated analysis of multimodal single-cell data. Cell. 2021.
Love MI, et al. DESeq2. Genome Biology. 2014.
Yu G, et al. clusterProfiler. OMICS. 2012.
Jin S, et al. CellChat. Nature Communications. 2021.
Cao J, et al. Monocle3. Nature. 2019.
```

---

## Contact

Questions, feedback, or collaboration? We'd love to hear from you.

| Author | Role | Contact |
|---|---|---|
| Sri Sathya Sandilya Garemilla | Lead analyst | [GitHub](https://github.com/srisathya-garemilla) |
| Ishita Chopra | Co-analyst | [GitHub](https://github.com/ishitachopra) |

Feel free to open an **Issue** on this repository if you encounter problems running the code, find bugs, or have questions about the biological interpretation. We'll do our best to respond.

---

## Acknowledgements

We thank the original authors — Grubman, Chew, Ouyang, and colleagues — for making their dataset publicly available under PRJNA577618. Open data sharing of this kind is what makes re-analyses like ours possible, and it accelerates science for everyone.

We also thank the developers of Seurat, DESeq2, clusterProfiler, CellChat, and Monocle3 — the open-source tools that made this analysis possible.

---

<div align="center">

*"The brain is the organ of destiny. It holds within its humming mechanism secrets that will determine the future of the human race."*
— Wilder Penfield

**Made with curiosity, caffeine, and a genuine belief that understanding Alzheimer's disease at the cellular level will one day lead to better treatments.**

</div>
