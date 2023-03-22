# PICRUSt2-Pipeline
Flowchart
![image](https://user-images.githubusercontent.com/100873921/210913670-b41bc185-d372-45f7-ad2e-0ef5a97c9bf5.png)

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

<img width="507" alt="Screen Shot 2023-01-05 at 7 38 16 PM" src="https://user-images.githubusercontent.com/100873921/210912180-f3ed1cd2-9427-4e0e-9122-26fc99accf13.png"> <img width="507" alt="Screen Shot 2023-01-05 at 7 40 09 PM" src="https://user-images.githubusercontent.com/100873921/210912380-9ef77b44-223d-43d3-80fd-e535aabbc5f6.png">

```
picrust2_pipeline.py -s Qiime2/rep_seqs/dna-sequences.fasta -i feature_table/feature_table.txt -o picrust3 -p 8
```

# The key output files are:

1. `EC_metagenome_out` - Folder containing unstratified EC number metagenome predictions (pred_metagenome_unstrat.tsv.gz), sequence table normalized by predicted 16S copy number abundances (seqtab_norm.tsv.gz), and the per-sample NSTI values weighted by the abundance of each ASV (weighted_nsti.tsv.gz).
2. `KO_metagenome_out` - As EC_metagenome_out above, but for KO metagenomes.
3. `pathways_out` - Folder containing predicted pathway abundances and coverages per-sample, based on predicted EC number abundances.

# Enzyme Commission(EC) number
The Enzyme Commission number (EC number) is a numerical classification scheme for enzymes, based on the chemical reactions they catalyze.[1] As a system of enzyme nomenclature, every EC number is associated with a recommended name for the corresponding enzyme-catalyzed reaction.

EC numbers do not specify enzymes but enzyme-catalyzed reactions. If different enzymes (for instance from different organisms) catalyze the same reaction, then they receive the same EC number.

# KO (KEGG orthology)
The KO (KEGG Orthology) database is a database of molecular functions represented in terms of functional orthologs.

KEGG PATHWAY is a collection of manually drawn pathway maps representing our knowledge of the molecular interaction, reaction and relation networks for:
1. Metabolism
    Global/overview   Carbohydrate   Energy   Lipid   Nucleotide   Amino acid   Other amino   Glycan
    Cofactor/vitamin   Terpenoid/PK   Other secondary metabolite   Xenobiotics   Chemical structure
2. Genetic Information Processing
3. Environmental Information Processing
4. Cellular Processes
5. Organismal Systems
6. Human Diseases
7. Drug Development

# Add descriptions 
`add_descriptions.py` is a convenience script that will add a column to your table of gene family or pathway abundances corresponding to a quick description of each functional category. 

To add a description column to **E.C. number, KO, and MetaCyc pathway** abundance tables respectively:
```
add_descriptions.py -i EC_metagenome_out/pred_metagenome_unstrat.tsv.gz -m EC \
                    -o EC_metagenome_out/pred_metagenome_unstrat_descrip.tsv.gz

add_descriptions.py -i KO_metagenome_out/pred_metagenome_unstrat.tsv.gz -m KO \
                    -o KO_metagenome_out/pred_metagenome_unstrat_descrip.tsv.gz

add_descriptions.py -i pathways_out/path_abun_unstrat.tsv.gz -m METACYC \
                    -o pathways_out/path_abun_unstrat_descrip.tsv.gz
```

# Add Pathway level 3
Download defult file from Github repository (https://github.com/picrust/picrust2/tree/master/picrust2/default_files/description_mapfiles), put it under `KO_metagenome_out` folder

```
# generate contrib.tsv
metagenome_pipeline.py -i ../../qiime2_2023/feature_table/feature_table.txt -m marker_predicted_and_nsti.tsv.gz -f KO_predicted.tsv.gz -o KO_metagenome_out --strat_out
```

```
pathway_pipeline.py -i KO_metagenome_out/pred_metagenome_unstrat.tsv.gz -o KEGG_pathways_out --no_regroup --map KO_metagenome_out/KEGG_pathways_to_KO.txt

add_descriptions.py -i KEGG_pathways_out/path_abun_unstrat.tsv.gz \
                    -o KEGG_pathways_out/path_abun_unstrat_descrip.tsv.gz \
                    --custom_map_table KO_metagenome_out/KEGG_pathways_info.tsv
```

# Visualising PICRUSt2 output in STAMP 

## Intall STAMP on MacOS
```
conda deactivate
conda create -n stamp python=2.7

conda activate stamp

pip install numpy

pip install cython

pip install biom-format==2.1.7

pip install STAMP

conda install -n stamp -c asmeurer pyqt=4

STAMP
```

# Reopen STAMP
```
conda activate stamp 

STAMP
```

# Data Analysis on STAMP

1. Import data (Sample_list and path_abun_unstrat_descrip.tsv-delet Ko colunm)
<img width="1135" alt="Screen Shot 2023-03-21 at 6 07 57 PM" src="https://user-images.githubusercontent.com/100873921/226762075-c44b219a-9b03-4e2b-ae7e-4e64654cf0d4.png">



