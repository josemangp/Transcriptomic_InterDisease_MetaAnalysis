# Transcriptomic_InterDisease_MetaAnalysis
Integrative bulk RNA-seq analysis of purified monocytes across five systemic autoimmune diseases. Identifies a 41-gene interferon-driven core signature via precision-weighted meta-analysis and correlates transcriptomic findings with clinical flow cytometry data from a cGVHD cohort.

---
---

## Introduction

This repository contains the full analytical pipeline developed for the Master's Thesis "Core Transcriptomic Monocyte Dysregulation Links Systemic Autoimmunity to Chronic Graft-versus-Host Disease" (José Manuel García Pérez, University of Sevilla / International University of Andalusia, 2026).

Given the critical scarcity of monocyte-specific transcriptomic data in chronic Graft-versus-Host Disease (cGVHD), this project leverages the pathophysiological similarities between cGVHD and systemic autoimmune diseases to infer shared molecular programs. Ten public bulk RNA-seq datasets from purified monocytes across five autoimmune conditions are integrated through a two-tier approach — descriptive overlap analysis and formal precision-weighted meta-analysis — culminating in a 41-gene interferon-driven core signature. Transcriptomic findings are subsequently correlated with flow cytometry data from the BIOALO cGVHD cohort.

## Repository Structure:
- Quantification_Salmon.rmd => HPC pipeline: download, trmming, QC and Salmon quantification.
- DEA_GSExxxxxx_disease.rmd => Individual differential expression analysis for each dataset, using DESeq2. GSE149050 is used as the main example, including all the necessary explanations. Individual considerations are including in the corresponding file if needed.
- IntegrativeI_MetaAnalysis.rmd => Descriptive overlap and precision-weighted meta-analysis.
- IntegrativeII_Cytometry.rmd => Integration with cGVHD flow cytometry data using biomarker interactors from BioGRID.
- data/ => All related files (MultiQC summaries, quantification matrices, inputs and outputs) are provided.

## Pipeline Summary

### 1. Sequencing Data Processing
- Reference: Ensembl GRCh38 release 115
- Download: fasterq-dump
- Trimming: fastp
- QC: FastQC + MultiQC
- Quantification: Salmon (decoy-aware, --gcBias, --validateMappings)

### 2. Individual DEA
- Import: tximport
- Exploratory QC: VST, PCA, sample-to-sample clustering
- DEA: DESeq2 + ashr shrinkage
- Thresholds: BH-adjusted p < 0.05, FC > 1.5x
- Functional: clusterProfiler (GO, KEGG), GSEA (fgsea), pathview

### 3. Multi-Study Integration
- Descriptive overlap: ggVennDiagram, UpSetR
- Meta-analysis: MetaVolcanoR (Random Effects Model + combining approach)
- Precision-weighted logFC: inverse-variance weighting (1/SE²)
- Core Signature: meta-p < 0.01, weighted FC > 1.5x
- Functional enrichment: MetaScape

### 4. Cytometric Integration
- Exploratory phenotyping: Wilcoxon rank-sum test by time point
- Longitudinal modelling: linear mixed-effects models (lme4 + lmerTest)
- Molecular convergence: BioGRID protein-protein interaction queries
- Functional comparison: MetaScape (Core Signature vs BioGRID interactomes)

## Key Results

- A 41-gene Core Signature consistently upregulated across all five autoimmune diseases, dominated by interferon-stimulated genes under IRF1 and STAT3 transcriptional control.
- DDX58/RIG-I, the primary sensor of the type I interferon cascade, was identified as a direct interactor of CD62L (SELL) and CD11c (ITGAM), representing a candidate molecular bridge between transcriptional interferon activation and monocyte surface phenotype.

## Citation

If you use this code, please cite:

García-Pérez, JM. Core Transcriptomic Monocyte Dysregulation Links Systemic Autoimmunity to Chronic Graft-versus-Host Disease. Master's Thesis, Universidad de Sevilla / Universidad Internacional de Andalucía, 2026.

## Contact

**José Manuel García Pérez**, BSc. Biomedicina (US), MSc. Omic Data Analysis and Systems Biology (US-UNIA).

For any inquiries, please contact: www.linkedin.com/in/josemanuelgarciaperez1102
