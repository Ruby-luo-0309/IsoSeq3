---
layout: default
parent: Single cell
title: Examples
nav_order: 4
---

## Real-world example

This is an example of an end-to-end cmd-line-only workflow:

    # Download HiFi reads
    $ wget https://downloads.pacbcloud.com/public/dataset/IsoSeq_sandbox/2022_pbmc_singlecell_mini/ccs.bam
    $ wget https://downloads.pacbcloud.com/public/dataset/IsoSeq_sandbox/2022_pbmc_singlecell_mini/ccs.bam.pbi

    # Download cDNA primers
    $ wget https://downloads.pacbcloud.com/public/dataset/IsoSeq_sandbox/2022_pbmc_singlecell_mini/primers.fasta
    
    # Download cell barcode include list
    $ wget https://downloads.pacbcloud.com/public/dataset/IsoSeq_sandbox/10x_barcodes/3M-february-2018-REVERSE-COMPLEMENTED.txt.gz

    # Check lima version to be >= 2.6.0
    $ lima --version
    lima 2.6.0

    # Check isoseq3 version to be >= 3.8.2
    $ isoseq3 --version
    isoseq3 3.8.2 

    # cDNA primer removal and read orientation
    $ lima --per-read --isoseq ccs.bam primers.fasta output.bam

    # Clip UMI and cell barcode
    $ isoseq3 tag output.5p--3p.bam flt.bam --design T-12U-16B

    # Remove poly(A) tails and concatemer
    $ isoseq3 refine flt.bam primers.fasta fltnc.bam --require-polya

    # Correct single cell barcodes based on an include list
    $ isoseq3 correct -B 3M-february-2018-REVERSE-COMPLEMENTED.txt.gz fltnc.bam corrected.bam

    # Deduplicate reads based on UMIs
    $ samtools sort -t CB corrected.bam -o corrected.sorted.bam
    $ isoseq3 groupdedup corrected.sorted.bam dedup.bam 
