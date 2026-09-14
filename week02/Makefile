SHELL := /bin/sh

ACCESSION := NC_001367.1

FASTA_URL := https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=nuccore&id=$(ACCESSION)&rettype=fasta&retmode=text
GFF_URL := https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=nuccore&id=$(ACCESSION)&rettype=gff3&retmode=text

FASTA_DIR := fasta
GFF_DIR := gff

FASTA := $(FASTA_DIR)/$(ACCESSION).fasta
GFF := $(GFF_DIR)/$(ACCESSION).gff3

.PHONY: all genome count clean

all: genome

genome: $(FASTA) $(GFF)

$(FASTA_DIR):
	mkdir -p $@

$(GFF_DIR):
	mkdir -p $@

$(FASTA): | $(FASTA_DIR)
	@if [ -f "$@" ]; then echo "FASTA already exists: $@"; else curl --fail --location --retry 3 --output $@ '$(FASTA_URL)'; fi

$(GFF): | $(GFF_DIR)
	@if [ -f "$@" ]; then echo "GFF3 already exists: $@"; else curl --fail --location --retry 3 --output $@ '$(GFF_URL)'; fi

count: $(GFF)
	@awk '!/^#/ && NF { n++ } END { print n }' $(GFF)

clean:
	rm -rf $(FASTA_DIR) $(GFF_DIR)
