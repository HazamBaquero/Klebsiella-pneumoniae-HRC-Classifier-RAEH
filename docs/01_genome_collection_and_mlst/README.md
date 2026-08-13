# Genome Collection and Primary MLST Labeling

## Overview

This stage established the initial genomic dataset used throughout the project. *Klebsiella pneumoniae* genome assemblies were obtained exclusively from the **National Center for Biotechnology Information (NCBI)** and subsequently subjected to multilocus sequence typing (MLST) to obtain an initial Sequence Type (ST) assignment for each genome.

The resulting accession–ST information constituted the primary genomic labeling used in the subsequent stages of data curation, metadata integration, and High-Risk Clone (HRC) classification.

---

## 1. Computational Environment

To improve reproducibility and dependency management, independent Conda environments were used for genome retrieval and MLST analysis.

Two environments were established:

- `ncbi_datasets`: used for genome retrieval using the NCBI Datasets CLI.
- `mlst_env`: used for multilocus sequence typing.

### NCBI Datasets environment

```bash
conda create -n ncbi_datasets -c conda-forge -c bioconda ncbi-datasets-cli
conda activate ncbi_datasets
```

The installation was verified with:

```bash
datasets --version
```

The MLST analysis was performed in a separate environment:

```bash
conda activate mlst_env
```

This separation was used to reduce dependency conflicts between genome retrieval and downstream sequence-typing analyses.

---

## 2. Genome Selection and Collection

Genome assemblies were retrieved from NCBI using the following criteria:

- **Taxon:** *Klebsiella pneumoniae*
- **Assembly source:** RefSeq
- **Assembly level:** scaffold, chromosome, or complete

The NCBI Datasets CLI was used to generate a structured summary of the available assemblies:

```bash
datasets summary genome taxon "Klebsiella pneumoniae" \
--assembly-level scaffold,chromosome,complete \
--assembly-source refseq > klebsiella_summary.jsonl
```

The resulting JSON Lines (`.jsonl`) file contained structured information for the selected genome assemblies.

---

## 3. Generation of the Master Accession List

Assembly accession identifiers were extracted from the NCBI summary using `jq`:

```bash
cat klebsiella_summary.jsonl | jq -r '.accession' > klebsiella_accessions.txt
```

The number of available assemblies was then verified:

```bash
wc -l klebsiella_accessions.txt
```

The resulting dataset contained approximately **10,301 genome assemblies** at this stage.

The file:

```text
klebsiella_accessions.txt
```

was used as the master accession list for subsequent genome retrieval.

---

## 4. Division into Download Batches

To facilitate controlled genome retrieval and reduce memory and data-management problems during large-scale downloads, the master accession list was divided into batches of 1,000 accessions:

```bash
split -l 1000 klebsiella_accessions.txt batch_
```

This generated files such as:

```text
batch_aa
batch_ab
batch_ac
...
```

Each batch contained up to 1,000 accession identifiers and was processed independently.

---

## 5. Genome Download

Each accession batch was downloaded independently using the NCBI Datasets CLI.

The dedicated environment was activated:

```bash
conda activate ncbi_datasets
```

A representative batch was downloaded using:

```bash
datasets download genome accession \
--inputfile batch_aa \
--filename batch_aa.zip \
--include genome
```

This approach allowed the genome collection process to be performed in manageable and traceable batches.

---

## 6. Genome Extraction

The downloaded archive was decompressed:

```bash
unzip batch_aa.zip -d batch_aa
```

The resulting directory contained the NCBI dataset structure, including the genomic FASTA files:

```text
batch_aa/
└── ncbi_dataset/
    └── data/
        └── GCF_xxxxx/
            └── genomic.fna
```

To facilitate MLST processing, the genomic FASTA files were collected into a dedicated directory:

```bash
mkdir batch_aa_genomes
```

The `.fna` files were extracted using:

```bash
find batch_aa -name "*.fna" -exec cp {} batch_aa_genomes/ \;
```

---

## 7. Multilocus Sequence Typing (MLST)

The extracted genome assemblies were subjected to MLST using the `mlst` tool and the *Klebsiella* scheme.

The MLST environment was activated:

```bash
conda activate mlst_env
```

The genome directory was then accessed:

```bash
cd batch_aa_genomes
```

MLST was performed using:

```bash
mlst --scheme klebsiella *.fna > mlst_results_batch_aa.txt
```

The number of resulting records was verified using:

```bash
wc -l mlst_results_batch_aa.txt
```

For a complete batch, the number of MLST result lines was expected to correspond to the number of genomes processed.

---

## 8. Generation of the Accession–ST Dataset

The complete MLST output was reduced to the information required for downstream analysis: the genome accession and its assigned Sequence Type (ST).

The following command was used:

```bash
awk '{
 split($1,a,"_");
 print a[1]"_"a[2] "\t" $3
}' mlst_results_batch_aa.txt > batch_aa_accession_ST.txt
```

The resulting file followed the structure:

```text
ACCESSION    ST
GCF_000364385.3    11
GCF_000689275.1    14
```

This accession–ST table constituted the **primary MLST labeling** for the corresponding batch.

---

## 9. Storage Optimization

Because the downloaded NCBI archives and extracted genome assemblies required substantially more storage than the resulting accession–ST tables, intermediate files could be removed after successful processing.

For example:

```bash
rm -r batch_aa
rm batch_aa.zip
rm -r batch_aa_genomes
```

The lightweight accession–ST file was retained:

```text
batch_aa_accession_ST.txt
```

This reduced the storage requirements from several gigabytes of intermediate data to a small tabular representation containing the accession and ST information.

---

## 10. Primary Output

The principal output of this stage was a collection of accession–ST files generated independently for each processed batch:

```text
batch_aa_accession_ST.txt
batch_ab_accession_ST.txt
batch_ac_accession_ST.txt
...
```

These files provided the initial sequence-typing information associated with each genome accession.

The accession identifier was retained as the principal identifier to maintain traceability between the genome sequence and all subsequent metadata and classification stages.

---

## 11. Workflow Summary

The complete process can be summarized as:

```text
NCBI
  │
  ▼
Genome selection
(RefSeq + assembly level criteria)
  │
  ▼
Master accession list
  │
  ▼
Batch division
(1,000 accessions)
  │
  ▼
Controlled genome download
  │
  ▼
Genome extraction (.fna)
  │
  ▼
MLST typing
  │
  ▼
Accession–ST extraction
  │
  ▼
Primary MLST dataset
```

---

## 12. Role in the Overall Project

The accession–ST dataset generated in this stage was subsequently used as an input for the broader data curation process.

The initial ST assignments enabled:

1. Assessment of the distribution and frequency of Sequence Types.
2. Identification of STs requiring further investigation.
3. Integration of MLST information with genomic and sequencing metadata.
4. Subsequent literature-based classification of genomes into High-Risk Clone (HRC) and Non-High-Risk Clone (No-HRC) categories.

The subsequent curation and HRC-labeling methodology will be documented separately in:

```text
docs/03_data_curation_and_hrc_labeling/
```