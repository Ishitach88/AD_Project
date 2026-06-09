# 🧠 Single-Nucleus RNA-Seq Analysis of Alzheimer's Disease
### Cell-Type-Specific Transcriptomics of the Human Entorhinal Cortex

<div align="center">

![Brain](https://img.shields.io/badge/Tissue-Entorhinal%20Cortex-blue?style=for-the-badge)
![Disease](https://img.shields.io/badge/Disease-Alzheimer's%20Disease-red?style=for-the-badge)
![Method](https://img.shields.io/badge/Method-snRNA--seq-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-In%20Progress-yellow?style=for-the-badge)

**Authors:** Sri Sathya Sandilya Garemilla · Ishita Chopra

**Source Data:** [PRJNA577618](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA577618) · Grubman et al., *Nature Neuroscience* (2019)

</div>

---

## What is this project?

Alzheimer's disease doesn't affect all brain cells equally. Some populations are devastated early, some try to compensate, and some actively drive the disease forward. The question we wanted to answer was simple but hard: **which cells do what, and how do they talk to each other during disease?**

To explore this, we took a publicly available single-nucleus RNA-sequencing (snRNA-seq) dataset from human entorhinal cortex — the brain region that fails first in Alzheimer's disease — and ran a comprehensive multi-modal analysis on it. This means we didn't just look at which genes change; we looked at how cell populations transition between healthy and diseased states over time, what signals they send to each other, and where the key molecular breakpoints are.

This is an ongoing research project by Sri Sathya Sandilya Garemilla and Ishita Chopra. The code, figures, and analysis scripts are shared here for transparency and reproducibility.

---

## Table of Contents

- [The Dataset](#the-dataset)
- [Disease Background](#disease-background)
- [Key Findings](#key-findings)
- [Project Structure](#project-structure)
- [Analysis Pipeline](#analysis-pipeline)
- [How to Reproduce](#how-to-reproduce)
- [Software Requirements](#software-requirements)
- [Results at a Glance](#results-at-a-glance)
- [How This Differs from the Original Paper](#how-this-differs-from-the-original-paper)
- [Limitations](#limitations)
- [Acknowledgements](#acknowledgements)
- [Contact](#contact)

---

## The Dataset

This project is a re-analysis of publicly available data from Grubman et al. (2019). We did not generate new sequencing data — we downloaded the raw FASTQs from NCBI SRA and reprocessed them from scratch using our own pipeline.

| Property | Details |
|---|---|
| **BioProject** | [PRJNA577618](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA577618) |
| **Original Paper** | Grubman et al. (2019), *Nature Neuroscience* 22:2087–2097 |
| **DOI** | [10.1038/s41593-019-0539-4](https://doi.org/10.1038/s41593-019-0539-4) |
| **Tissue** | Human entorhinal cortex (post-mortem) |
| **Sequencing platform** | 10x Chromium single-nucleus RNA-seq |
| **Individuals** | 16 Alzheimer's disease · 16 age-matched controls (32 total) |
| **Alignment** | CellRanger (10x Genomics) |
| **Reference genome** | GRCh38 |

> The original Grubman et al. paper used n=6 AD and n=6 Control. We used all 32 available individuals from the same BioProject, giving us more statistical power to detect subtle cell-type-specific differences.

---

## Disease Background

Before diving into the analysis, here is a visual overview of what Alzheimer's disease actually does to the brain — and why studying it at single-cell resolution matters.

<div align="center">

<img width="3000" height="2100" alt="Figure_1_ AD" src="https://github.com/user-attachments/assets/13a7ae9a-5da8-45fb-aac1-bcd6a9401f8c" />


**Figure 1.** Healthy brain (left) vs Alzheimer's disease brain (right) — macroscopic structure and cellular pathology.

</div>

At the macroscopic level, the AD brain shows **cortical shrinkage**, **white matter degeneration**, and **enlarged ventricles** — the direct physical consequence of widespread cell loss over years. At the cellular level, the damage is driven by two converging processes happening simultaneously inside and outside neurons.

The first is **amyloid-beta plaques** — aggregated protein fragments that accumulate in the spaces between neurons, disrupting their ability to communicate and triggering an inflammatory response from surrounding glial cells.

The second is **tau protein tangles** — and this is where the cellular biology becomes particularly striking. In a healthy neuron, tau acts like a series of stabilising brackets along the microtubule scaffold that runs through the axon. That scaffold is the neuron's internal highway, used to transport molecules from the cell body to the synapse and back. In Alzheimer's disease, tau becomes chemically modified (hyperphosphorylated), detaches from the microtubules it was stabilising, and clumps together into tangles. The microtubule highway disintegrates. The neuron can no longer transport what it needs, can no longer communicate with its neighbours, and eventually dies.

What single-nucleus RNA sequencing adds to this picture is the ability to read the molecular response of **every cell type in the tissue simultaneously** — not just the neurons that are dying, but the microglia responding to the plaques, the astrocytes changing their identity, the oligodendrocytes failing to repair myelin, and the endothelial cells losing control of the blood-brain barrier. This is why snRNA-seq is such a powerful tool for understanding AD: it transforms the question from "what is dying?" to "what is every cell in the tissue doing about it?"

---

## Key Findings

These are the main things we found. Some confirm what the original paper reported; others are new observations that came from the additional analyses we ran.

### 🔴 Microglia instruct astrocytes to become inflammatory — via one specific signal

Using CellChat to map intercellular communication, we found a single ligand-receptor interaction — **SPP1 (from microglia) binding CD44 (on astrocytes)** — that is completely absent in healthy controls and appears only in Alzheimer's disease brains. This is the molecular signal by which disease-activated microglia push astrocytes into a pro-inflammatory reactive state. It's not just that both cell types are activated; one is directly driving the other.

### 🔴 Superficial cortical neurons are being marked for immune elimination

When we separated excitatory neurons by cortical layer, we found something striking in the Layer 2/3 population — the most abundant neurons in the cortex and the first to be lost in AD. These cells are upregulating **MHC-I antigen presentation genes, NK cell immunity pathways, and leukocyte cytotoxicity programs**. This is the transcriptional signature of cells being flagged for immune-mediated destruction. Deeper layer neurons (L5/6) show metabolic and ER stress instead — two mechanistically different ways neurons die in the same tissue.

### 🔴 Oligodendrocyte precursors try to repair but get stuck halfway

OPCs (progenitor cells that become myelin-producing oligodendrocytes) are proliferating in AD — but they're not completing the journey to mature oligodendrocytes. Our pseudotime trajectory shows cells piling up at the **committed OPC (COP) stage**, unable to cross the final differentiation checkpoint. The reason: **NF-κB inflammatory signalling** is trapping COPs in an activated state, while **Notch-HES5 signalling** is simultaneously blocking the myelination gene program. The cells are proliferating but functionally stuck.

### 🔴 The same molecular signal is disrupting two different cell types

**HES5** — a Notch pathway effector gene — rises at late pseudotime in both the astrocyte reactivity trajectory AND the oligodendrocyte differentiation trajectory. The same signal is simultaneously blocking myelination in oligodendrocytes and driving inflammatory identity in astrocytes. This makes Notch-HES5 a shared upstream driver of two otherwise distinct pathologies in AD.

### 🔴 Neurons change the signal they send to microglia

Healthy neurons communicate with microglia through **APP–SORL1** — an interaction involved in normal APP processing. In Alzheimer's disease, this switches to **APP–CD74**, which triggers microglial inflammatory activation. The amyloid precursor protein gene (APP — the central Alzheimer's gene) is sending a qualitatively different message to brain immune cells in disease.

### 🔴 The blood-brain barrier loses its cognitive function programs

Endothelial cells show the highest proportional gene dysregulation of any cell type in the dataset — nearly 90% of tested genes are significantly changed. They specifically lose gene programs for **"Learning or Memory"**, **"Cognition"**, and **"Cell Junction Organization"**. These are the molecular programs that couple blood flow to neural activity and maintain barrier integrity. Whether this is a cause or consequence of neurodegeneration is not yet clear, but the scale of change suggests it is not simply a secondary effect.

---

## Project Structure

```
AD_snRNAseq_EntorhinalCortex/
│
├── 📁 data/
│   ├── raw/                        # Raw FASTQ files (from SRA — not uploaded to GitHub)
│   ├── cellranger_output/          # Per-sample CellRanger output directories
│   │   ├── AD_1/
│   │   │   ├── outs/
│   │   │   │   ├── filtered_feature_bc_matrix/
│   │   │   │   ├── raw_feature_bc_matrix/
│   │   │   │   └── metrics_summary.csv
│   │   └── WT_1/ ...
│   └── metadata/
│       ├── sample_metadata.csv     # Sample IDs, condition, Braak stage, age, sex
│       └── SRR_accessions.txt      # SRR IDs for all 32 samples
│
├── 📁 analysis/
│   ├── 01_QC/                      # Quality control and filtering
│   │   ├── QC_01_load_and_filter.R
│   │   ├── QC_02_violin_plots.R
│   │   ├── QC_03_scatter_plots.R
│   │   └── QC_04_density_plots.R
│   │
│   ├── 02_clustering/              # Normalisation, integration, UMAP, clustering
│   │   ├── 01_normalise.R
│   │   ├── 02_integration_RPCA.R
│   │   ├── 03_clustering_leiden.R
│   │   └── 04_umap_visualisation.R
│   │
│   ├── 03_annotation/              # Cell type annotation
│   │   ├── 01_broad_annotation.R   # 8 broad cell types
│   │   ├── 02_fine_annotation.R    # 17 fine subtypes
│   │   └── 03_marker_dotplots.R
│   │
│   ├── 04_DEG/                     # Differential gene expression (DESeq2)
│   │   ├── 01_pseudobulk_DESeq2.R
│   │   └── 02_volcano_plots.R
│   │
│   ├── 05_GSEA/                    # Gene set enrichment analysis
│   │   ├── 01_GSEA_broad.R
│   │   ├── 02_GSEA_fine.R
│   │   └── 03_barplots.R
│   │
│   ├── 06_CellChat/                # Cell-cell communication inference
│   │   ├── 01_cellchat_AD.R
│   │   ├── 02_cellchat_Control.R
│   │   ├── 03_comparison.R
│   │   └── 04_LR_bubble_plots.R
│   │
│   └── 07_Pseudotime/              # Trajectory analysis (Monocle3)
│       ├── 01_astrocyte_trajectory.R
│       ├── 02_microglia_trajectory.R
│       ├── 03_oligo_lineage_trajectory.R
│       └── 04_DAM_signature_scoring.R
│
├── 📁 figures/                     # All output figures (organised by analysis)
│   ├── Figure_1_AD.png             # Disease overview figure (healthy vs AD brain)
│   ├── QC/
│   ├── UMAP/
│   ├── Volcano/
│   ├── GSEA/
│   ├── CellChat/
│   └── Pseudotime/
│
├── 📁 results/                     # Saved R objects and result tables
│   ├── seurat_integrated.rds       # Full integrated Seurat object
│   ├── DEG_tables/                 # Per-cell-type DESeq2 results (.csv)
│   ├── GSEA_tables/                # Per-cell-type GSEA results (.csv)
│   └── cellchat_objects/           # CellChat objects for AD and Control
│
├── 📄 README.md                    # This file
├── 📄 requirements_R.txt           # R package versions
└── 📄 environment.yml              # Conda environment for Python tools
```

> **Note:** Raw FASTQs and large intermediate files (Seurat .rds objects) are not uploaded to GitHub due to file size. Download raw data from NCBI SRA using the accession list in `data/metadata/SRR_accessions.txt`.

---

## Analysis Pipeline

Here's what we did, step by step:

```
Raw FASTQ files (downloaded from SRA — PRJNA577618)
          │
          ▼
  ┌───────────────────────────────────────────────────────┐
  │  STEP 1 — CellRanger (10x Genomics)                   │
  │                                                       │
  │  cellranger count per sample                          │
  │  • Reference: GRCh38 (refdata-gex-GRCh38-2020-A)     │
  │  • Output: filtered_feature_bc_matrix per sample      │
  │  • 32 samples run independently                       │
  └───────────────────────────────────────────────────────┘
          │
          ▼
  ┌───────────────────────────────────────────────────────┐
  │  STEP 2 — Quality Control (Seurat)                    │
  │                                                       │
  │  Filters applied per nucleus:                         │
  │  • nFeature_RNA: 200 – 5,000 genes                    │
  │  • nCount_RNA: 500 – 12,500 UMIs                      │
  │  • percent.mt: < 10%                                  │
  │  • Mitochondrial genes removed post-filter            │
  └───────────────────────────────────────────────────────┘
          │
          ▼
  ┌───────────────────────────────────────────────────────┐
  │  STEP 3 — Normalisation & Integration (Seurat RPCA)   │
  │                                                       │
  │  • LogNormalize (scale factor 10,000)                 │
  │  • 2,000 highly variable genes                        │
  │  • RPCA integration across all 32 samples             │
  │  • PCA → UMAP → Leiden clustering (resolution 0.5)   │
  │  → 20 initial clusters                                │
  └───────────────────────────────────────────────────────┘
          │
          ▼
  ┌───────────────────────────────────────────────────────┐
  │  STEP 4 — Cell Type Annotation                        │
  │                                                       │
  │  Broad (8 types):                                     │
  │  Excitatory Neurons, Inhibitory Neurons, Astrocytes,  │
  │  Microglia, OPCs, Oligodendrocytes, Endothelial,      │
  │  Cajal-Retzius                                        │
  │                                                       │
  │  Fine (17 subtypes):                                  │
  │  Layer-specific neurons, interneuron subtypes,        │
  │  Reactive astrocytes, COPs, and more                  │
  └───────────────────────────────────────────────────────┘
          │
          ▼
  ┌───────────────────────────────────────────────────────┐
  │  STEP 5 — Differential Expression (DESeq2)            │
  │                                                       │
  │  • Pseudobulk: counts aggregated per sample           │
  │  • Wald test with BH correction                       │
  │  • Run independently per fine cell subtype            │
  │  • Threshold: padj < 0.05, |log2FC| > 0.25           │
  └───────────────────────────────────────────────────────┘
          │
          ▼
  ┌───────────────────────────────────────────────────────┐
  │  STEP 6 — Gene Set Enrichment Analysis (GSEA)         │
  │                                                       │
  │  • clusterProfiler + GO Biological Process gene sets  │
  │  • Genes pre-ranked by DESeq2 test statistic          │
  │  • Run at broad resolution (8 types)                  │
  │  • Run at fine resolution (16 subtypes)               │
  │  • 1,000 permutations, q-value threshold < 0.05       │
  └───────────────────────────────────────────────────────┘
          │
          ▼
  ┌───────────────────────────────────────────────────────┐
  │  STEP 7 — Cell-Cell Communication (CellChat v1.6)     │
  │                                                       │
  │  • Run separately on AD and Control Seurat objects    │
  │  • Compared: interaction count, strength, info flow   │
  │  • Identified AD-gained and Control-enriched LR pairs │
  │  • Bubble plots for key biological communication axes │
  └───────────────────────────────────────────────────────┘
          │
          ▼
  ┌───────────────────────────────────────────────────────┐
  │  STEP 8 — Pseudotime Trajectories (Monocle3)          │
  │                                                       │
  │  Three lineages analysed independently:               │
  │  • Astrocyte reactivity (homeostatic → reactive)      │
  │  • Microglia activation (homeostatic → DAM)           │
  │  • Oligodendrocyte lineage (OPC → COP → mature)       │
  │                                                       │
  │  • DAM + homeostatic signature scores along trajectory│
  │  • Top trajectory-varying genes (Moran's I test)      │
  │  • AD vs Control pseudotime distribution comparison   │
  └───────────────────────────────────────────────────────┘
```

---

## How to Reproduce

### Step 1 — Download the raw data from SRA

```bash
# Install SRA Toolkit: https://github.com/ncbi/sra-tools/wiki/01.-Downloading-SRA-Toolkit

# Download all 32 samples using the accession list
prefetch --option-file data/metadata/SRR_accessions.txt

# Convert to FASTQ
for sra in *.sra; do
    fasterq-dump --split-files "$sra" --outdir fastq/
done
```

### Step 2 — Run CellRanger per sample

```bash
# Download the 10x reference genome (GRCh38)
wget https://cf.10xgenomics.com/supp/cell-exp/refdata-gex-GRCh38-2020-A.tar.gz
tar -xvf refdata-gex-GRCh38-2020-A.tar.gz

# Run CellRanger for each sample
# Replace SAMPLE_ID, R1_PATH, R2_PATH for each of your 32 samples

cellranger count \
  --id=SAMPLE_ID \
  --transcriptome=refdata-gex-GRCh38-2020-A \
  --fastqs=path/to/fastq/SAMPLE_ID/ \
  --sample=SAMPLE_ID \
  --localcores=16 \
  --localmem=64

# Output will be at: SAMPLE_ID/outs/filtered_feature_bc_matrix/
```

> Running CellRanger for all 32 samples takes significant time and compute. We recommend submitting each sample as a separate job on a computing cluster. Each sample takes roughly 2–4 hours with 16 cores and 64GB RAM.

### Step 3 — Run the R analysis pipeline

Once CellRanger output is ready, all downstream analyses are in R. Run the scripts in order:

```r
# 1. Load CellRanger output, apply QC filters, and create Seurat objects
source("analysis/01_QC/QC_01_load_and_filter.R")

# 2. Normalise, integrate across 32 samples, cluster
source("analysis/02_clustering/02_integration_RPCA.R")
source("analysis/02_clustering/03_clustering_leiden.R")

# 3. Annotate cell types (broad then fine)
source("analysis/03_annotation/01_broad_annotation.R")
source("analysis/03_annotation/02_fine_annotation.R")

# 4. Differential expression with DESeq2
source("analysis/04_DEG/01_pseudobulk_DESeq2.R")

# 5. GSEA at broad and fine resolution
source("analysis/05_GSEA/01_GSEA_broad.R")
source("analysis/05_GSEA/02_GSEA_fine.R")

# 6. CellChat cell-cell communication
source("analysis/06_CellChat/01_cellchat_AD.R")
source("analysis/06_CellChat/02_cellchat_Control.R")
source("analysis/06_CellChat/03_comparison.R")

# 7. Pseudotime trajectories
source("analysis/07_Pseudotime/01_astrocyte_trajectory.R")
source("analysis/07_Pseudotime/02_microglia_trajectory.R")
source("analysis/07_Pseudotime/03_oligo_lineage_trajectory.R")
```

---

## Software Requirements

### R packages

```r
# Single-cell analysis
install.packages("Seurat")           # v4.3+

# Differential expression
BiocManager::install("DESeq2")       # v1.42+

# GSEA and pathway analysis
BiocManager::install("clusterProfiler")
BiocManager::install("org.Hs.eg.db")
install.packages("msigdbr")

# Cell-cell communication
devtools::install_github("jinworks/CellChat")   # v1.6+

# Trajectory analysis
devtools::install_github("cole-trapnell-lab/monocle3")  # v1.3+

# Visualisation
install.packages(c("ggplot2", "patchwork", "ggrepel", "viridis"))
```

Full version-locked package list is in `requirements_R.txt`.

### System requirements

| Resource | Minimum | Recommended |
|---|---|---|
| RAM | 64 GB | 128 GB |
| Storage | 100 GB | 200 GB |
| CPU cores | 8 | 16–32 |
| R version | 4.2 | 4.3+ |
| OS | Linux / macOS | Linux (cluster) |

> CellRanger and CellChat are the most memory-intensive steps. If you're working on a laptop, consider running on a cloud instance (AWS, Google Cloud) or a university HPC cluster for those steps.

---

## Results at a Glance

### Cell types identified

| Broad Type | Fine Subtypes |
|---|---|
| Excitatory Neurons | Ex_L2_3, Ex_L5_6, Ex_L6, Ex_L6b |
| Inhibitory Neurons | In_SST, In_PVALB, In_VIP, In_LAMP5, In_LAMP5_PAX6 |
| Astrocytes | Astrocytes, Astrocytes_Reactive |
| Oligodendrocyte lineage | OPCs, COPs, Oligodendrocytes |
| Other | Microglia, Endothelial, Cajal_Retzius |

### DEG summary per cell type

| Cell Type | Genes Up in AD | Genes Down in AD | Top Finding |
|---|---|---|---|
| Ex_L2_3 | 1,052 | 2,174 | Immune gene induction (MHC-I) |
| Astrocytes | 757 | 1,092 | Complete innate immune reprogramming |
| COPs | 596 | 297 | NF-κB + HES5 differentiation block |
| Oligodendrocytes | 542 | 372 | APOE↑, LINGO3↓, synaptic loss |
| Microglia | 347 | 511 | DAM transition, homeostatic loss |
| Endothelial | 213 | 296 | BBB integrity and cognition programs lost |

### The simplified disease circuit

```
  Amyloid / Tau accumulation
            │
            ▼
  Neurons: APP–SORL1 → APP–CD74 switch
            │
            ▼
  Microglia: Homeostatic ──────────► DAM state
            │                         │
            │                    SPP1 secreted
            │                         │
            │                         ▼
            │              Astrocytes: CD44 activated
            │                         │
            │                         ▼
            │              Reactive astrogliosis
            │              (HES5, IFI6, CCL2)
            │
            ▼
  Oligodendrocytes: COPs stall
  (NF-κB active + Notch-HES5 blocks myelination)
            │
            ▼
  Neurons: trophic support lost
           NRG3–ERBB4 gone
           MHC-I induced in L2/3
            │
            ▼
  Synaptic failure → Neuronal loss
```

---

## How This Differs from the Original Paper

We want to be completely transparent: this project re-analyses the same dataset as Grubman et al. (2019). We did not generate new sequencing data. What we did was apply a different and expanded set of analytical methods that weren't part of the original study.

| Grubman et al. (2019) | This project |
|---|---|
| n=6 AD, n=6 Control | n=16 AD, n=16 Control |
| Transcription factor regulon analysis (SCENIC) | Fine-resolution GSEA (16 subtypes) |
| GWAS risk loci integration | Cell-cell communication (CellChat) |
| Broad cell-type DEGs | Layer-specific neuronal analysis |
| – | Pseudotime trajectories (Monocle3) |
| – | DAM signature scoring along trajectory |
| – | SPP1–CD44 interaction identification |
| – | HES5 pan-glial convergence analysis |

These are complementary analyses — we cite and build on the original paper throughout. The goal was never to replicate their work but to ask new questions of the same dataset using tools developed after 2019.

---

## Limitations

We think it's important to be honest about what this analysis can and cannot tell you:

**It's a re-analysis of published data.** All findings are computational. Nothing here has been validated at the protein level or in a functional experiment. These results are starting points for hypotheses, not conclusions.

**Cross-sectional data.** Post-mortem tissue gives us a snapshot, not a time series. Our pseudotime trajectories infer a temporal order from a population of cells — they are not a direct measurement of how cells change over time in a living person.

**One brain region.** The entorhinal cortex is the most relevant region for early AD, but findings may not generalise to other affected areas like the prefrontal cortex or hippocampus.

**No experimental validation.** The SPP1–CD44 interaction, the HES5 trajectory findings, and the Ex_L2_3 immune gene induction all need orthogonal validation — ideally immunofluorescence, proteomics, or functional perturbation experiments — before strong conclusions can be drawn.

**Nuclear RNA capture.** snRNA-seq captures nuclear RNA. Rapidly turned-over cytoplasmic transcripts may be underrepresented, which affects interpretation of some synaptic gene findings.

**Rare populations.** Cajal-Retzius cells and some inhibitory neuron subtypes are rare in this dataset. Statistical power for these populations is limited.

---

## What's Next

Things we're planning to add to this project:

- **CellRank** — fate probability analysis to quantify how much less likely a COP is to become a mature oligodendrocyte in AD vs Control
- **Cell proportion analysis** — formal statistical testing of whether any cell types are gained or lost in AD (MASC or propeller)
- **Cross-dataset validation** — check whether key findings replicate in Mathys et al. (2019) or the SEA-AD dataset
- **SCENIC / pySCENIC** — transcription factor regulon analysis to complement what the original paper found with TFEB

---

## Acknowledgements

Thank you to Grubman, Chew, Ouyang, and all co-authors of the 2019 Nature Neuroscience paper for making their dataset publicly available. Open data sharing is what makes re-analyses like this possible.

Thank you to the developers of Seurat, DESeq2, clusterProfiler, CellChat, and Monocle3 — all open-source tools that the scientific community has built and maintains freely.

---

## Contact

This is an ongoing project. If you have questions about the analysis, spot something that looks wrong, or just want to talk about the biology — feel free to open an Issue on this repository.

| Author | Role | Contact |
|---|---|---|
| Sri Sathya Sandilya Garemilla | Lead analyst | [GitHub](https://github.com/srisathya-garemilla) |
| Ishita Chopra | Co-analyst | [GitHub](https://github.com/ishitachopra) |

---

<div align="center">

*"The brain is the organ of destiny. It holds within its humming mechanism secrets that will determine the future of the human race."*
— Wilder Penfield

*Built with curiosity and a genuine interest in understanding what goes wrong in the Alzheimer's disease brain, one cell at a time.*

</div>
