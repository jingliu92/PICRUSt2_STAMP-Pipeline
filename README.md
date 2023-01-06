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
The entire PICRUSt2 pipeline can be run using a single script, called 'picrust2_pipeline.py'. This script will run each of the 4 key steps outlined on this wiki: (1) sequence placement, (2) hidden-state prediction of genomes, (3) metagenome prediction, (4) pathway-level predictions.
```
picrust2_pipeline.py -s Qiime2/rep_seqs/dna-sequences.fasta -i feature_table/feature_table.txt -o picrust3 -p 8
```


