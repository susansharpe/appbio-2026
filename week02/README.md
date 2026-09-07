# Week 02 - Tobacco mosaic virus genome data

## Selected genome

This assignment uses the complete RefSeq record for tobacco mosaic virus (TMV),
accession [NC_001367.1](https://www.ncbi.nlm.nih.gov/nuccore/NC_001367.1).
TMV is a positive-sense single-stranded RNA virus. Its complete reference
genome is one linear RNA molecule with **6,395 nucleotides**. It does not have
chromosomes in the cellular sense; for this assignment, the genome is treated
as **one genomic sequence/segment**.

The files are downloaded from NCBI's E-utilities genomic data repository:

- FASTA: `https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=nuccore&id=NC_001367.1&rettype=fasta&retmode=text`
- GFF3: `https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=nuccore&id=NC_001367.1&rettype=gff3&retmode=text`

The GFF3 contains **13 annotations/features** when comment and directive lines
are excluded: one region feature, six gene features, and six CDS features.

## Completeness assessment

The NCBI record describes this as a complete genome, and the FASTA contains
the full 6,395-nt reference sequence. The GFF3 annotates all six listed genes
and their coding sequences. I therefore consider this build **complete for a
reference genome**, while noting that it is a single reference isolate and
does not represent the sequence variation found across TMV populations.

## Reproduce the download

Requirements: `make` and `curl`. On Ubuntu or WSL, install them with:

```sh
sudo apt update
sudo apt install -y make curl
```

For a fresh Ubuntu or WSL setup, the reviewer can run:

```sh
git clone https://github.com/susansharpe/appbio-2026.git
cd appbio-2026/week02
sudo apt update
sudo apt install -y make curl
```

If the repository is already cloned, only change into its `week02` directory
and install any missing packages.

From this directory, run:

```sh
make
```

The Makefile creates `data/raw/` and downloads:

- `data/raw/NC_001367.1.fasta`
- `data/raw/NC_001367.1.gff3`

Running `make` again does not redownload files that already exist. To remove
the downloaded data and repeat the download, run:

```sh
make clean
make
```

To count the GFF3 feature rows yourself:

```sh
awk '!/^#/ && NF { n++ } END { print n }' data/raw/NC_001367.1.gff3
```
