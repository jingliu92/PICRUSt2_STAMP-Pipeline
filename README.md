# PICRUSt2-Pipeline

# Installation
```
conda create -n picrust2 -c bioconda -c conda-forge picrust2=2.3.0_b
conda activate picrust2
```
# Note
If error occurs, you may want to complete delet anocoda3 or minicoda3, below are the commands for completely delet them

```
rm -rf ~/opt/anacoda3
rm -rf ~/.condac ~/.conda
vi ~/.bash_profile (press enter, press DD, delete all lines. Press ESC, then Shift+: then wq)
```

# Run Picrust2 use one command line
The entire PICRUSt2 pipeline can be run using a single script, called `picrust2_pipeline.py`. This script will run each of the 4 key steps: (1) sequence placement, (2) hidden-state prediction of genomes, (3) metagenome prediction, (4) pathway-level predictions.

The pipeline will generate metagenome predictions for 16S rRNA gene data. The input files should be a FASTA of amplicon sequences variants (ASVs; i.e. your representative sequences, which is dna-sequences.fasta  below) and a BIOM table of the abundance of each ASV across each sample (feature_table.txt below). 
<img width="507" alt="Screen Shot 2023-01-05 at 7 38 16 PM" src="https://user-images.githubusercontent.com/100873921/210912180-f3ed1cd2-9427-4e0e-9122-26fc99accf13.png">
<img width="507" alt="Screen Shot 2023-01-05 at 7 40 09 PM" src="https://user-images.githubusercontent.com/100873921/210912380-9ef77b44-223d-43d3-80fd-e535aabbc5f6.png">

```
picrust2_pipeline.py -s Qiime2/rep_seqs/dna-sequences.fasta -i feature_table/feature_table.txt -o picrust3 -p 8
```


