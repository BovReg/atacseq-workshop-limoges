# Subsample fastq files

1. Get only chromosome 4 from the fasta file

    ```console
    cd /home/kadomu/DELETE_ME/atacseq_limoges_dataset/data
    samtools faidx ARS-UCD1.2_Btau5.0.1Y.fa 4 > chr4.fa
    ```

2. Modify the entry header of `chr4.fa` from `>4` to `>4|kraken:taxid|9913`

3. Build Kraken2 database for custom genome

    ```console
    conda activate kraken2
    DBNAME='bostaurus_chr4'
    # Next step does not work when connected to the CRG vpn, see https://github.com/DerrickWood/kraken2/issues/38
    kraken2-build --download-taxonomy --db $DBNAME
    kraken2-build --add-to-library chr4.fa --db $DBNAME
    kraken2-build --build --db $DBNAME
    ```

4. Subsample fastq files using the viralrecon pipeline

    ```bash
    nextflow run nf-core/viralrecon \
        --input samplesheet.csv \
        --kraken2_db bostaurus_chr4/ \
        --fasta chr4.fa \
        --platform illumina \
        --protocol metagenomic \
        --skip_fastqc \
        --skip_fastp \
        --skip_multiqc \
        --skip_assembly \
        --skip_variants \
        --skip_consensus \
        --skip_variants_quast \
        --skip_variants_long_table \
        --skip_pangolin \
        --skip_nextclade \
        --skip_asciigenome \
        --skip_cutadapt \
        --skip_mosdepth \
        --skip_multiqc \
        --outdir results_subsampled \
        -profile singularity \
        -c custom.config \ 
        -resume
    ```
    <!-- en mi caso me interesan las que son clasificadas que son las que estan en el chromosoma 4 no las no clasificadas que se conseguirian con `--kraken2_variants_host_filter` \ -->

5. Move the files to the data directory:

    ```bash
    mv /home/kadomu/DELETE_ME/atacseq_limoges_dataset/data/results_subsampled/kraken2/MAC-T_7kCELLS_1.classified.fastq.gz ./MAC-T_7kCELLS_R1.fastq.gz
    mv /home/kadomu/DELETE_ME/atacseq_limoges_dataset/data/results_subsampled/kraken2/MAC-T_7kCELLS_2.classified.fastq.gz ./MAC-T_7kCELLS_R2.fastq.gz
    mv /home/kadomu/DELETE_ME/atacseq_limoges_dataset/data/results_subsampled/kraken2/MDBK_7kCELLS_1.classified.fastq.gz ./MDBK_7kCELLS_R1.fastq.gz
    mv /home/kadomu/DELETE_ME/atacseq_limoges_dataset/data/results_subsampled/kraken2/MDBK_7kCELLS_2.classified.fastq.gz ./MDBK_7kCELLS_R2.fastq.gz
    ```

6. Generate reference for just chromosome 4:

    ```bash
    zcat Bos_taurus.ARS-UCD1.2.105.gtf.gz | grep ^4 > Bos_taurus.ARS-UCD1.2.105_chr4.gtf
    pigz -4 Bos_taurus.ARS-UCD1.2.105_chr4.gtf
    cp chr4.fa ARS-UCD1.2_Btau5.0.1Y.chr4.fa

    aws s3 cp MAC-T_7kCELLS_R1.fastq.gz  s3://cbcrg-eu/atacseq-training-bovreg/data/
    aws s3 cp MAC-T_7kCELLS_R2.fastq.gz  s3://cbcrg-eu/atacseq-training-bovreg/data/
    aws s3 cp MDBK_7kCELLS_R1.fastq.gz  s3://cbcrg-eu/atacseq-training-bovreg/data/
    aws s3 cp MDBK_7kCELLS_R2.fastq.gz  s3://cbcrg-eu/atacseq-training-bovreg/data/
    aws s3 cp ARS-UCD1.2_Btau5.0.1Y.chr4.fa   s3://cbcrg-eu/atacseq-training-bovreg/data/
    aws s3 cp Bos_taurus.ARS-UCD1.2.105_chr4.gtf.gz s3://cbcrg-eu/atacseq-training-bovreg/data/

    cd ..
    mkdir config
    cd config
    aws s3 cp s3://cbcrg-eu/atacseq-training-bovreg/config/nf-atacseq-params.json .
    
    ```

## General implementation for any chromosome

1. Get only chromosome 4 from the fasta file

    ```console
    CHROMOSOME=27
    cd /home/kadomu/DELETE_ME/atacseq_limoges_dataset/data
    samtools faidx ARS-UCD1.2_Btau5.0.1Y.fa $CHROMOSOME > "chr$CHROMOSOME.fa"
    ```

2. Modify the entry header of `chr$CHROMOSOME.fa` from `>$CHROMOSOME` to `>$CHROMOSOME|kraken:taxid|9913`

3. Build Kraken2 database for custom genome

    ```console
    conda activate kraken2
    DBNAME="bostaurus_chr$CHROMOSOME"
    # Next step does not work when connected to the CRG vpn, see https://github.com/DerrickWood/kraken2/issues/38
    kraken2-build --download-taxonomy --db $DBNAME
    kraken2-build --add-to-library "chr${CHROMOSOME}.fa" --db $DBNAME
    kraken2-build --build --db $DBNAME
    ```

4. Subsample fastq files using the viralrecon pipeline

    ```bash
    NXF_WORK=22.10.0 nextflow run nf-core/viralrecon \
        --input samplesheet.csv \
        --kraken2_db $DBNAME/ \
        --fasta "chr${CHROMOSOME}.fa" \
        --platform illumina \
        --protocol metagenomic \
        --skip_fastqc \
        --skip_fastp \
        --skip_multiqc \
        --skip_assembly \
        --skip_variants \
        --skip_consensus \
        --skip_variants_quast \
        --skip_variants_long_table \
        --skip_pangolin \
        --skip_nextclade \
        --skip_asciigenome \
        --skip_cutadapt \
        --skip_mosdepth \
        --skip_multiqc \
        --outdir results_subsampled_$CHROMOSOME \
        -profile singularity \
        -c custom.config \
        -resume
    ```
    <!-- en mi caso me interesan las que son clasificadas que son las que estan en el chromosoma 4 no las no clasificadas que se conseguirian con `--kraken2_variants_host_filter` \ -->

5. Move the files to the data directory:

    ```bash
    mv /home/kadomu/DELETE_ME/atacseq_limoges_dataset/data/results_subsampled/kraken2/MAC-T_7kCELLS_1.classified.fastq.gz ./MAC-T_7kCELLS_R1.fastq.gz
    mv /home/kadomu/DELETE_ME/atacseq_limoges_dataset/data/results_subsampled/kraken2/MAC-T_7kCELLS_2.classified.fastq.gz ./MAC-T_7kCELLS_R2.fastq.gz
    mv /home/kadomu/DELETE_ME/atacseq_limoges_dataset/data/results_subsampled/kraken2/MDBK_7kCELLS_1.classified.fastq.gz ./MDBK_7kCELLS_R1.fastq.gz
    mv /home/kadomu/DELETE_ME/atacseq_limoges_dataset/data/results_subsampled/kraken2/MDBK_7kCELLS_2.classified.fastq.gz ./MDBK_7kCELLS_R2.fastq.gz
    ```

6. Generate reference for just chromosome 4:

    ```bash
    zcat Bos_taurus.ARS-UCD1.2.105.gtf.gz | grep ^4 > Bos_taurus.ARS-UCD1.2.105_chr4.gtf
    pigz -4 Bos_taurus.ARS-UCD1.2.105_chr4.gtf
    cp chr4.fa ARS-UCD1.2_Btau5.0.1Y.chr4.fa

    aws s3 cp MAC-T_7kCELLS_R1.fastq.gz  s3://cbcrg-eu/atacseq-training-bovreg/data/
    aws s3 cp MAC-T_7kCELLS_R2.fastq.gz  s3://cbcrg-eu/atacseq-training-bovreg/data/
    aws s3 cp MDBK_7kCELLS_R1.fastq.gz  s3://cbcrg-eu/atacseq-training-bovreg/data/
    aws s3 cp MDBK_7kCELLS_R2.fastq.gz  s3://cbcrg-eu/atacseq-training-bovreg/data/
    aws s3 cp ARS-UCD1.2_Btau5.0.1Y.chr4.fa   s3://cbcrg-eu/atacseq-training-bovreg/data/
    aws s3 cp Bos_taurus.ARS-UCD1.2.105_chr4.gtf.gz s3://cbcrg-eu/atacseq-training-bovreg/data/

    cd ..
    mkdir config
    cd config
    aws s3 cp s3://cbcrg-eu/atacseq-training-bovreg/config/nf-atacseq-params.json .
    
    ```