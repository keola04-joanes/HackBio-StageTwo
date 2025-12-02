# Bone Marrow scRNA-seq Analysis

## Overview

This project performs a complete single-cell RNA sequencing (scRNA-seq) analysis workflow on a human bone marrow dataset. The goal is to clean, preprocess, cluster, and annotate the cellular populations found within the data using modern computational biology tools.

* * *

## What This Project Does

The notebook walks through the following major steps:

### 1\. Doublet Detection (Scrublet)

*   Raw UMI counts are extracted.
    
*   Scrublet is used to identify potential doublets (artificial cell multiplets).
    
*   Predicted doublets are removed to improve clustering accuracy.
    

### 2\. Normalization & Feature Selection

*   Counts are normalized across cells.
    
*   Log transformation stabilizes gene expression variance.
    
*   Top 1000 highly variable genes are selected for downstream analysis.
    

### 3\. Dimensionality Reduction & Clustering

*   PCA is used to reduce dimensionality.
    
*   Nearest-neighbor graph and UMAP embedding are computed.
    
*   Leiden clustering is performed at multiple resolutions (0.02, 0.5, 2.0) to capture fine and coarse cellular structure.
    

### 4\. Cell-Type Annotation

*   PanglaoDB marker genes are retrieved through decoupler.
    
*   decoupler’s ULM scoring assigns cell-type signatures to each cell.
    
*   Clusters are annotated by ranking enriched marker sets.
    
*   Marker-based plots validate cell-type identity.
    

* * *

## What We Found

*   **Doublets were successfully removed**, improving signal quality.
    
*   UMAP showed clear separation of major immune cell populations.
    
*   Different Leiden resolutions revealed:
    
    *   Broad cell groups (low resolution)
        
    *   Fine-grained subtypes (high resolution)
        
*   Automated scoring and marker validation identified:
    
    *   **Neutrophils**
        
    *   **Macrophages / Monocytes**
        
    *   **NK cells**
        
    *   **Dendritic cells**
        
    *   **B-cell subsets**
        
*   Results reflect known bone marrow immunological diversity.
    

* * *

## How to Run the Notebook

### 1\. Install Dependencies

Use the following command:

    pip install scanpy anndata scrublet decoupler omnipath
    

### 2\. Load Your Data

Ensure your AnnData object is named:

    bone_marrow_adata
    

### 3\. Run the Notebook Sequentially

Each step builds on the previous one. Running out of order can cause errors.

### 4\. Internet Requirement

`decoupler.op.resource()` downloads PanglaoDB markers — an internet connection is required.

### 5\. Inspect Outputs

The notebook will generate:

*   UMAP plots
    
*   Violin plots
    
*   Dotplots
    
*   Matrix plots
    
*   Final cluster annotations
    

These visualizations help interpret the bone marrow’s cellular composition.

* * *

## Summary

This README provides a concise explanation of:

*   What the analysis does
    
*   Key biological insights gained
    
*   How to reproduce the workflow
    

The notebook serves as a clean, reproducible scRNA-seq analysis pipeline suitable for coursework, research, or benchmarking.

# Biological Interpretation of Identified Cell Types

## 1\. Identified Cell Types

*   NK cells
    
*   Gamma delta T cells
    
*   Naive T cells
    
*   Nuocytes (ILC2 lineage)
    
*   Monocytes
    
*   T memory cells
    
*   Plasma cells
    
*   Pancreatic progenitor cells\* (likely misannotated)
    
*   Memory B cells
    
*   Naive B cells
    
*   Neutrophils
    
*   Myeloid-derived suppressor cells (MDSCs)
    
*   Platelets
    
*   Pluripotent stem cells (very small fraction)
    

  

* * *

## 2\. Biological Roles of Each Cell Type

**NK cells:** Innate cytotoxic lymphocytes that kill virally infected or stressed cells; major producers of IFN-γ.

**Gamma delta T cells:** Unconventional T cells that rapidly respond to stress antigens; bridge innate and adaptive immunity.

**Naive T cells:** Antigen-inexperienced T cells circulating in blood/lymph; become activated upon encountering antigen.

**Nuocytes (ILC2s):** Type 2 innate lymphoid cells producing IL-5 and IL-13; involved in parasite defense and allergic inflammation.

**Monocytes:** Circulating precursors of macrophages and dendritic cells; phagocytic and inflammatory responders.

**T memory cells:** Antigen-experienced T cells that respond rapidly upon re-exposure.

**Plasma cells:** Terminally differentiated B cells that secrete high levels of antibodies.

**Pancreatic progenitor cells:** Misannotation; unlikely to exist in a blood or marrow sample.

**Memory B cells:** Antigen-experienced B cells involved in rapid secondary antibody responses.

**Naive B cells:** Antigen-inexperienced B cells circulating in blood until encountering antigen.

**Neutrophils:** Short-lived innate granulocytes; primary responders to bacterial infection.

**MDSCs:** Immunosuppressive myeloid cells seen in chronic inflammation, cancer, and some infections.

**Platelets:** Cytoplasmic fragments of megakaryocytes; essential for clotting and inflammatory signaling.

**Pluripotent stem cells:** Likely a misinterpreted progenitor-like signature rather than true PSCs.

* * *

## 3\. Is the Tissue Source Really Bone Marrow?

**Conclusion: No — the dataset does not represent bone marrow. It is more consistent with peripheral blood or PBMCs.**

### Evidence Against Bone Marrow:

*   Absence of **erythroid lineage cells** (HBB/HBA1+, AHSP+), which are abundant in bone marrow.
    
*   No **megakaryocyte** signatures (PF4, PPBP, ITGA2B).
    
*   Virtually no **HSC/MPP/CD34+ progenitors**.
    
*   Low neutrophil percentage (2.6%), whereas bone marrow typically contains large populations of immature granulocytes.
    
*   Composition dominated by **T, NK, and B cells**, typical of PBMCs.
    

### Additional Notes:

*   The presence of misannotated “pancreatic progenitors” further supports that this dataset likely comes from blood, where certain marker-based scoring tools can mislabel ambiguous progenitor-like signatures.
    

* * *

## 4\. Healthy vs. Infected State Interpretation

**Conclusion: Most consistent with a healthy or non-acutely infected individual.**

### Evidence Supporting a Healthy State:

*   **Neutrophils are low (2.6%)**, not elevated — no neutrophilia.
    
*   **Monocytes at 9%**, within normal healthy PBMC range.
    
*   NK cells are high proportionally but without known activation markers (e.g., GZMB, PRF1, IFNG), so not indicative of viral infection.
    
*   No ISG-high cluster (e.g., ISG15, IFIT1, MX1), which is typical in viral infections.
    
*   Plasma cells at ~5% could indicate mild stimulation but are still within normal or mildly elevated ranges.
    

### Summary:

The transcriptional landscape lacks hallmarks of acute bacterial or viral infection. Overall, this cell-type composition reflects a **non-infected or baseline immune state**.