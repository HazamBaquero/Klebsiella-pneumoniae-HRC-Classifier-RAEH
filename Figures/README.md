# Results

This directory contains the main results generated throughout the development of the *Klebsiella pneumoniae* High-Risk Clone (HRC) classification project.

The results are organized according to the main stages of the project, from genome and metadata characterization to dataset curation, HRC labeling, genomic representation, and machine learning.

> **Data availability note:**  
> Due to project data management considerations, genomic sequence files and individual genomic representations generated during the analysis are not included in this repository. Only selected figures, tables, scripts, workflows, and documentation are provided to describe and reproduce the analytical methodology.

---

## 1. Genome and Metadata Characterization

The initial dataset consisted of approximately 10,000 *Klebsiella pneumoniae* genomes obtained exclusively from the National Center for Biotechnology Information (NCBI).

For each genome, metadata associated with the NCBI record and sequencing information were collected. These data were subsequently analyzed to characterize the genomic dataset and evaluate its overall quality.

### 1.1 Genome Completeness

The distribution of genome completeness across the collected genomes was evaluated as part of the initial quality assessment.

![Genome completeness distribution](/figures/genome_completeness_distribution.png)

**Figure 1. Genome completeness distribution.**

The histogram shows the global distribution of genome completeness values across the collected *K. pneumoniae* genomes. The distribution is concentrated toward high completeness values, indicating that the dataset predominantly consisted of high-quality genome assemblies.

---

### 1.2 Genome Contamination

Genome contamination was also evaluated as part of the quality assessment.

![Genome contamination distribution](01_metadata/figures/genome_contamination_distribution.png)

**Figure 2. Genome contamination distribution.**

The histogram represents the global distribution of contamination values across the genome dataset. Most genomes showed low contamination values, supporting the suitability of the dataset for subsequent genomic analyses.

---

## 2. Sequence Type Distribution

Multilocus sequence typing (MLST) was initially performed to assign a Sequence Type (ST) to each genome whenever sufficient information was available.

The distribution of STs was subsequently analyzed to characterize the population structure of the dataset and identify the most frequently represented lineages.

### 2.1 Most Frequent Sequence Types

The 20 most frequently represented STs were identified based on the number of genomes assigned to each sequence type.

![Top 20 Sequence Types - vertical](01_metadata/figures/top20_sequence_types_vertical.png)

**Figure 3. Top 20 Sequence Types according to the number of genomes.**

The distribution shows substantial differences in the number of genomes represented by individual STs, with a limited number of STs accounting for a large proportion of the dataset.

---

### 2.2 Top 20 Sequence Types

An alternative horizontal representation of the same distribution is provided below.

![Top 20 Sequence Types - horizontal](01_metadata/figures/top20_sequence_types_horizontal.png)

**Figure 4. Top 20 Sequence Types according to genome frequency.**

The horizontal representation facilitates comparison between the most frequently observed STs and highlights the predominance of specific sequence types within the dataset.

---

### 2.3 Low-Frequency Sequence Types

The distribution of STs represented by between one and ten genomes was also evaluated.

![Low-frequency Sequence Types](01_metadata/figures/low_frequency_sequence_types.png)

**Figure 5. Number of Sequence Types according to low-frequency genome representation.**

A large number of STs were represented by relatively few genomes. This observation was relevant for the subsequent analysis because the identification of High-Risk Clones could not rely exclusively on genome frequency and required an extensive bibliographic assessment.

---

# 3. Dataset Curation and HRC Labeling

Following the initial MLST assignment, an extensive data curation process was performed.

The curation incorporated information from NCBI metadata, sequencing information, genome quality indicators, and the availability of MLST assignments. Genomes that did not meet the established criteria were excluded from the final analytical dataset.

After curation, the distribution of Sequence Types was analyzed in greater detail. The most frequently represented STs were evaluated alongside the scientific literature to identify *K. pneumoniae* lineages recognized as High-Risk Clones (HRCs).

The classification process was progressive and was updated as the bibliographic evidence for individual STs was reviewed.

---

## 3.1 Progressive HRC Classification

During the bibliographic classification process, genomes were progressively assigned to three categories:

- **HRC**
- **No-HRC**
- **Unclassified**

![Progressive HRC labeling](03_hrc_labeling/figures/progressive_hrc_labeling.png)

**Figure 6. Progressive distribution of HRC, No-HRC, and unclassified genomes during the classification process.**

At this stage, 6,062 genomes (62.0%) had been classified as HRC, 819 genomes (8.4%) as No-HRC, and 2,895 genomes (29.6%) remained unclassified.

The unclassified category represented sequence types for which sufficient evidence had not yet been established to assign them confidently to either category.

---

## 3.2 Final HRC Classification

The classification process was subsequently completed through an intensive bibliographic review.

The remaining unclassified sequence types were evaluated through an extensive search of the available scientific literature. Additional searches were conducted with assistance from artificial intelligence-based literature exploration tools to identify relevant evidence concerning their potential classification as High-Risk Clones.

For the remaining sequence types, no relevant evidence supporting their classification as HRCs was identified, and their relatively low number of available genome sequences was also considered.

Consequently, the remaining unclassified genomes were incorporated into the No-HRC category.

![Final HRC and No-HRC distribution](03_hrc_labeling/figures/final_hrc_nohrc_distribution.png)

**Figure 7. Final distribution of HRC and No-HRC genomes.**

The final dataset consisted of:

- **6,062 HRC genomes (62.01%)**
- **3,714 No-HRC genomes (37.99%)**

This resulted in a final two-class classification framework consisting of HRC and No-HRC genomes.

---

# 4. Genomic Representation

Following completion of the genomic labeling process, a final metadata dataset was constructed.

The final metadata preserved the accession identifier of each genome throughout the analytical workflow and integrated the information collected during genome acquisition, metadata characterization, quality assessment, MLST assignment, curation, and HRC classification.

Subsequently, the genomes were processed using **GenoSig** to obtain numerical genomic representations suitable for machine learning.

The genomic representation was generated through nucleotide-frequency analysis based on:

- Dinucleotide frequencies
- Trinucleotide frequencies

The resulting representations constitute the input space for the subsequent machine learning stage.

> Genomic sequence files and individual GenoSig representations are not included in this repository.

Future results related to genomic representations will be added to:

```text
04_genomic_representation/
