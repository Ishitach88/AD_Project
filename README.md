# Alzheimer's Disease Single-Nucleus Transcriptomic Atlas: Integrated Analysis of Cellular States, Communication Networks, and Lineage Dynamics

---

## Authors

**Sri Sathya Sandilya Garemilla**
**Ishita Chopra**

---

## Project Overview

Alzheimer's disease (AD) is a progressive neurodegenerative disorder characterized by cognitive decline, neuronal dysfunction, neuroinflammation, and extensive cellular remodeling. Recent advances in single-nucleus RNA sequencing (snRNA-seq) have enabled high-resolution characterization of cellular heterogeneity and disease-associated transcriptional programs within the human brain.

This repository presents a comprehensive downstream analysis of a publicly available Alzheimer's disease snRNA-seq dataset from the Religious Orders Study and Memory and Aging Project (ROSMAP). Using Seurat-based workflows, differential expression analysis, Gene Set Enrichment Analysis (GSEA), CellChat, and trajectory inference, we investigate disease-associated cellular states, communication networks, and lineage dynamics across major neuronal and glial populations.

In addition to reproducing core aspects of the original study, we extend the analysis through:

* Cell-cell communication network reconstruction
* Astrocyte trajectory analysis
* Microglial activation trajectory analysis
* Oligodendrocyte-lineage trajectory analysis
* Disease-associated microglia (DAM) scoring
* Integrated disease modeling

---

## Original Dataset

### BioProject

PRJNA577618

### Source

Religious Orders Study and Memory and Aging Project (ROSMAP)

### Dataset Link

https://www.ncbi.nlm.nih.gov/Traces/study/?acc=PRJNA577618

---

## Original Reference Publication

Mathys H, Davila-Velderrain J, Peng Z, et al.

**Single-cell transcriptomic analysis of Alzheimer's disease**

Nature Neuroscience (2019)

DOI:
https://doi.org/10.1038/s41593-019-0539-4

Publication:
https://www.nature.com/articles/s41593-019-0539-4

---

# Objectives

This project aims to answer the following questions:

1. Which brain cell populations exhibit the strongest transcriptional alterations in AD?
2. Which biological pathways are dysregulated across cell types?
3. How are intercellular communication networks remodeled?
4. Do astrocytes transition toward reactive inflammatory states?
5. Do microglia transition toward disease-associated microglial (DAM) states?
6. Is oligodendrocyte maturation disrupted in Alzheimer's disease?
7. How do these cellular alterations collectively contribute to disease progression?

---

# Analysis Workflow

```text
Raw snRNA-seq Counts
        │
        ▼
Quality Control
        │
        ▼
Filtering
        │
        ▼
Normalization
        │
        ▼
Integration
        │
        ▼
PCA
        │
        ▼
UMAP
        │
        ▼
Clustering
        │
        ▼
Cell-Type Annotation
        │
        ▼
AD vs Control Comparison
        │
 ┌──────┼────────────┬─────────────┐
 ▼      ▼            ▼             ▼
DEG    GSEA      CellChat    Trajectory
Analysis        Analysis      Analysis
                                   │
      ┌─────────────┬──────────────┴─────────────┐
      ▼             ▼                            ▼
Astrocytes     Microglia              Oligodendrocyte Lineage
      │             │                            │
      └─────────────┴──────────────┬─────────────┘
                                   ▼
                      Integrated Disease Model
```

# Quality Control

Quality control was performed using standard Seurat workflows.

### Metrics Evaluated

* Number of detected genes (nFeature_RNA)
* Number of UMIs (nCount_RNA)
* Mitochondrial transcript percentage

### Key Observations

* Comparable distributions between AD and Control samples.
* Low mitochondrial transcript percentages across nuclei.
* No major quality-control biases between experimental groups.
* High-quality nuclei retained for downstream analysis.

### Conclusion

Observed disease-associated transcriptional differences are unlikely to be driven by technical artifacts.

---

# Cell Clustering and Annotation

## Broad Cell Types

Eight major cell populations were identified:

| Cell Type           | Description                          |
| ------------------- | ------------------------------------ |
| Excitatory Neurons  | Glutamatergic neurons                |
| Inhibitory Neurons  | GABAergic neurons                    |
| Astrocytes          | Homeostatic and reactive astrocytes  |
| Microglia           | Brain-resident immune cells          |
| OPCs                | Oligodendrocyte precursor cells      |
| Oligodendrocytes    | Myelinating glial cells              |
| Vascular Cells      | Endothelial and vascular populations |
| Cajal-Retzius Cells | Specialized cortical neurons         |

---

## Fine Cell Types

### Excitatory Neurons

* Ex_L2_3
* Ex_L5_6
* Ex_L6
* Ex_L6b

### Inhibitory Neurons

* In_VIP
* In_SST
* In_PVALB
* In_LAMP5
* In_LAMP5_PAX6

### Glial Populations

* Astrocytes
* Reactive Astrocytes
* Microglia
* OPCs
* COPs
* Oligodendrocytes

### Other Populations

* Endothelial Cells
* Cajal-Retzius Cells

---

# Differential Gene Expression Analysis

Differential expression analysis was performed between AD and Control samples within each cell type.

## Major Findings

Cell types exhibiting the largest transcriptional changes included:

* Astrocytes
* OPCs
* Oligodendrocytes
* Ex_L2_3 neurons
* Ex_L5_6 neurons
* In_PVALB neurons

These findings indicate extensive disease-associated remodeling across both neuronal and glial populations.

---

# Gene Set Enrichment Analysis

## Astrocytes

Enriched pathways:

* Innate immune response
* Cytokine signaling
* Defense response
* Immune system activation

Interpretation:

Astrocytes acquire reactive inflammatory phenotypes in AD.

---

## Excitatory Neurons

Enriched pathways:

* ER stress
* RNA processing
* mRNA metabolism
* Apoptotic signaling

Interpretation:

Neurons exhibit molecular stress signatures consistent with neurodegeneration.

---

## Microglia

Reduced pathways:

* Synaptic signaling
* Neuron development
* Axon development
* Synapse organization

Interpretation:

Microglia lose homeostatic neuron-support functions.

---

## OPCs

Enriched pathways:

* Cell cycle
* DNA damage response
* Cytokine production
* Protein folding

Interpretation:

OPCs adopt stress-associated states.

---

## Oligodendrocytes

Reduced pathways:

* Synaptic organization
* Glutamatergic signaling
* Membrane potential regulation

Interpretation:

Mature oligodendrocytes become functionally impaired in AD.

---

# CellChat Analysis

CellChat was used to infer cell-cell communication networks.

## Global Communication Changes

| Condition | Number of Interactions | Communication Strength |
| --------- | ---------------------- | ---------------------- |
| Control   | 563                    | 48.5                   |
| AD        | 419                    | 33.6                   |

### Key Observation

AD exhibits:

* Reduced interaction number
* Reduced communication strength
* Global network simplification

---

## Communication Pathways Reduced in AD

* NRXN
* NCAM
* CNTN
* NRG
* ADGRL

These pathways support:

* Synaptic maintenance
* Cell adhesion
* Neuronal connectivity

---

## Communication Pathways Increased in AD

* SPP1
* COLLAGEN
* LAMININ
* CLDN
* EPHA

These pathways are associated with:

* Tissue remodeling
* Extracellular matrix signaling
* Neuroinflammation
* Vascular remodeling

---

# Astrocyte Trajectory Analysis

Trajectory inference identified a continuum between homeostatic and reactive astrocyte states.

### Findings

* AD cells preferentially occupied reactive states.
* Reactive astrocytes showed strong inflammatory signatures.
* Immune-response programs increased along pseudotime.

### Interpretation

Alzheimer's disease promotes astrocyte activation and neuroinflammatory remodeling.

---

# Microglia Trajectory Analysis

Trajectory analysis revealed progressive microglial activation.

### Transition

Homeostatic Microglia

↓

Activated Microglia

↓

Disease-Associated Microglia (DAM)

### Findings

* DAM scores increased along pseudotime.
* Homeostatic signatures decreased.
* AD cells accumulated within activated states.

### Interpretation

Microglia undergo disease-associated activation during AD progression.

---

# Oligodendrocyte-Lineage Trajectory Analysis

Trajectory analysis reconstructed:

OPC

↓

COP

↓

Mature Oligodendrocyte

### Findings

* AD samples displayed accumulation of intermediate COP-like states.
* Reduced terminal differentiation was observed.
* Oligodendrocyte maturation appeared impaired.

### Interpretation

Alzheimer's disease disrupts oligodendrocyte-lineage progression and myelination-associated support functions.

---

# Integrated Disease Model

## Healthy Brain

Homeostatic Astrocytes

*

Homeostatic Microglia

*

Mature Oligodendrocytes

↓

Stable Communication Networks

↓

Normal Synaptic Function

↓

Neuronal Homeostasis

---

## Alzheimer's Disease

Reactive Astrocytes

*

Disease-Associated Microglia

*

Stress-Activated OPCs

*

Dysfunctional Oligodendrocytes

↓

Communication Rewiring

↓

Loss of Neuronal Signaling

↓

Synaptic Dysfunction

↓

Progressive Neurodegeneration

---

# Repository Structure

```text
AD_snRNAseq_Analysis/
│
├── data/
│   ├── raw/
│   ├── processed/
│   └── metadata/
│
├── scripts/
│   ├── 01_qc.R
│   ├── 02_integration.R
│   ├── 03_clustering.R
│   ├── 04_annotation.R
│   ├── 05_deg_analysis.R
│   ├── 06_gsea.R
│   ├── 07_cellchat.R
│   ├── 08_astrocyte_trajectory.R
│   ├── 09_microglia_trajectory.R
│   └── 10_oligolineage_trajectory.R
│
├── results/
│   ├── figures/
│   ├── tables/
│   └── supplementary/
│
├── docs/
│   ├── manuscript/
│   ├── methods/
│   └── figures/
│
└── README.md
```

# Software

* R
* Seurat
* SeuratWrappers
* CellChat
* clusterProfiler
* Monocle3
* Slingshot
* ggplot2
* patchwork
* dplyr
* tidyverse

---

# Disclaimer

This repository represents an independent downstream analysis of the publicly available ROSMAP Alzheimer's disease snRNA-seq dataset originally generated and published by Mathys et al. (2019).

All downstream analyses, biological interpretations, CellChat analyses, trajectory analyses, and integrated disease models were generated as part of this project.

---

# Citation

If you use this repository, please cite:

Mathys H, Davila-Velderrain J, Peng Z, et al. Single-cell transcriptomic analysis of Alzheimer's disease. Nature Neuroscience. 2019. https://doi.org/10.1038/s41593-019-0539-4

and cite this repository accordingly.

---

# Contact

**Sri Sathya Sandilya Garemilla**
**Ishita Chopra**
