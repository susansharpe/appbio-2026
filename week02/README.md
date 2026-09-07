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

This project requires **`make`** and **`curl`**. On Ubuntu or WSL, install them with:

```sh
sudo apt update
sudo apt install -y make curl
```

To reproduce the data download from a new copy of this repository:

```sh
git clone https://github.com/susansharpe/appbio-2026.git
cd appbio-2026/week02
make
```

If the repository is already cloned, navigate to the `week02` directory and run:

```sh
make
```

The Makefile automatically creates the `data/raw/` directory and downloads these two files:

- `data/raw/NC_001367.1.fasta`
- `data/raw/NC_001367.1.gff3`

The downloaded files are generated data and are intentionally not stored in the Git repository. They can be verified with:

```sh
ls -lh data/raw/
```

Running `make` again will not redownload files that already exist.

To remove the downloaded files and reproduce the download from scratch, run:

```sh
make clean
make
```

To count the annotation rows in the GFF3 file, excluding comments and directive lines, run:

```sh
awk '!/^#/ && NF { n++ } END { print n }' data/raw/NC_001367.1.gff3
```
