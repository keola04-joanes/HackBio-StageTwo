# HackBio-StageTwo
# Interpreting Cell Types and Biological Context — scRNA-seq

## 1) What cell types did you identify?

Based on the final annotated labels in `obs['cell_type']`, the dataset contains three major immune cell types:

*   **T cells** — 3,270 cells (**62.2%**)
    
*   **Monocytes** — 1,307 cells (**24.8%**)
    
*   **B cells** — 685 cells (**13.0%**)
    

## 2) Biological role of each cell type

*   **T cells:** Adaptive immune lymphocytes. CD4 T cells coordinate immune responses, while CD8 T cells kill infected or malignant cells.
    
*   **Monocytes:** Circulating innate phagocytes that engulf pathogens and debris. They can differentiate into macrophages or dendritic cells during immune activation.
    
*   **B cells:** Adaptive lymphocytes responsible for producing antibodies and differentiating into plasma cells during humoral immune responses.
    

## 3) Is the tissue source really bone marrow? (Evidence-driven conclusion)

**Conclusion: The sample is _not_ bone marrow. It is consistent with peripheral blood (PBMCs).**

### Evidence

*   **Absence of bone marrow progenitors:** No hematopoietic stem cells (CD34⁺), no myeloid progenitors, and no erythroid precursors (HBB/HBA1/GYPA). These lineages are normally abundant in bone marrow.
    
*   **No megakaryocytes or platelet precursors:** Markers such as PF4 and ITGA2B are not present.
    
*   **No granulocyte lineage:** Bone marrow typically contains mature neutrophils and neutrophil precursors. These are entirely absent here.
    
*   **Frequency distribution matches PBMCs:** T cells ~62%, monocytes ~25%, B cells ~13% — a classic peripheral blood profile.
    
*   **No stromal or endothelial niche cells**, which are common in marrow aspirates.
    

**Reasoned verdict:** The simplest and biologically supported interpretation is that the dataset represents **peripheral blood mononuclear cells**, not bone marrow.

## 4) Healthy vs infected — immune landscape interpretation

**Conclusion: The immune profile is consistent with a healthy, non-infected donor.**

### Evidence presentation (per required template)

#### Neutrophils

*   **0% of cells.** Expected markers (S100A8/A9, MPO, ELANE) do not form a cluster.
    
*   **Interpretation:** Absence of neutrophils argues against acute bacterial infection or emergency granulopoiesis.
    

#### Monocytes

*   **24.8% of cells.**
    
*   No evidence of inflammatory monocyte activation (e.g., IL1B, TNF).
    
*   **Interpretation:** Proportion and transcriptional state match healthy PBMCs without systemic inflammation.
    

#### NK cell activation states

*   No distinct NK-cell cluster.
    
*   No enrichment of activation markers (GZMB, PRF1).
    
*   **Interpretation:** No evidence of ongoing antiviral cytotoxic activity.
    

#### Lymphocytes (T + B cells)

*   T cells = **62.2%**, B cells = **13.0%**, total lymphocytes ≈ **75%**.
    
*   This is normal for PBMCs and shows **no lymphopenia**, which is common in severe infection.
    
*   **Interpretation:** Lymphocyte compartment is stable and consistent with a resting immune state.
    

### Final biological interpretation

**The lack of neutrophils, non-inflammatory monocyte profile, absence of NK/T activation signatures, and preserved lymphocyte proportions strongly support that the donor is healthy and not experiencing active infection.**
