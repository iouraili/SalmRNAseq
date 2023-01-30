# SalmRNAseq
A pipeline for rapid and accurate transcript quantification for PE bulk-RNAseq samples with UMIs using Salmon

## Tools
The pipeline uses fastp (Chen et al. 2018) for adapter trimming and low-quality reads filtering, as well as deduplication using UMIs, in case this option is enabled in the pipeline. 

Salmon (Patro et al. 2017) is used for the quantification of transcript expression.

## Input data
The pipeline expect as input a directory populated with paired-end RNAseq FASTQ.GZ files following the naming format SAMPLE_R1.fastq.gz and SAMPLE_R2.fastq.gz. If your files' naming format is, for example, SAMPLE_R1_001_anyotherinformation.fastq.gz make sure remove the "001_anyotherinformation" part or append it to the beginning of the files' name. By default the pipeline will perform no UMI deduplication. You can activate this option by calling snakemake with the UMIs="with-UMIs" configuration option or by editing the config_files/config.yaml file.

## Usage
Activate the conda environment
```
conda activate SalmRNAseq
```
Execute the pipeline with snakemake
```
# Example execution with no UMI and the mouse transcriptome
snakemake -s scripts/Snakefile --cores 12 --config dataset="/path/to/the/directory/with/the/fastq.gz" UMIs="no-UMIs" output_dir="/path/to/output/directory" salmon_index="resources/mm39/salmon_index"
```
For additional options consult the config_files/config.yaml file.


## References

Chen S, Zhou Y, Chen Y, Gu J. fastp: an ultra-fast all-in-one FASTQ preprocessor. Bioinformatics. 2018 Sep 1;34(17):i884-i890. doi: 10.1093/bioinformatics/bty560. PMID: 30423086; PMCID: PMC6129281.

Patro R, Duggal G, Love MI, Irizarry RA, Kingsford C. Salmon provides fast and bias-aware quantification of transcript expression. Nat Methods. 2017 Apr;14(4):417-419. doi: 10.1038/nmeth.4197. Epub 2017 Mar 6. PMID: 28263959; PMCID: PMC5600148.
