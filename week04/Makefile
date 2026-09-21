SHELL := /bin/bash
.SHELLFLAGS := -eu -o pipefail -c

# Default parameters
SRR ?= SRR38258968
N ?= 10000
NAME ?= $(SRR)

# Output directories
RAW_DIR := fastq
TRIM_DIR := trimmed
QC_RAW_DIR := qc/raw
QC_TRIM_DIR := qc/trimmed
FASTP_DIR := qc/fastp

.PHONY: all download qc_raw trim qc_trimmed

all: download qc_raw trim qc_trimmed


# Download the first N SRA spots
download:
	mkdir -p $(RAW_DIR)

	# Remove temporary accession-named files from an earlier run
	rm -f "$(RAW_DIR)/$(SRR).fastq" \
	      "$(RAW_DIR)/$(SRR)_1.fastq" \
	      "$(RAW_DIR)/$(SRR)_2.fastq"

	fastq-dump \
		--split-files \
		--skip-technical \
		-X $(N) \
		--outdir $(RAW_DIR) \
		$(SRR)

	# Rename according to whether the run is paired-end or single-end
	if [[ -f "$(RAW_DIR)/$(SRR)_2.fastq" ]]; then \
		mv "$(RAW_DIR)/$(SRR)_1.fastq" "$(RAW_DIR)/$(NAME)_R1.fastq"; \
		mv "$(RAW_DIR)/$(SRR)_2.fastq" "$(RAW_DIR)/$(NAME)_R2.fastq"; \
	elif [[ -f "$(RAW_DIR)/$(SRR)_1.fastq" ]]; then \
		mv "$(RAW_DIR)/$(SRR)_1.fastq" "$(RAW_DIR)/$(NAME).fastq"; \
	elif [[ -f "$(RAW_DIR)/$(SRR).fastq" ]]; then \
		mv "$(RAW_DIR)/$(SRR).fastq" "$(RAW_DIR)/$(NAME).fastq"; \
	else \
		echo "ERROR: FASTQ download did not produce an expected file"; \
		exit 1; \
	fi


# FastQC before trimming
qc_raw: download
	mkdir -p $(QC_RAW_DIR)

	if [[ -f "$(RAW_DIR)/$(NAME)_R2.fastq" ]]; then \
		fastqc \
			--outdir $(QC_RAW_DIR) \
			"$(RAW_DIR)/$(NAME)_R1.fastq" \
			"$(RAW_DIR)/$(NAME)_R2.fastq"; \
	else \
		fastqc \
			--outdir $(QC_RAW_DIR) \
			"$(RAW_DIR)/$(NAME).fastq"; \
	fi


# Trim/filter reads with fastp
trim: qc_raw
	mkdir -p $(TRIM_DIR) $(FASTP_DIR)

	if [[ -f "$(RAW_DIR)/$(NAME)_R2.fastq" ]]; then \
		fastp \
			-i "$(RAW_DIR)/$(NAME)_R1.fastq" \
			-I "$(RAW_DIR)/$(NAME)_R2.fastq" \
			-o "$(TRIM_DIR)/$(NAME)_R1.trimmed.fastq" \
			-O "$(TRIM_DIR)/$(NAME)_R2.trimmed.fastq" \
			--detect_adapter_for_pe \
			--cut_tail \
			--cut_mean_quality 20 \
			--length_required 30 \
			--html "$(FASTP_DIR)/$(NAME).html" \
			--json "$(FASTP_DIR)/$(NAME).json"; \
	else \
		fastp \
			-i "$(RAW_DIR)/$(NAME).fastq" \
			-o "$(TRIM_DIR)/$(NAME).trimmed.fastq" \
			--cut_tail \
			--cut_mean_quality 20 \
			--length_required 30 \
			--html "$(FASTP_DIR)/$(NAME).html" \
			--json "$(FASTP_DIR)/$(NAME).json"; \
	fi


# FastQC after trimming
qc_trimmed: trim
	mkdir -p $(QC_TRIM_DIR)

	if [[ -f "$(TRIM_DIR)/$(NAME)_R2.trimmed.fastq" ]]; then \
		fastqc \
			--outdir $(QC_TRIM_DIR) \
			"$(TRIM_DIR)/$(NAME)_R1.trimmed.fastq" \
			"$(TRIM_DIR)/$(NAME)_R2.trimmed.fastq"; \
	else \
		fastqc \
			--outdir $(QC_TRIM_DIR) \
			"$(TRIM_DIR)/$(NAME).trimmed.fastq"; \
	fi
