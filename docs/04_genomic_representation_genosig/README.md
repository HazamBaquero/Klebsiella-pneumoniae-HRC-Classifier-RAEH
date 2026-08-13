# Genomic Representation Using GenoSig

## Overview

This stage transformed the curated *Klebsiella pneumoniae* genome assemblies into numerical genomic representations suitable for subsequent machine learning analysis.

The **GenoSig** software was used to generate genomic signatures based on dinucleotide (Di) and trinucleotide (Tri) frequencies using the **AltKarlinSignature** representation.

The final representation consists of:

- **16 dinucleotide features (Di)**
- **64 trinucleotide features (Tri)**
- **80 genomic features per genome**

The original genome accession was preserved as the unique identifier throughout the process.

The resulting feature matrix will be used in the subsequent machine learning stage for the classification of:

```text
High-Risk Clone (HRC)
vs.
Non-High-Risk Clone (No-HRC)
```

---

## 1. Computational Environment

A dedicated Conda environment was created for GenoSig and its associated dependencies.

```bash
conda create -n genosig_env python=3.10 -y
conda activate genosig_env
```

The environment was used specifically for the genomic representation stage.

---

## 2. GenoSig Repository

The GenoSig software was obtained from its source repository and organized within the project workspace.

The project directory was created using:

```bash
mkdir -p ~/proyecto_klebsiella
cd ~/proyecto_klebsiella
```

The GenoSig repository was then downloaded or cloned into the project directory.

The working structure used during the analysis was:

```text
/home/raeh/proyecto_klebsiella/genosig/
```

The GenoSig executable was located at:

```text
/home/raeh/proyecto_klebsiella/genosig/GenoSig/genosig/
```

---

## 3. Verification of GenoSig

The GenoSig executable was verified before processing the complete dataset.

The executable was accessed using:

```bash
cd ~/proyecto_klebsiella/genosig/GenoSig/genosig
```

The available commands were checked with:

```bash
./All/genosig/bin/genosig
```

A successful installation returned the GenoSig usage information:

```text
Usage: ./All/genosig/bin/genosig <command> [options]
```

---

## 4. Input Genome Assemblies

The genome assemblies used in this stage were previously obtained from **NCBI** during the genome collection stage.

The assemblies were stored in batch-specific directories, for example:

```text
/home/raeh/proyecto_klebsiella/genomes/lote_aa_genomes/
```

Individual genome files followed the NCBI FASTA naming convention:

```text
GCF_000364385.3_ASM36438v3_genomic.fna
GCF_000689275.1_AFX_PRJEB4159_v1_genomic.fna
...
```

Each assembly could contain multiple sequences representing different genomic components, including:

```text
Chromosome
Plasmid 1
Plasmid 2
...
```

---

## 5. Initial GenoSig Assessment

An initial test was performed by applying GenoSig directly to the original genome assemblies.

A representative command was:

```bash
./All/genosig/bin/genosig generate contigs \
-i archivo.fna \
-m 100 \
-r \
-t karl 2
```

When applied directly to assemblies containing multiple contigs, GenoSig generated a separate representation for each contig.

For example:

```text
chromosome
plasmid1
plasmid2
plasmid3
```

This behavior was not suitable for the intended machine learning problem because the biological unit of interest was:

```text
1 genome = 1 sample
```

rather than:

```text
1 contig = 1 sample
```

---

## 6. Genome Concatenation Strategy

To ensure that each genome was represented as a single biological unit, all contigs and plasmids belonging to each assembly were concatenated into a single nucleotide sequence.

### Before concatenation

```text
>chromosome
ATGC...

>plasmid1
ATGC...

>plasmid2
ATGC...
```

### After concatenation

```text
>GCF_000364385.3
ATGCATGCATGCATGC...
```

This transformation ensured that one FASTA file corresponded to one genome accession and therefore to one sample in the subsequent machine learning dataset.

---

## 7. Creation of Concatenated FASTA Files

A dedicated directory was created for the concatenated genomes:

```bash
mkdir -p \
/home/raeh/proyecto_klebsiella/genomes/lote_aa_concatenados
```

A Python script was used to concatenate the sequences contained within each genome assembly.

The script was saved as:

```text
concatenar_genomas.py
```

The implementation was:

```python
from pathlib import Path

entrada = Path(
    "/home/raeh/proyecto_klebsiella/genomes/lote_aa_genomes"
)

salida = Path(
    "/home/raeh/proyecto_klebsiella/genomes/lote_aa_concatenados"
)

salida.mkdir(exist_ok=True)

for archivo in entrada.glob("*.fna"):

    accession = archivo.name.split("_ASM")[0]

    secuencia = []

    with open(archivo) as f:
        for linea in f:
            if linea.startswith(">"):
                continue
            secuencia.append(linea.strip())

    secuencia_final = "".join(secuencia)

    archivo_salida = salida / f"{accession}.fasta"

    with open(archivo_salida, "w") as out:
        out.write(f">{accession}\n")
        out.write(secuencia_final + "\n")

print("Proceso terminado")
```

The script was executed using:

```bash
python concatenar_genomas.py
```

---

## 8. Verification of Concatenated Genomes

The resulting FASTA files were inspected to verify that the accession was correctly preserved and that each file contained a single sequence.

For example:

```bash
head -2 \
/home/raeh/proyecto_klebsiella/genomes/lote_aa_concatenados/GCF_000364385.3.fasta
```

Expected output:

```text
>GCF_000364385.3
ATGCGATCGATCGATCGATCG...
```

The number of FASTA headers was also checked:

```bash
grep ">" \
/home/raeh/proyecto_klebsiella/genomes/lote_aa_concatenados/GCF_000364385.3.fasta
```

Expected output:

```text
>GCF_000364385.3
```

This confirmed that the concatenated file contained a single sequence representing the complete genome assembly.

---

## 9. Dinucleotide Representation

The concatenated FASTA files were used as input for GenoSig to generate dinucleotide genomic signatures.

A representative test genome was copied to the GenoSig working directory:

```bash
cp \
/home/raeh/proyecto_klebsiella/genomes/lote_aa_concatenados/GCF_000364385.3.fasta \
/home/raeh/proyecto_klebsiella/genosig/GenoSig/genosig/All/
```

The dinucleotide representation was generated using:

```bash
./All/genosig/bin/genosig generate contigs \
-i All/GCF_000364385.3.fasta \
-m 100 \
-r \
-t karl 2 > prueba_di.csv
```

For `k = 2`, the resulting representation contains:

```text
16 dinucleotide features
```

corresponding to the possible nucleotide pairs:

```text
AA
AC
AG
AT
CA
CC
CG
CT
GA
GC
GG
GT
TA
TC
TG
TT
```

---

## 10. Trinucleotide Representation

The same concatenated genome was then processed to generate the trinucleotide representation.

The command used was:

```bash
./All/genosig/bin/genosig generate contigs \
-i All/GCF_000364385.3.fasta \
-m 100 \
-r \
-t karl 3 > prueba_tri.csv
```

For `k = 3`, the resulting representation contains:

```text
64 trinucleotide features
```

representing all possible combinations of three nucleotides:

```text
AAA
AAC
AAG
...
TTT
```

---

## 11. AltKarlinSignature Representation

The genomic representation generated by GenoSig corresponds to the **AltKarlinSignature** approach.

This representation is based on deviations between observed and expected frequencies of k-mers.

For:

```text
k = 2
```

the representation contains:

```text
4² = 16 features
```

For:

```text
k = 3
```

the representation contains:

```text
4³ = 64 features
```

Therefore, the combined genomic representation contains:

```text
16 + 64 = 80 features
```

per genome.

---

## 12. Final Genomic Feature Representation

Each genome was transformed into a numerical vector containing:

```text
16 dinucleotide features
+
64 trinucleotide features
=
80 genomic features
```

The conceptual structure of the resulting matrix is:

```text
accession    AA    AC    ...    TTT
GCF_000364385.3    ...    ...    ...    ...
GCF_000689275.1    ...    ...    ...    ...
...
```

The **accession** was retained as the unique identifier for each genome.

This was essential to maintain the connection between the genomic representation and the curated metadata containing the HRC/No-HRC classification.

---

## 13. Integration with Classification Labels

The genomic feature matrix will subsequently be integrated with the final classification labels obtained during the data curation stage.

The conceptual structure of the final machine learning dataset is:

```text
accession    AA    AC    ...    TTT    class
GCF_000364385.3    ...    ...    ...    ...    HRC
GCF_000689275.1    ...    ...    ...    ...    No-HRC
```

where:

- `accession` identifies the genome;
- `AA` through `TTT` represent the 80 genomic features;
- `class` represents the final HRC classification.

The classification labels are generated during the preceding curation stage and are therefore not assigned by GenoSig.

---

## 14. Output

The principal output of this stage is a numerical genomic representation in which each genome is represented by:

```text
80 genomic features
```

composed of:

```text
16 Di features
64 Tri features
```

The accession identifier is preserved for every genome.

The resulting representation constitutes the **feature space for the subsequent machine learning stage**.

---

## 15. Workflow Summary

The complete genomic representation workflow can be summarized as:

```text
Curated NCBI genome assemblies
              │
              ▼
      Original FASTA files
              │
              ▼
       Multiple contigs
       per genome assembly
              │
              ▼
       Genome concatenation
              │
              ▼
     One FASTA = one genome
              │
              ▼
          GenoSig
              │
       ┌──────┴──────┐
       ▼             ▼
   k = 2          k = 3
       │             │
       ▼             ▼
   16 Di          64 Tri
       │             │
       └──────┬──────┘
              ▼
      80 genomic features
              │
              ▼
      Accession-based
       feature matrix
              │
              ▼
     Machine Learning
```

---

## 16. Role in the Overall Project

This stage represents the transition from **genomic sequence data to a numerical feature space**.

The preceding stages generated and curated the genome dataset and established the final HRC/No-HRC labels. GenoSig then transformed each genome into a standardized numerical representation based on its nucleotide composition.

The resulting feature matrix is intended to serve as the input for the subsequent machine learning stage, where models will be developed to classify *Klebsiella pneumoniae* genomes according to their High-Risk Clone status.

The machine learning methodology will be documented separately in:

```text
docs/05_machine_learning/
```

---

## 17. Reproducibility Notes

The following elements should be preserved when reproducing this stage:

- The original NCBI accession identifiers.
- The genome assemblies used as input.
- The concatenation procedure.
- The GenoSig version and executable used.
- The parameters applied during GenoSig execution.
- The distinction between dinucleotide (`k = 2`) and trinucleotide (`k = 3`) representations.
- The accession identifier used to link the genomic representation to the final metadata.

The accession identifier must remain unchanged throughout the pipeline to preserve traceability between the original genome, its metadata, its HRC/No-HRC label, and its numerical genomic representation.