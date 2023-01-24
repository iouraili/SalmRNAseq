## Steps to generate the required data and to create the environment for the pipeline

### Directory structure
```
mkdir scripts config_files env annotation_files data resources instructions
```

### Conda environment
```
conda create -n SalmRNAseq salmon fastp snakemake
```

### Prepare the transcriptome indices for Salmon's mapping-based mode of transcript quantification
Salmon's documentation can be found here: https://salmon.readthedocs.io/en/latest/salmon.html#preparing-transcriptome-indices-mapping-based-mode.

To use this mode, we need to build a salmon index for the transcriptome of our interest (the human transcriptome in our case). To do this, we first need to get the human transcript and genome sequences:
```
wget https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_41/gencode.v41.transcripts.fa.gz
mv gencode.v41.transcripts.fa.gz resources
wget https://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_human/release_41/GRCh38.primary_assembly.genome.fa.gz
mv GRCh38.primary_assembly.genome.fa.gz resources
```

Salmon indexing requires the names of the genome targets and the concatenated transcriptome and genome reference:
```
# Genome targets
grep "^>" <(gunzip -c resources/GRCh38.primary_assembly.genome.fa.gz) | cut -d " " -f 1 > resources/decoys.txt
sed -i.bak -e 's/>//g' resources/decoys.txt

# Concatenating the transcriptome and genome reference 
cat resources/gencode.v41.transcripts.fa.gz resources/GRCh38.primary_assembly.genome.fa.gz > resources/gentrome.fa.gz
```

Now we can build the salmon index:
```
conda activate SalmRNAseq
salmon index -t resources/gentrome.fa.gz -d resources/decoys.txt -p 12 -i resources/salmon_index --gencode -k 25
```
