# Week 3: Collaborating with Others


## Repository reviewed

I reviewed the Week 02 assignment from the following repository:

https://github.com/hairuow622/appbio

## Code safety

I inspected the `Makefile` before running it. The code did not contain any commands that appeared dangerous. It downloads two specific files from the NCBI FTP server using `curl`, decompresses them with `gzip`, creates the appropriate data directories, and moves the completed files into place. It does not use `sudo`, delete files with commands such as `rm`, change permissions, or modify files outside of the assignment directories.

## README evaluation

The README is clear and well organized. It identifies the selected genome, provides the accession and data source, explains how to run the Makefile, and documents the commands used to calculate genome statistics and annotation counts. It also includes expected outputs and screenshots from IGV, which makes it easier to understand what the results should look like.

One reproducibility issue I found was that the README listed `curl`, `gzip`, and GNU Make as required tools, but later used `seqkit` without listing it as a dependency.

## Reproducibility

I first used:

```bash
make -B -n
```

to inspect the commands that the Makefile would execute without actually running them. After confirming that the commands were safe, I ran:

```bash
make -B
```

The FASTA and GFF files were successfully downloaded again from NCBI. Running `git status` afterward showed a clean working tree, indicating that the downloaded files matched the versions committed to the repository.

I also reproduced the reported results:

* Genome size: 12,591,253 bp
* Number of chromosomes: 3
* Number of GFF annotation records: 53,910
* Number of unknown bases (`N`): 402

These values matched the results reported in the README, so the command-line portion of the assignment was reproducible.

## AI prompts and evaluation

I used ChatGPT as the AI Agent for this assignment. I provided the AI with the reviewed repository's `README.md` and `Makefile`, along with information about my own Week 02 solution.

I prompted the AI to help with the following tasks:

* Evaluate whether the reviewed code appeared safe before I ran it.
* Check whether the README clearly explained how to run the workflow and interpret the results.
* Compare the reviewed Week 02 solution with my own Week 02 solution.
* Evaluate which solution was stronger in terms of reproducibility, readability, and structure.
* Identify a specific improvement that could be made to the reviewed repository.

The AI identified that both solutions were reproducible and well organized, but noted that the reviewed Makefile had stronger error handling and used temporary files before moving completed downloads into place. It also identified that `seqkit` was used in the README without being listed as a required dependency. I independently verified the reproducibility results by running the commands myself before making the change.

## Comparison with my solution

My Week 02 solution used the Tobacco mosaic virus reference genome (`NC_001367.1`), while this repository used the *Schizosaccharomyces pombe* genome (`GCF_000002945.2`). Both solutions used a Makefile to download genomic data, organized the files by type, documented the UNIX commands used for analysis, and visualized the genome and annotations in IGV.

The reviewed solution has a particularly robust Makefile. It uses shell error handling with `-eu -o pipefail` and downloads data into temporary files before renaming them, which helps prevent incomplete files from being mistaken for successful downloads. My solution was simpler because the TMV genome is much smaller, but both approaches were reproducible. Overall, I think the reviewed solution has a slightly stronger Makefile structure, while my solution is somewhat simpler to follow.

## Summary

The repository was readable, organized, and reproducible. The Makefile safely downloaded the expected files from NCBI, and I was able to reproduce the reported genome size, chromosome count, annotation count, and number of unknown bases. The README also clearly documented the analysis and IGV visualization steps.

The main issue I identified was that `seqkit` was used for genome statistics but was not listed as a required dependency. I updated the README in my fork to include `seqkit` among the tools required for the workflow. This makes the dependency requirements clearer for another user attempting to reproduce the analysis.

## Pull request

I submitted the following pull request with the documentation correction:

https://github.com/hairuow622/appbio/pull/2
