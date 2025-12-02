# Bone Marrow scRNA-seq Analysis

## Overview

This project implements a full single-cell RNA sequencing (scRNA-seq) bioinformatics workflow designed to process, clean, cluster, and biologically interpret a dataset originally labeled as human _bone marrow_. The analysis incorporates best practices from modern computational biology, ensuring that technical artifacts are removed and true biological signals are preserved.

This expanded README provides deeper explanations of the workflow, clearer justification for biological interpretations, and methodological context for each step.

* * *

## What This Project Does

The notebook executes the following major stages of analysis:

### 1\. Doublet Detection (Scrublet)

Doublets occur when two cells are captured in the same droplet, generating artificially inflated gene counts. If not removed, they form spurious clusters.

Steps performed:

*   Extract raw UMI counts either from `layers['counts']` or `.X`.
    
*   Convert to dense format for Scrublet compatibility.
    
*   Run Scrublet to compute:
    
    *   a _doublet score_ per cell
        
    *   a _binary predicted\_doublet_ assignment
        
*   Remove predicted doublets to avoid distortions during clustering.
    

**Why this matters:** doublets often masquerade as rare cell types, so removal improves downstream cluster quality.

* * *

### 2\. Normalization & Feature Selection

To ensure comparability between cells:

*   Total UMI counts are normalized (Scanpy's `normalize_total`).
    
*   Log1p transformation stabilizes variance and reduces skew from high-expression genes.
    
*   Highly variable genes (HVGs) are identified (top 1000) to focus on biologically meaningful variation.
    

**Why this matters:** Only a subset of genes drive cell-type identity; HVGs extract the most informative ones.

* * *

### 3\. Dimensionality Reduction & Clustering

The workflow uses:

*   **PCA**: projects data onto principal components capturing major variance.
    
*   **Neighbor graph**: constructs a KNN graph in PCA space.
    
*   **UMAP**: produces a 2D embedding for visualization.
    
*   **Leiden clustering**: identifies transcriptionally coherent groups.
    

Three resolutions are used:

*   **0.02** — broad, coarse grouping of major cell types.
    
*   **0.5** — balanced granularity.
    
*   **2.0** — high granularity, capturing subtle subtypes.
    

**Why multiple resolutions?** Immune tissues are hierarchical; low resolution finds major compartments, high resolution finds specialized subtypes.

* * *

### 4\. Automated Cell-Type Annotation (decoupler + PanglaoDB)

This step assigns biological identity to clusters using curated marker sets.

Procedure:

*   Retrieve PanglaoDB markers through Omnipath.
    
*   Use decoupler's ULM scoring to quantify enrichment of each marker set in each cell.
    
*   Rank enriched signatures per cluster (`rankby_group`).
    
*   Assign tentative cell-type labels.
    
*   Validate with dotplots, matrixplots, and marker expression checks.
    

**Why decoupler?** It provides gene-set–based scoring that is less sensitive to noise than single-marker approaches.

* * *

## What We Found (Enhanced Detail)

### Technical Results

*   Scrublet successfully identified doublets, resulting in a cleaner dataset.
    
*   PCA and UMAP embeddings displayed well-separated immune compartments.
    
*   Multi-resolution Leiden clustering captured both broad (e.g., T vs myeloid) and fine-grained differences (e.g., naive vs memory T cells).
    

### Biological Results

Automated scoring and manual inspection identified the following immune populations:

*   Neutrophils
    
*   Monocytes
    
*   NK cells
    
*   Dendritic cells
    
*   B-cell subtypes (naive, memory, plasma)
    
*   Gamma delta T cells
    
*   Nuocytes (ILC2-like)
    
*   MDSCs
    

These cell types are commonly found in PBMC preparations and in immune-rich environments.

* * *

## How to Run the Notebook (Expanded Instructions)

### 1\. Install Dependencies

Run:

    pip install scanpy anndata scrublet decoupler omnipath
    

Ensure Python ≥ 3.9 is installed.

### 2\. Load the Dataset

The main AnnData object must be stored as:

    bone_marrow_adata
    

If you use a different variable name, adjust the notebook accordingly.

### 3\. Run Cells in Order

The notebook is sequential. Skipping cells can cause missing layers, missing normalization, or incorrect marker indexing.

### 4\. Internet Required for Marker Download

`decoupler.op.resource()` dynamically pulls PanglaoDB signatures. A stable internet connection is required.

### 5\. Expected Outputs

You will see:

*   UMAP visualizations
    
*   Violin plots (QC + marker expression)
    
*   Dotplots of signature enrichment
    
*   Matrix plots of lineage markers
    
*   Final cell-type labels in `obs['cell_type']`
    

These outputs allow biological interpretation.

* * *

## Summary

This README explains:

*   The computational steps taken
    
*   The biological findings
    
*   How to re-run the workflow
    

This notebook serves as a **reproducible, research-grade scRNA-seq pipeline** ideal for understanding immune-cell composition.

* * *

# Biological Interpretation of Identified Cell Types

## 1\. Identified Cell Types

The following annotated populations were identified:

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
    
*   MDSCs
    
*   Platelets
    
*   Pluripotent stem cells (very small fraction)
    

  

* * *

## 2\. Biological Roles of Each Cell Type

*   **NK cells** — Innate cytotoxic lymphocytes involved in early antiviral defense; kill stressed, infected, or transformed cells.
    
*   **Gamma delta T cells** — Rapid-response lymphocytes recognizing non-classical antigens; involved in epithelial defense and tumor surveillance.
    
*   **Naive T cells** — Resting lymphocytes that have never encountered antigen; key to adaptive immune diversity.
    
*   **Nuocytes (ILC2s)** — Innate lymphoid cells producing IL‑5 and IL‑13; support tissue repair and type‑2 immunity.
    
*   **Monocytes** — Circulating phagocytes; differentiate into macrophages and dendritic cells in tissues; major producers of inflammatory cytokines.
    
*   **T memory cells** — Long-lived T cells providing rapid protective responses on reinfection.
    
*   **Plasma cells** — Long‑lived or short‑lived antibody‑secreting cells; produce IgG/IgA/IgM.
    
*   **Pancreatic progenitor cells** — Almost certainly false-positive annotation; likely progenitor-like signature overlap.
    
*   **Memory B cells** — Persist after infection/vaccination; rapidly differentiate into plasma cells during secondary exposure.
    
*   **Naive B cells** — Antigen-inexperienced B cells circulating between blood and lymphoid tissues.
    
*   **Neutrophils** — First responders to bacterial infection; highly phagocytic granulocytes.
    
*   **MDSCs** — Myeloid cells that suppress T-cell responses; emerge during chronic inflammation.
    
*   **Platelets** — Anucleate cell fragments mediating clotting and participating in immune signaling.
    
*   **Pluripotent stem cells** — Likely a mislabeled tiny progenitor cluster based on generic stem-like markers.
    

* * *

## 3\. Is the Tissue Really Bone Marrow? (Detailed Justification)

**Conclusion: No — the dataset does not represent bone marrow. It is highly consistent with PBMCs (peripheral blood mononuclear cells).**

### Strong Evidence Against Bone Marrow:

*   **Absence of erythroid lineage** (HBB+, HBA1+, AHSP+) typical of marrow.
    
*   **No megakaryocyte signatures** (PF4, ITGA2B, PPBP), which should be present.
    
*   **Very low CD34+/KIT+ progenitors**, whereas marrow normally contains several progenitor populations.
    
*   **Low neutrophil fraction (2.6%)**, inconsistent with marrow, which contains abundant immature granulocytes.
    
*   **Lymphocyte-dominant composition**, typical of PBMC preps.
    

### Additional Supporting Points:

*   Misannotation of "pancreatic progenitors" suggests non‑tissue‑specific signature interference, common in PBMC analyses.
    

* * *


## 4\. Healthy vs. Infected State — Detailed Interpretation

**Conclusion:** The sample is most consistent with a **non-acutely infected (baseline/healthy-like)** peripheral blood sample rather than an infected or inflamed bone marrow sample.

### Evidence & Reasoning (detailed)

1.  **Neutrophil fraction is low (2.6%)**
    
    *   Acute bacterial infection typically produces neutrophilia (large increase in neutrophil proportion, often >50% in whole blood). In PBMC preps neutrophils are low because of density-based separation, but 2–3% is expected background; not a sign of acute infection.
        
2.  **Monocytes (≈9%) are within normal ranges**
    
    *   Peripheral monocyte frequencies typically range 5–10% in healthy individuals. No expansion that would suggest strong systemic inflammation.
        
3.  **NK cells (≈23%) are proportionally high but not conclusive for activation**
    
    *   Elevated NK percentage can occur due to biological variation or sample handling. Crucially, the activation state matters: upregulation of cytotoxic genes (GZMB, PRF1), activation markers (CD69), or IFNG would support recent immune activation. Without such transcriptional evidence, a high NK fraction alone is not diagnostic.
        
4.  **Absence of interferon-stimulated-gene (ISG) high cluster**
    
    *   Viral infections typically produce a clear ISG-high cell state (ISG15, IFIT1, MX1) across multiple cell types. You did not report such a cluster.
        
5.  **Plasma cells (~5.5%) — mild but not diagnostic**
    
    *   A plasmablast/plasma cell expansion (several percent) can reflect recent vaccination or antigen exposure but is not definitive for acute infection. Context is required.
        
6.  **No strong MDSC or immunosuppressive signature dominance**
    
    *   While MDSCs are present (~1.9%), they are not abundant enough to suggest a major tumor- or chronic-inflammation–driven immunosuppression.
        

###   

*   **Acute bacterial infection likely** if neutrophil fraction is high _and_ neutrophils express activation/immaturity markers (MPO, ELANE, S100A8/9) at high levels.
    
*   **Acute viral infection likely** if an ISG signature is upregulated broadly and NK/T cells show cytotoxic activation (GZMB/PRF1/IFNG up).
    
*   **Adaptive response/plasmablast-driven** if plasmablasts are >~2–5% and Ig transcripts are highly expressed.
    

### Limitations & Caveats

*   **Sample type bias:** PBMC enrichment (Ficoll) removes many granulocytes and erythroid cells, making PBMCs poor for marrow-specific conclusions.
    
*   **Annotation noise:** Automated signature mapping (decoupler/PanglaoDB) can mislabel clusters; manual marker checks are essential.
    
*   **Ambient RNA & doublets:** Even after Scrublet, ambient transcripts (e.g., HBB) or residual doublets can confound lineage calling.
    

* * *

## Final Short Summary

The provided dataset most closely resembles a peripheral blood (PBMC) sample, not bone marrow. The immune landscape — dominated by NK and T cells with low neutrophil content, modest monocyte presence, and no erythroid or megakaryocyte progenitors — argues against marrow origin. Transcriptional signals consistent with acute infection (neutrophilia with activation markers, interferon-stimulated gene expression, or marked plasmablast expansion) are absent; therefore the sample is best interpreted as representing a non-acutely infected, baseline immune state.
