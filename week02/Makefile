SHELL := /bin/sh

ACCESSION := NC_001367.1
FASTA_URL := https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=nuccore&id=$(ACCESSION)&rettype=fasta&retmode=text
GFF_URL := https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?db=nuccore&id=$(ACCESSION)&rettype=gff3&retmode=text

DATA_DIR := data/raw
FASTA := $(DATA_DIR)/$(ACCESSION).fasta
GFF := $(DATA_DIR)/$(ACCESSION).gff3

.PHONY: all genome clean

all: genome

genome: $(FASTA) $(GFF)

$(DATA_DIR):
	mkdir -p $@

$(FASTA): | $(DATA_DIR)
	curl --fail --location --retry 3 --output $@ '$(FASTA_URL)'

$(GFF): | $(DATA_DIR)
	curl --fail --location --retry 3 --output $@ '$(GFF_URL)'

clean:
	rm -rf $(DATA_DIR)
