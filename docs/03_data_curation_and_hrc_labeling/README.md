# Data Curation and High-Risk Clone Labeling

## Overview

This stage established the final curated dataset used for the genomic representation and subsequent machine learning stages.

After the initial genome collection, primary MLST assignment, and metadata acquisition, the dataset contained genomic, sequencing, and sequence-typing information for a large collection of *Klebsiella pneumoniae* genomes.

The objective of this stage was to:

1. Assess the quality and completeness of the available information.
2. Identify genomes and records requiring exclusion or further review.
3. Characterize the distribution of Sequence Types (STs).
4. Identify STs associated with recognized *K. pneumoniae* High-Risk Clones (HRCs).
5. Perform an intensive literature-based investigation of the relevant STs.
6. Progressively assign genomes to **HRC**, **No-HRC**, or **Unclassified** categories.
7. Reassess the remaining unclassified STs using additional evidence.
8. Establish the final HRC/No-HRC classification.
9. Generate the final metadata dataset while preserving the genome accession as the unique identifier.

The classification process was therefore **progressive and evidence-based**, rather than being assigned in a single step.

---

## 1. Initial Integrated Dataset

The curation stage began with the integrated dataset generated during the previous stages.

At this point, each genome was associated with information obtained from multiple sources, including:

```text
Genome accession
       +
NCBI genomic metadata
       +
Sequencing metadata
       +
Primary MLST assignment
```

The **GCF accession** was maintained as the principal identifier for each genome.

This identifier was not modified during the curation process, allowing each genome to remain traceable across all stages of the project.

---

## 2. Data Quality Assessment

Before assigning High-Risk Clone labels, the integrated dataset was subjected to a detailed quality and completeness assessment.

The objective was to identify records that could compromise the reliability of subsequent analyses.

The review considered information originating from:

- NCBI genome records
- Genome assembly information
- Sequencing metadata
- MLST results
- Accession identifiers
- ST assignments

The dataset was examined for missing, incomplete, inconsistent, or otherwise unsuitable information.

---

## 3. Genome and Metadata Curation

The initial dataset was progressively curated to establish a high-quality set of genomes suitable for downstream analysis.

The curation process included the review of:

### 3.1 Genomic information

- Genome accession
- Assembly information
- Assembly level
- Genome-related metadata

### 3.2 Sequencing information

- Availability of sequencing metadata
- Sequencing platform
- Sequencing characteristics
- Associated SRA information when available

### 3.3 MLST information

- Presence or absence of an ST assignment
- Consistency between accession and MLST information
- Identification of genomes that could not be assigned an ST

Genomes that did not meet the established criteria were considered during the curation process and removed when necessary.

The purpose of this stage was to avoid carrying low-quality or insufficiently characterized records into the subsequent HRC classification process.

---

## 4. Primary MLST-Based Dataset

Following the initial curation, the MLST information was used to characterize the genomic population.

The primary structure of the dataset at this stage was:

```text
Accession
    │
    └── ST
```

The ST assignment provided the basis for grouping genomes according to their sequence type.

This was important because the HRC classification was primarily investigated at the **ST level**, while the final label was subsequently transferred to the corresponding genome accessions.

---

## 5. Analysis of ST Distribution

The curated dataset was analyzed to determine the distribution and frequency of Sequence Types.

This analysis included:

- Total number of genomes associated with each ST.
- Relative frequency of the most represented STs.
- Identification of frequent STs.
- Identification of less represented STs.
- Identification of STs requiring further investigation.
- Identification of genomes that remained without an ST assignment.

The purpose of this analysis was not only descriptive.

The ST distribution was used to prioritize the subsequent literature review and to understand the composition of the genomic dataset before HRC classification.

---

## 6. Identification of Candidate High-Risk Clones

The identification of High-Risk Clones was performed through an extensive literature-based investigation.

The literature review focused on determining whether individual STs represented recognized or repeatedly described high-risk lineages of *Klebsiella pneumoniae*.

The investigation considered evidence describing:

- High-Risk Clones
- High-Risk Lineages
- Internationally disseminated clones
- Clinically relevant lineages
- Antimicrobial resistance-associated lineages
- ST-specific epidemiological relevance
- Repeated identification of particular STs as high-risk clones

The investigation was performed iteratively as the ST distribution was characterized.

---

## 7. HRC Evidence Synthesis

To organize the information obtained from the literature, a synthesis table was constructed for the investigated clones.

The table was used to consolidate information for each candidate lineage, including relevant evidence supporting its classification.

The conceptual structure was:

```text
ST
│
├── Clone / lineage identification
├── Evidence from literature
├── Epidemiological relevance
├── Resistance-associated evidence
└── HRC classification decision
```

This synthesis allowed the classification process to be performed consistently across the different STs.

---

## 8. Progressive Classification

The classification of the genomes was not performed as a single static operation.

Instead, labels were progressively assigned as the literature review advanced.

Three categories were initially maintained:

```text
HRC
No-HRC
Unclassified
```

### HRC

The **HRC** category included genomes belonging to STs for which the literature provided sufficient evidence to support their classification as High-Risk Clones.

### No-HRC

The **No-HRC** category included genomes belonging to STs that were not classified as High-Risk Clones according to the evidence available during the classification process.

### Unclassified

The **Unclassified** category was used for STs for which the available evidence was initially insufficient to establish a reliable HRC or No-HRC classification.

This intermediate category was important because it prevented unsupported assumptions from being introduced prematurely into the dataset.

---

## 9. Iterative Reassessment of Classification

As the literature review progressed, the distribution of the three categories was repeatedly reassessed.

The process can be represented as:

```text
Initial ST dataset
       │
       ▼
ST frequency analysis
       │
       ▼
Literature investigation
       │
       ▼
Initial classification
   ┌───┼──────────┐
   ▼   ▼          ▼
 HRC No-HRC  Unclassified
               │
               ▼
       Additional investigation
               │
               ▼
       Classification decision
```

This iterative process allowed the classification proportions to be monitored throughout the investigation.

The objective was to avoid prematurely forcing uncertain STs into one of the two final classes.

---

## 10. Investigation of Remaining Unclassified STs

After the principal HRC-associated STs had been identified, a subset of STs remained without a definitive classification.

These STs underwent an additional and more targeted investigation.

The objective was to determine whether there was sufficient evidence to consider any of them High-Risk Clones.

The investigation included an extensive search for relevant information concerning:

- ST-specific epidemiology
- High-risk lineage descriptions
- International dissemination
- Antimicrobial resistance relevance
- Clinical significance
- Previous descriptions as high-risk clones

Additional search assistance was also used during this stage to improve the breadth of the literature investigation.

---

## 11. Additional Evidence Assessment

The remaining unclassified STs were evaluated according to the evidence obtained from the additional investigation.

The decision process was conceptually:

```text
Unclassified ST
      │
      ▼
Additional evidence search
      │
      ▼
Is there relevant evidence supporting
High-Risk Clone classification?
      │
   ┌──┴──┐
   │     │
  YES    NO
   │     │
   ▼     ▼
  HRC   Evaluate remaining evidence
             │
             ▼
      Low representation /
      absence of relevant HRC evidence
             │
             ▼
          No-HRC
```

No ST was classified as HRC solely because it was frequent in the dataset.

Frequency was used as a descriptive and prioritization criterion, while HRC classification required supporting evidence.

---

## 12. Final Decision for Remaining Unclassified STs

Following the additional investigation, the remaining unclassified STs did not provide sufficient relevant evidence to justify their classification as High-Risk Clones.

Their representation within the dataset was also comparatively low.

Based on:

- the absence of relevant evidence supporting HRC status;
- the additional literature investigation; and
- their low representation within the dataset;

the remaining unclassified proportion was incorporated into the **No-HRC** category.

This decision allowed the final dataset to contain two definitive classes:

```text
HRC
No-HRC
```

Importantly, these STs were **not initially identified as No-HRC**. Their final classification resulted from the progressive evidence assessment and the final decision-making process.

---

## 13. Final HRC / No-HRC Labeling

After completion of the literature investigation and reassessment of the remaining unclassified STs, each genome was assigned one final classification:

```text
HRC
or
No-HRC
```

The classification was propagated from the ST-level evidence to the corresponding genome accessions.

Conceptually:

```text
Genome accession
       │
       ▼
      ST
       │
       ▼
Literature-based ST classification
       │
       ▼
HRC / No-HRC
```

This resulted in a genome-level classification suitable for subsequent computational analysis.

---

## 14. Construction of the Final Metadata Dataset

Once the HRC/No-HRC labels had been finalized, the classification information was incorporated into the integrated metadata.

The final dataset combined the information generated throughout the preceding stages:

```text
Genome accession
        +
NCBI metadata
        +
Sequencing metadata
        +
ST
        +
HRC / No-HRC
```

The genome accession remained unchanged and served as the primary key.

The resulting dataset therefore maintained traceability between:

```text
Original NCBI genome
        │
        ▼
Metadata
        │
        ▼
MLST
        │
        ▼
HRC classification
```

---

## 15. Final Dataset Structure

The conceptual structure of the final curated dataset was:

```text
+----------------+----------------+----------------+----------------+
| Accession      | Metadata       | ST             | Classification |
+----------------+----------------+----------------+----------------+
| GCF_xxxxx      | ...            | ST_xx          | HRC            |
| GCF_xxxxx      | ...            | ST_xx          | No-HRC         |
| GCF_xxxxx      | ...            | ST_xx          | HRC            |
| ...            | ...            | ...            | ...            |
+----------------+----------------+----------------+----------------+
```

Each row corresponded to an individual genome.

The accession was retained as the permanent identifier connecting the row to the original genome assembly.

---

## 16. Quality Control of the Final Labels

After the final classification was established, the dataset was reviewed to verify the consistency of the labels.

The final review considered:

- Presence of a valid accession.
- Presence of an ST assignment.
- Presence of a final HRC/No-HRC classification.
- Consistency between ST and assigned classification.
- Preservation of the original genome identifier.
- Absence of unresolved classification categories.

The objective was to ensure that the dataset entering the genomic representation stage was internally consistent.

---

## 17. Final Output

The main output of this stage was the **final curated and labeled metadata dataset**.

The dataset contained:

```text
Genome accession
+
Genomic metadata
+
Sequencing metadata
+
MLST
+
Final HRC/No-HRC classification
```

This dataset constituted the final biological and metadata reference used for the subsequent genomic representation stage.

---

## 18. Relationship with GenoSig

The final curated dataset generated in this stage was subsequently connected to the genomic representation produced using GenoSig.

The relationship between both stages was maintained through the genome accession:

```text
Final curated metadata
        │
        │ Accession
        ▼
Concatenated genome
        │
        ▼
GenoSig
        │
        ▼
80 genomic features
```

Therefore, the accession served as the link between the biological classification and the numerical genomic representation.

---

## 19. Overall Curation and Labeling Workflow

The complete process can be summarized as:

```text
Initial integrated dataset
            │
            ▼
     Data quality review
            │
            ▼
      Genome curation
            │
            ▼
      MLST distribution
         analysis
            │
            ▼
     ST frequency analysis
            │
            ▼
  Literature-based HRC search
            │
            ▼
   HRC / No-HRC / Unclassified
            │
            ▼
    Progressive reassessment
            │
            ▼
Additional investigation of
 remaining unclassified STs
            │
            ▼
 Evidence-based final decision
            │
            ▼
       Final HRC / No-HRC
            │
            ▼
    Final metadata dataset
            │
            ▼
        Accession-based
          integration
            │
            ▼
           GenoSig
```

---

## 20. Methodological Principles

Several principles guided the classification process.

### 20.1 Evidence-based classification

HRC status was assigned based on supporting evidence rather than solely on ST frequency.

### 20.2 Progressive classification

Classification was performed iteratively as additional information was obtained.

### 20.3 Explicit uncertainty

The **Unclassified** category was maintained during the investigation to avoid assigning unsupported labels prematurely.

### 20.4 Intensive literature investigation

The HRC classification was based on an extensive search of the available literature and additional evidence sources.

### 20.5 Accession preservation

The original genome accession was never changed, ensuring complete traceability throughout the pipeline.

### 20.6 Final binary classification

After completion of the investigation, the final dataset contained two classes:

```text
HRC
No-HRC
```

This final binary structure was required for the subsequent supervised machine learning stage.

---

## 21. Role in the Overall Project

This stage represents the transition from a large collection of publicly available *Klebsiella pneumoniae* genomes to a **curated, labeled, and traceable research dataset**.

The preceding stages established the genomic collection, MLST assignments, and metadata.

This stage established the biological classification required for the machine learning problem:

```text
Genome
   │
   ▼
ST
   │
   ▼
Literature-based evidence
   │
   ▼
HRC / No-HRC
```

The final labeled dataset was subsequently used to generate numerical genomic representations with GenoSig.

The resulting feature matrix constitutes the input for the next stage of the project:

```text
docs/04_genomic_representation_genosig/
```

which transforms each genome into a numerical representation based on dinucleotide and trinucleotide frequencies.