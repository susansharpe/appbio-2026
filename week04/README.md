# Week 4: Downloading FASTQ Data

## Assessing experimental evidence for Tobacco mosaic virus

For my previous assignment, I selected the Tobacco mosaic virus (TMV) reference genome, accession `NC_001367.1`.

### How popular is this genome?

I searched the NCBI Sequence Read Archive (SRA) for **Tobacco mosaic virus (TMV)** and obtained **162 search results**. However, I do not interpret all 162 results as independent TMV sequencing datasets.

Of the 162 results returned by my search, **16 listed Homo sapiens as the organism**. When I examined these records, I did not find an obvious description indicating that they represented TMV sequencing. Therefore, I consider 162 to be the number of raw SRA search hits rather than the number of confirmed TMV-specific datasets.

Many of the biologically relevant results were not sequencing purified TMV alone. Instead, they involved sequencing a host plant infected with TMV. These experiments can be used to study the response of the host to infection as well as viral RNA present in infected plant tissue.

### Sequencing platform breakdown

Of the 162 SRA search results:

* **152** used Illumina sequencing.
* **10** used Oxford Nanopore sequencing.

Illumina therefore represented the large majority of records returned by the search. These counts describe the raw SRA search results and not a manually curated collection of confirmed TMV-specific experiments.

### What I found interesting or surprising

I was surprised that searching for Tobacco mosaic virus did not simply return datasets containing isolated TMV genome sequencing. Many of the experiments instead involved sequencing plants infected with TMV.

I was also surprised that some results listed Homo sapiens as the organism even though I had searched for TMV. This demonstrated that a keyword search in SRA can return records that are not necessarily direct sequencing experiments of the organism being searched. The experiment and sample metadata therefore need to be examined before deciding whether a result is relevant.

## Selected SRA experiment

For the FASTQ analysis, I selected the following RNA-Seq experiment involving a TMV-infected plant:

* Experiment: `SRX33095684`
* Run: `SRR38258968`
* Description: RNA-Seq of *Nicotiana benthamiana* infected with TMV

The run contains paired-end reads.

## Downloading a subset of the sequencing reads

The `Makefile` allows the accession number and number of SRA spots to be changed from the command line.

For this analysis, I used:

```bash
make SRR=SRR38258968 N=10000 NAME=TMV_infected_benthamiana
```

The `N` parameter was set to 10,000. Because this run is paired-end, downloading the first 10,000 SRA spots produced 10,000 R1 sequences and 10,000 R2 sequences.

The raw reads were renamed to more descriptive filenames:

```text
fastq/TMV_infected_benthamiana_R1.fastq
fastq/TMV_infected_benthamiana_R2.fastq
```

This naming scheme is easier to interpret than using only the SRR accession number.

## File organization

Generated files are organized according to their type and purpose:

```text
week04/
├── Makefile
├── README.md
├── fastq/
│   ├── TMV_infected_benthamiana_R1.fastq
│   └── TMV_infected_benthamiana_R2.fastq
├── trimmed/
│   ├── TMV_infected_benthamiana_R1.trimmed.fastq
│   └── TMV_infected_benthamiana_R2.trimmed.fastq
└── qc/
    ├── raw/
    ├── trimmed/
    └── fastp/
```

The generated FASTQ, trimmed, and QC directories are excluded from Git because they can be regenerated using the Makefile.

## Quality control

I used **FastQC** to visualize read quality before trimming.

I then used **fastp** to perform quality filtering, tail trimming, adapter detection, and minimum-length filtering. FastQC was run again on the trimmed reads so that the raw and processed data could be compared.

### fastp results

The original paired-end files contained:

* R1: 10,000 reads
* R2: 10,000 reads

After filtering:

* R1: 9,047 reads
* R2: 9,047 reads

Approximately **90.47% of the original read pairs were retained**.

For R1:

* Q20 bases increased from **93.24%** to **94.66%**.
* Q30 bases increased from **90.50%** to **92.20%**.

For R2:

* Q20 bases increased from **86.97%** to **91.08%**.
* Q30 bases increased from **83.26%** to **87.70%**.

`fastp` also reported that 131 reads contained adapter sequence that was trimmed. Reads were additionally filtered because of low quality, excessive ambiguous bases, or insufficient length.

## Did quality control make a difference?

The FastQC reports before and after trimming looked very similar.

For R1, the original file contained 10,000 sequences and approximately 755.1 kbp, while the trimmed file contained 9,047 sequences and approximately 682.5 kbp. Most of the original data passed filtering, so there was not a dramatic visual difference between the raw and trimmed FastQC graphs.

There was still a measurable improvement in sequence quality. R1 Q30 bases increased from 90.50% to 92.20%, while R2 Q30 bases increased from 83.26% to 87.70%. The numerical improvement was greater for R2.

Overall, the QC step made a **modest rather than dramatic difference**. The starting reads were already relatively high quality, and approximately 90% of the sequences were retained. Quality filtering removed lower-quality reads and bases while slightly increasing the proportion of high-quality sequence data.

## Reproducing the analysis

The complete workflow can be run with:

```bash
make SRR=SRR38258968 N=10000 NAME=TMV_infected_benthamiana
```

The Makefile performs the following steps:

1. Downloads a subset of the SRA run using `fastq-dump`.
2. Places the raw FASTQ files in the `fastq/` directory.
3. Renames the reads using a descriptive sample name.
4. Runs FastQC on the raw reads.
5. Uses fastp to trim and filter the reads.
6. Stores processed reads in the `trimmed/` directory.
7. Runs FastQC again on the processed reads.
8. Stores the QC reports in the `qc/` directory.

The workflow requires `fastq-dump`, `fastqc`, `fastp`, and GNU Make.
