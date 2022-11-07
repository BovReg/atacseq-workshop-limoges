.. _ATAC-seq_pipeline-page:

***************
nf-core/atacseq
***************

Introduction
============

`nfcore/atacseq <https://nf-co.re/atacseq>`__ is a bioinformatics best-practice pipeline used for the analysis of ATAC-seq data. As discussed before, 
the pipeline is built using `Nextflow <https://www.nextflow.io/>`__, a workflow manager to run tasks across multiple compute infrastructures in a 
very portable manner. It uses Docker/Singularity containers making installation trivial and results highly reproducible. 
The `Nextflow DSL2 <https://www.nextflow.io/docs/latest/dsl2.html>`__ implementation of this pipeline uses one container per process which makes 
it much easier to maintain and update software dependencies. Where possible, these processes have been submitted to and installed from 
`nf-core/modules <https://github.com/nf-core/modules>`__ in order to make them available to all nf-core pipelines, and
to everyone within the Nextflow community. 

Pipeline summary
================

.. image:: images/metro_map_atacseq.png
	:width: 600
    
The main functionality of the nf-core/atacseq is summarised in the figure above and listed in the following numbered list:

    1. Raw read QC (FastQC)
    2. Adapter trimming (Trim Galore!)
    3. Alignment (BWA)
    4. Mark duplicates (picard)
    5. Merge alignments from multiple libraries of the same sample (picard)
        1. Re-mark duplicates (picard)
        2. Filtering to remove:
            * reads mapping to mitochondrial DNA (SAMtools)
            * reads mapping to blacklisted regions (SAMtools, BEDTools)
            * reads that are marked as duplicates (SAMtools)
            * reads that arent marked as primary alignments (SAMtools)
            * reads that are unmapped (SAMtools)
            * reads that map to multiple locations (SAMtools)
            * reads containing > 4 mismatches (BAMTools)
            * reads that are soft-clipped (BAMTools)
            * reads that have an insert size > 2kb (BAMTools; paired-end only)
            * reads that map to different chromosomes (Pysam; paired-end only)
            * reads that arent in FR orientation (Pysam; paired-end only)
            * reads where only one read of the pair fails the above criteria (Pysam; paired-end only)
        3. Alignment-level QC and estimation of library complexity (picard, Preseq)
        4. Create normalised bigWig files scaled to 1 million mapped reads (BEDTools, bedGraphToBigWig)
        5. Generate gene-body meta-profile from bigWig files (deepTools)
        6. Calculate genome-wide enrichment (deepTools)
        7. Call broad/narrow peaks (MACS2)
        8. Annotate peaks relative to gene features (HOMER)
        9. Create consensus peakset across all samples and create tabular file to aid in the filtering of the data (BEDTools)
        10. Count reads in consensus peaks (featureCounts)
        11. Differential accessibility analysis, PCA and clustering (R, DESeq2)
        12. Generate ATAC-seq specific QC html report (ataqv)
    6. Merge filtered alignments across replicates (picard)
            1. Re-mark duplicates (picard)
            2. Remove duplicate reads (SAMtools)
            3. Create normalised bigWig files scaled to 1 million mapped reads (BEDTools, bedGraphToBigWig)
            4. Call broad/narrow peaks (MACS2)
            5. Annotate peaks relative to gene features (HOMER)
            6. Create consensus peakset across all samples and create tabular file to aid in the filtering of the data (BEDTools)
            7. Count reads in consensus peaks relative to merged library-level alignments (featureCounts)
            8. Differential accessibility analysis, PCA and clustering (R, DESeq2)
    7. Create IGV session file containing bigWig tracks, peaks and differential sites for data visualisation (IGV).
    8. Present QC for raw read, alignment, peak-calling and differential accessibility results (ataqv, MultiQC, R)


.. nextflow run nf-core/atacseq -r dev -params-file ./config/nf-atacseq-params.json -profile docker -c ./config/nextflow.config -resume


.. Exercise
.. ********

.. Pull version ``3.8.1`` of the nf-core/atacseq pipeline, run it using the ``nf-core launch`` command and produce the ``nf-params.json``.

.. .. raw:: html

.. 	<details>
.. 	<summary><a>Solution</a></summary>

.. .. code-block:: console

.. 	nextflow pull nf-core/rnaseq -r 3.8.1
.. 	nf-core launch rnaseq -r 3.8.1

.. .. raw:: html

.. 	</details>
.. |
