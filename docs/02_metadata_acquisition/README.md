# Genomic and Sequencing Metadata Acquisition

## Overview

This stage focused on the acquisition, processing, and integration of genomic and sequencing metadata associated with the *Klebsiella pneumoniae* genome assemblies collected from the National Center for Biotechnology Information (NCBI).

The metadata workflow combined information obtained from:

- **NCBI genome assembly records**
- **Sequence Read Archive (SRA)** records associated with the assemblies
- **Primary MLST results** generated during the previous stage

The **genome accession (GCF)** was maintained as the primary identifier throughout the process, allowing the different information sources to be integrated while preserving traceability for each genome.

---

## 1. Computational Environment

A dedicated Conda environment was used for metadata acquisition and processing.

The environment included tools required for querying NCBI and SRA databases and manipulating structured metadata files.

### Environment creation

```bash
conda create -n ncbi_datasets -y
conda activate ncbi_datasets
```

### Required tools

The following tools were installed:

```bash
conda install -c conda-forge jq -y
conda install -c bioconda entrez-direct -y
conda install -c conda-forge coreutils -y
```

The tools were used for:

- `datasets` — retrieval of structured NCBI metadata.
- `jq` — extraction and transformation of JSON Lines metadata.
- `entrez-direct` — querying the SRA database.
- `coreutils` — command-line manipulation and processing of tabular files.

---

## 2. NCBI Genomic Metadata Acquisition

The genomic metadata were obtained from NCBI using the accession lists generated during the genome collection stage.

Each previously defined genome batch was processed independently.

For a given batch, the NCBI Datasets CLI was used to retrieve structured metadata in JSON Lines (`.jsonl`) format:

```bash
datasets summary genome accession \
--inputfile lote_ax_accessions.txt \
--as-json-lines > lote_ax_metadata_raw.jsonl
```

Where:

```text
lote_ax_accessions.txt
```

contains the genome accession identifiers, and:

```text
lote_ax_metadata_raw.jsonl
```

contains the raw metadata retrieved from NCBI.

Processing the batches independently maintained traceability and reduced the risk of conflicts during large-scale metadata handling.

---

## 3. Extraction of Relevant Genomic Metadata

The raw JSON Lines metadata were processed using `jq` to extract variables relevant to the genomic assemblies and their associated isolates.

The following fields were extracted:

- Genome accession
- Assembly method
- Assembly level
- Genome coverage
- GC percentage
- Organism name
- Host
- Geographic location
- Isolation source
- Collection date

The extraction was performed using:

```bash
jq -r '
[
  .accession,
  .assembly_info.assembly_method,
  .assembly_info.assembly_level,
  .assembly_stats.genome_coverage,
  .assembly_stats.gc_percent,
  .organism.organism_name,
  .assembly_info.biosample.host,
  .assembly_info.biosample.geo_loc_name,
  .assembly_info.biosample.isolation_source,
  .assembly_info.biosample.collection_date
] | @tsv
' lote_ax_metadata_raw.jsonl > lote_ax_metadata.tsv
```

The resulting file:

```text
lote_ax_metadata.tsv
```

contained the selected genomic and isolate metadata in tab-separated format.

---

## 4. Recovery of SRA Identifiers

To obtain additional information about the sequencing process, the SRA identifiers associated with each genome assembly were extracted from the NCBI metadata.

The accession and corresponding SRA identifier were obtained using:

```bash
jq -r '
[
  .accession,
  (.assembly_info.biosample.sample_ids[]?
  | select(.db=="SRA")
  | .value)
] | @tsv
' lote_ax_metadata_raw.jsonl > lote_ax_accession_sra.tsv
```

The resulting file:

```text
lote_ax_accession_sra.tsv
```

linked each genome accession to its corresponding SRA identifier when available.

---

## 5. Retrieval of Sequencing Metadata from SRA

The SRA identifiers were subsequently used to query the Sequence Read Archive and retrieve sequencing-related information.

First, the SRA identifiers were extracted:

```bash
cut -f2 lote_ax_accession_sra.tsv > lote_ax_ers.txt
```

The SRA database was then queried using Entrez Direct:

```bash
while read ers; do
    esearch -db sra -query $ers | \
    efetch -format runinfo | \
    cut -d',' -f1,5,10,15 | \
    tail -n +2
done < lote_ax_ers.txt > lote_ax_sra_runinfo.tsv
```

The sequencing metadata included information such as:

- **Run** (`SRR`)
- **Platform**
- **Sequencing instrument/model**
- **Library layout**

Examples of sequencing platforms represented in the metadata include:

```text
ILLUMINA
PACBIO
OXFORD_NANOPORE
```

Examples of instrument models include:

```text
HiSeq
MiSeq
Sequel
```

The library layout was represented as:

```text
PAIRED
SINGLE
```

---

## 6. Metadata Integration

The different metadata sources were integrated using the genome accession as the primary key.

Before performing the joins, the corresponding files were sorted by accession:

```bash
sort -k1,1 lote_ax_metadata.tsv > lote_ax_metadata_sorted.tsv
```

```bash
sort -k1,1 lote_ax_accession_sra.tsv > lote_ax_accession_sra_sorted.tsv
```

The genomic metadata and SRA identifiers were then integrated using:

```bash
join -t $'\t' \
lote_ax_metadata_sorted.tsv \
lote_ax_accession_sra_sorted.tsv \
> lote_ax_metadata_sra.tsv
```

This generated an intermediate dataset containing the genomic metadata together with the associated SRA information.

---

## 7. Integration of MLST Results

The MLST results generated during the previous stage were incorporated into the metadata dataset.

The MLST file was first sorted according to the genome accession:

```bash
sort -k1,1 lote_ax_mlst_clean.tsv > lote_ax_mlst_sorted.tsv
```

The genomic, sequencing, and MLST information were then integrated:

```bash
join -t $'\t' \
lote_ax_metadata_sra.tsv \
lote_ax_mlst_sorted.tsv \
> lote_ax_metadata_final.tsv
```

The resulting file:

```text
lote_ax_metadata_final.tsv
```

represented the integrated metadata for the corresponding genome batch.

---

## 8. Primary Identifier and Traceability

The **GCF genome accession** was maintained as the primary identifier throughout the entire metadata integration process.

This allowed information from different sources to remain associated with the same genome:

```text
Genome Assembly
      │
      │ GCF accession
      ▼
NCBI Metadata
      │
      │ GCF accession
      ▼
SRA Information
      │
      │ GCF accession
      ▼
MLST
      │
      ▼
Integrated Metadata
```

Maintaining the accession as a constant identifier was essential for preserving traceability between the original genome assembly and all subsequent analytical stages.

---

## 9. Batch-Based Processing

Each genome batch was processed independently throughout the metadata workflow.

For each batch, the general process was:

```text
Accession list
      │
      ▼
NCBI metadata retrieval
      │
      ▼
Relevant metadata extraction
      │
      ▼
SRA identifier recovery
      │
      ▼
SRA sequencing metadata retrieval
      │
      ▼
Metadata integration
      │
      ▼
MLST integration
      │
      ▼
Final batch metadata
```

The batch-based strategy maintained consistency with the genome collection workflow and facilitated traceability during large-scale processing.

---

## 10. Final Output

For each processed batch, the final integrated metadata file was generated as:

```text
lote_ax_metadata_final.tsv
```

The resulting dataset contained information from multiple sources, including:

### Genomic assembly information

- Accession
- Assembly method
- Assembly level
- Genome coverage
- GC percentage

### Isolate metadata

- Organism
- Host
- Geographic location
- Isolation source
- Collection date

### Sequencing information

- SRA identifier
- Run (`SRR`)
- Sequencing platform
- Instrument/model
- Library layout

### MLST information

- Sequence Type (ST)

---

## 11. Methodological Considerations

Several considerations were maintained throughout the integration process.

### Independent batch processing

Each genome batch was processed independently to maintain traceability and avoid conflicts during large-scale data manipulation.

### Accession-based integration

The GCF accession was used as the primary key for joining information from NCBI, SRA, and MLST.

### Sequencing metadata source

Sequencing-specific information was obtained from SRA because detailed sequencing characteristics were not consistently available in the genome assembly records.

### Multiple sequencing runs

In cases where multiple sequencing runs (`SRR`) were associated with a single genome assembly, the resulting relationships required consideration during subsequent data analysis and consolidation.

---

## 12. Role in the Overall Project

The integrated metadata generated during this stage provided the information required for the subsequent data curation and classification process.

At this point, each genome could be associated through its accession with:

```text
Genomic information
        +
Sequencing information
        +
MLST information
```

This integrated dataset served as the basis for the subsequent analysis of genome quality, ST distributions, metadata completeness, and the literature-based classification of genomes into **High-Risk Clone (HRC)** and **Non-High-Risk Clone (No-HRC)** categories.

The subsequent curation and HRC-labeling methodology will be documented separately in:

```text
docs/03_data_curation_and_hrc_labeling/
```