# PICRUSt2-Pipeline

# Installation
```
conda create -n picrust2 -c bioconda -c conda-forge picrust2=2.5.1
conda activate picrust2

# If error occurs, you may want to complete delet anocoda3 or minicoda3, below are the commands for completely delet them
rm -rf ~/opt/anacoda3
rm -rf ~/.condac ~/.conda
vi ~/.bash_profile (press enter, press DD, delete all lines. Press ESC, then Shift+: then wq)
```

picrust2_pipeline.py -s /Users/jingliu1/Desktop/phD/NE_model/Inbred_Data_Analysis/Qiime2/rep_seqs/dna-sequences.fasta -i /Users/jingliu1/Desktop/phD/NE_model/Inbred_Data_Analysis/feature_table/feature_table.txt -o /Users/jingliu1/Desktop/phD/NE_model/Inbred_Data_Analysis/Picrust --threads 8
