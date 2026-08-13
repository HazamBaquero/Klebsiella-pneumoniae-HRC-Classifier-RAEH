# Klebsiella pneumoniae HRC Classifier – RAEH

A reproducible bioinformatics and machine learning framework for the identification of High-Risk Clones (HRC) of *Klebsiella pneumoniae* from whole-genome sequences using genomic compositional signatures.

Developed by the **Antimicrobial Resistance and Hospital Epidemiology Research Group (RAEH)** and the **Laboratory for Advanced Solutions in Virtualization and Artificial Intelligence (SavIA-Lab)**, **Universidad El Bosque**, Bogotá, Colombia.

---

## Background

Antimicrobial resistance (AMR) is recognized as one of the greatest global public health threats, limiting the effectiveness of available antimicrobial therapies and increasing morbidity, mortality, and healthcare costs worldwide.

Among the priority bacterial pathogens, *Klebsiella pneumoniae* has emerged as one of the most clinically relevant species due to its remarkable capacity to acquire antimicrobial resistance determinants and disseminate internationally through successful epidemic lineages known as **High-Risk Clones (HRCs)**.

Early identification of these clones is essential for genomic surveillance, infection prevention, epidemiological investigations, and antimicrobial stewardship. However, most current identification strategies rely primarily on sequence typing and extensive genomic analyses, which may become computationally demanding for large-scale genomic datasets.

This project aims to develop a reproducible computational workflow capable of transforming whole-genome sequences into genomic compositional representations suitable for machine learning, enabling the future identification of High-Risk Clones of *Klebsiella pneumoniae*.

---

## Objectives

### General Objective

Develop a reproducible computational workflow for the identification of High-Risk Clones (HRCs) of *Klebsiella pneumoniae* from whole-genome sequences using genomic compositional signatures and machine learning.

### Specific Objectives

- Collect and curate publicly available *Klebsiella pneumoniae* genomes and associated metadata.
- Perform sequence typing using the MLST scheme.
- Apply rigorous quality control and metadata curation.
- Identify High-Risk Clones through an extensive literature review.
- Construct a curated metadata dataset with HRC and non-HRC labels.
- Generate genomic compositional representations using GenoSig.
- Develop and evaluate machine learning models for HRC classification.
- ---

## Project Overview

The **Klebsiella pneumoniae HRC Classifier – RAEH** project aims to establish a reproducible computational workflow for identifying High-Risk Clones (HRCs) of *Klebsiella pneumoniae* from whole-genome sequencing data.

Unlike conventional approaches that focus exclusively on machine learning model development, this project encompasses the complete data generation pipeline, including genome collection, metadata acquisition, quality assessment, sequence typing, literature-based HRC classification, metadata curation, genomic representation, and the subsequent development of predictive machine learning models.

The workflow has been designed to ensure reproducibility, transparency, and scalability, allowing every processing stage to be independently documented, reproduced, and improved.

Currently, the project has completed the construction of a curated genomic dataset and the generation of genomic compositional representations using GenoSig. These representations constitute the input features for the next stage of the project, which consists of developing and evaluating machine learning models for High-Risk Clone classification.


---

## Methodological Workflow

The project follows a sequential workflow composed of the following stages:

1. **Genome Collection**
   - Retrieval of approximately 10,000 publicly available *Klebsiella pneumoniae* genomes from the NCBI database.

2. **Metadata Acquisition**
   - Collection of genome-associated metadata and sequencing information available from NCBI.

3. **MLST Classification**
   - Initial sequence typing of every genome using the MLST scheme to assign Sequence Types (STs).

4. **Data Quality Assessment and Curation**
   - Evaluation of metadata quality, sequencing information, and genomes lacking MLST assignments to generate a high-quality dataset.

5. **High-Risk Clone Identification**
   - Comprehensive literature review to identify internationally recognized High-Risk Clones.
   - Progressive HRC labeling based on current scientific evidence.

6. **Metadata Construction**
   - Integration of all curated information into a unified metadata table using genome accession identifiers.

7. **Genomic Representation**
   - Genome transformation into di- and trinucleotide compositional signatures using GenoSig.

8. **Machine Learning Development (ongoing)**
   - Development and evaluation of predictive models for HRC classification.



---

## Repository Structure

```text
Klebsiella-pneumoniae-HRC-Classifier-RAEH/
│
├── docs/
├── data/
├── workflow/
├── scripts/
├── results/
├── environment/
│
├── README.md
├── LICENSE
└── .gitignore
```

### Main directories

| Directory | Description |
|-----------|-------------|
| `data/` | Raw, intermediate and processed datasets. |
| `workflow/` | Scientific workflow organized by project stages. |
| `scripts/` | General-purpose scripts used throughout the project. |
| `results/` | Figures, tables and trained models. |
| `docs/` | Project documentation and methodological notes. |
| `environment/` | Computational environment configuration files. |




---

## Current Project Status

| Stage | Status |
|-------|--------|
| Genome Collection | ✅ Completed |
| Metadata Acquisition | ✅ Completed |
| MLST Classification | ✅ Completed |
| Data Curation | ✅ Completed |
| HRC Literature Review | ✅ Completed |
| HRC Labeling | ✅ Completed |
| Final Metadata Construction | ✅ Completed |
| GenoSig Genomic Representation | 🚧 In Progress |
| Machine Learning Model Development | 🚧 In Progress |
| Model Evaluation | ⏳ Pending |
| External Validation | ⏳ Pending |




---

## Future Work

The next stages of the project include:

- Development of supervised machine learning models.
- Model optimization and hyperparameter tuning.
- Internal and external performance evaluation.
- Biological interpretation of predictive features.
- Development of an accessible classification tool.
- Dissemination of the workflow through scientific publications and open-source software.




---

## Citation

If you use this repository, please cite it as:

> Nom, et al. *Klebsiella pneumoniae HRC Classifier – RAEH*. GitHub repository. (Work in progress)




---

## Authors

**Juan Carlos García Betancur, Ph.D.**  
Teaching-researcher  
Antimicrobial Resistance and Hospital Epidemiology Research Group (RAEH)  
Universidad El Bosque



---

## License

This project is distributed under the MIT License.

See the `LICENSE` file for more information.