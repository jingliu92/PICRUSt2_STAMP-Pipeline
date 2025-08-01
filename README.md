# PICRUSt2 + STAMP for Microbial 16S rRNA Gene Data

![Conda](https://img.shields.io/conda/vn/bioconda/picrust2?label=PICRUSt2&logo=anaconda)
![License](https://img.shields.io/github/license/picrust/picrust2)
![Last Commit](https://img.shields.io/github/last-commit/picrust/picrust2)
![GitHub Repo stars](https://img.shields.io/github/stars/picrust/picrust2)
![GitHub forks](https://img.shields.io/github/forks/picrust/picrust2)
![GitHub issues](https://img.shields.io/github/issues/picrust/picrust2)

PICRUSt2 (Phylogenetic Investigation of Communities by Reconstruction of Unobserved States) is a powerful tool to predict the functional composition of metagenomes from 16S rRNA gene data.

---

## Workflow Overview 
![image](https://user-images.githubusercontent.com/100873921/210913670-b41bc185-d372-45f7-ad2e-0ef5a97c9bf5.png)

Check the detailed information from [The Huttenhower Lab](https://huttenhower.sph.harvard.edu/picrust/).

---

## Installation

```bash
conda create -n picrust2 -c bioconda -c conda-forge picrust2=2.4.1
conda activate picrust2
```

### Troubleshooting

To fully remove old Conda installations:

```bash
rm -rf ~/opt/anaconda3
rm -rf ~/.conda ~/.condarc

# Clear Conda from bash profile
vi ~/.bash_profile
# In vi: press `dd` to delete each line, then type `:wq` to save and exit
```

---

## Run the Pipeline

```bash
picrust2_pipeline.py \
  -s Qiime2/rep_seqs/dna-sequences.fasta \
  -i feature_table/feature_table.txt \
  -o picrust2 \
  -p 8
```

### Input:
- `dna-sequences.fasta`: Representative ASV sequences from QIIME2
- `feature_table.txt`: ASV abundance table (BIOM TSV format)

---

## Key Outputs

| Folder               | Description |
|----------------------|-------------|
| `EC_metagenome_out`  | EC number-based metagenome predictions |
| `KO_metagenome_out`  | KEGG Orthology predictions |
| `pathways_out`       | MetaCyc pathway abundance and coverage |

Each includes:
- `pred_metagenome_unstrat.tsv.gz` – Functional predictions  
- `seqtab_norm.tsv.gz` – Copy number-normalized table  
- `weighted_nsti.tsv.gz` – Sample-level NSTI scores

---

## Functional Annotation

### Enzyme Commission (EC)
- EC numbers classify enzyme-catalyzed reactions.
- Same EC number = same reaction across organisms.

### KEGG Orthology (KO)
- KO groups genes by function.
- Supports mapping to metabolism, disease, and pathway networks.

---

## Add Functional Descriptions

```bash
add_descriptions.py -i EC_metagenome_out/pred_metagenome_unstrat.tsv.gz -m EC \
                    -o EC_metagenome_out/pred_metagenome_unstrat_descrip.tsv.gz

add_descriptions.py -i KO_metagenome_out/pred_metagenome_unstrat.tsv.gz -m KO \
                    -o KO_metagenome_out/pred_metagenome_unstrat_descrip.tsv.gz

add_descriptions.py -i pathways_out/path_abun_unstrat.tsv.gz -m METACYC \
                    -o pathways_out/path_abun_unstrat_descrip.tsv.gz
```

---

## Add KEGG Pathway Level 3

1. Download KEGG mapping files from:  
   [PICRUSt2 description maps](https://github.com/picrust/picrust2/tree/master/picrust2/default_files/description_mapfiles)

2. Generate pathway tables:

```bash
metagenome_pipeline.py -i feature_table.txt \
  -m marker_predicted_and_nsti.tsv.gz \
  -f KO_predicted.tsv.gz \
  -o KO_metagenome_out --strat_out

pathway_pipeline.py -i KO_metagenome_out/pred_metagenome_unstrat.tsv.gz \
  -o KEGG_pathways_out --no_regroup \
  --map KO_metagenome_out/KEGG_pathways_to_KO.txt

add_descriptions.py -i KEGG_pathways_out/path_abun_unstrat.tsv.gz \
  -o KEGG_pathways_out/path_abun_unstrat_descrip.tsv.gz \
  --custom_map_table KO_metagenome_out/KEGG_pathways_info.tsv
```

---

## Add KEGG Level 1 & 2 with EasyMicrobiome

```bash
git clone https://github.com/YongxinLiu/EasyMicrobiome
chmod +x EasyMicrobiome/linux/*

# Add to environment path
echo "PATH=$PATH:`pwd`/EasyMicrobiome/linux:`pwd`/EasyMicrobiome/script" >> ~/.bashrc
source ~/.bashrc
```

### Prepare input
```bash
gunzip pred_metagenome_unstrat.tsv.gz
mv pred_metagenome_unstrat.tsv KEGG.KO.txt
```

### Run summarization
```bash
python3 ../EasyMicrobiome/script/summarizeAbundance.py \
  -i KEGG.KO.txt \
  -m ../EasyMicrobiome/kegg/KO1-4.txt \
  -c 2,3,4 -s ',+,+,' -n raw \
  -o KEGG
```

---

## 📊 Visualize in STAMP

### Install STAMP (MacOS)

```bash
conda deactivate
conda create -n stamp python=2.7
conda activate stamp

pip install numpy cython biom-format==2.1.7 STAMP
conda install -n stamp -c asmeurer pyqt=4
```

### Run STAMP
```bash
conda activate stamp
STAMP
```

### Import Data
- Use `path_abun_unstrat_descrip.tsv` (remove `KO` column if needed)
- Prepare `Sample_list` for group metadata
<img width="1135" alt="Screen Shot 2023-03-21 at 6 07 57 PM" src="https://user-images.githubusercontent.com/100873921/226762075-c44b219a-9b03-4e2b-ae7e-4e64654cf0d4.png">

---

## Example Dataset

Try a quick test using this [example dataset](link-to-your-example-data):

```bash
picrust2_pipeline.py \
  -s example_data/rep_seqs.fasta \
  -i example_data/feature_table.txt \
  -o example_output \
  -p 4
```

---

## Recommended Directory Structure

```text
project/
├── Qiime2/
│   ├── rep_seqs.fasta
│   └── feature_table.txt
├── picrust3/
│   ├── EC_metagenome_out/
│   ├── KO_metagenome_out/
│   └── pathways_out/
├── EasyMicrobiome/
│   ├── script/
│   └── kegg/
```

---

## References
1. https://github.com/YongxinLiu/EasyMicrobiome
2. https://huttenhower.sph.harvard.edu/picrust/
3. Douglas, G.M., Maffei, V.J., Zaneveld, J.R. et al. PICRUSt2 for prediction of metagenome functions. Nat Biotechnol 38, 685–688 (2020). https://doi.org/10.1038/s41587-020-0548-6

---
## Contact

For questions or contributions, feel free to open an issue or submit a pull request.

---

