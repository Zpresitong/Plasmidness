# Plasmidness

[GitHub Repository](https://github.com/Zpresitong/Plasmidness)

This directory contains all analysis code for the study of **intermediate replicons**.
The project is built around the hypothesis of a continuum between plasmids and chromosomes: between the **typical plasmid** and the **typical chromosome** there exists a class of **intermediate replicons**, which can be classified by the two-dimensional scheme of **plasmidness × size**, and further characterized and validated using genomic features, replication origins, sequence-similarity networks, machine-learning models, and foundation-model embeddings.

All code is organized as **Jupyter Notebooks (.ipynb)** and split into 4 top-level modules. `.ipynb_checkpoints` are auto-generated Jupyter backup directories and can be ignored.

---

## Directory Structure

```
Plasmidness/
├── codes/
│   ├── 1_basic_process/      # Core data processing and analysis pipeline (Stage 1)
│   ├── 2_figure_draw/        # Main-text figure generation (Figs 1–6)
│   ├── 3_supfigure/          # Supplementary figure generation (S01–S22)
│   └── 4_basic_data/          # Basic data table generation (S1–S5)
├── data/
│   └── basic_data/           # Output data tables (Table 1–5)
│       ├── Table1_replicon_plasmidness_data.tsv
│       ├── Table2_replicon_category_prediction_data.tsv
│       ├── Table3_replicon_chromid_data.tsv
│       ├── Table4_replicon_mobility_and_AMR_profile_data.tsv
│       └── Table5_IMG_PR_metagenome_plasmid_plasmidness_data.tsv
└── README.md
```

---

## 1. Core Pipeline `1_basic_process/`

### 1.1 Data Preparation
| File | Description |
| --- | --- |
| `Get_assembly_submission_info.ipynb` | Extracts assembly and submission metadata (accession, submission date, etc.) from the NCBI RefSeq `assembly_data_report.jsonl` |
| `Genome_re-assemble/get_accession_biosample_info.ipynb` | Retrieves BioSample / SRA information for each accession |
| `Genome_re-assemble/assemble_info.ipynb` | Summarizes sample information for re-assembly |

### 1.2 Chromosome / Plasmid Prediction and Annotation `Annotation_tools/`
Runs multiple predictors and functional annotation on the replicons of each genus:

- `Chromosome_plasmid_prediction/`
  - `NMS_deeplasmid_rfplasmid.ipynb` — runs **DeepPlasmid** and **RFPlasmid** to predict chromosome / plasmid propensity
  - `NMS_plasflow.ipynb` — runs **PlasFlow** for probabilistic classification
  - `NMS_plasmer.ipynb` — runs **Plasmer** for prediction
  - `NMS_annotation_results.ipynb` — aggregates predictions from all tools into a consensus annotation
- `NMS_replicon_fasta_file.ipynb` — extracts FASTA sequences of non-maximum-size (NMS) replicons from GBFF files
- `NMS_mob_suite.ipynb` — runs **MOB-suite** to analyze replicon mobility (relaxase, oriT, etc.)
- `NMS_amr_finder.ipynb` — runs **AMRFinderPlus** to annotate antimicrobial resistance (AMR) genes
- `eggnog_reannotation.ipynb` — runs **eggNOG-mapper** for functional re-annotation (COG / KEGG / GO)
- `Chromid_finder.ipynb` / `Chromid_data.ipynb` — runs **Chromid_finder** to identify and organize candidate **chromids (chromosome-like plasmids)**

### 1.3 Plasmidness Calculation `BLAST_process_for_plasmidness_calculation/`
- `chr-pla_analysis-extract_nucleotide_sequences.ipynb` — extracts nucleotide sequences for comparison
- `chr-pla_analysis-nucleotide_blastn.ipynb` — runs **BLASTn** against the genus sequence database and computes the plasmid coverage fraction of each contig / replicon
- `chr-pla_analysis-classification.ipynb` — classifies replicons by the "plasmidness × size" scheme (typical plasmid / intermediate replicon / typical chromosome) and merges taxonomy information

### 1.4 Replicon Feature Statistics `Replicon_features/`
- `Replicon-plasmidness-self_bitscore-statistics.ipynb` — computes plasmidness and self-bitscore statistics for each replicon; produces the core table `replicon-plasmid_fraction-self_bitscore_statistics.csv`
- `Replicon_GC.ipynb` — GC content
- `k-mer_calculator.ipynb` / `k-mer_calculator_chr_fragment_generator.ipynb` — k-mer (3–5 mer) frequency calculation and chromosome-fragment generation
- `Replicon_topology.ipynb` — replicon topology (linear / circular) determination
- `Pseudogene_statistics.ipynb` / `Pseudogene_GC_chr_frag.ipynb` — pseudogene statistics and GC features of contigs / sequences
- `COG_category_count.ipynb` / `COG_category_count-chr_fragment.ipynb` — COG functional category counts
- `Replicon_cog_statistics.ipynb` — COG statistics summary

### 1.5 Replicon Association Analysis `Replicon_association/`
- `Intermediate_replicon_BLAST_result.ipynb` — parses BLAST associations between intermediate replicons and chromosomes / plasmids
- `Intermediate_replicon_denominator.ipynb` — for each intermediate replicon, after BLAST against the genus sequence database, counts the number of matching sequences covering each nucleotide site ("denominator"), and computes the average / max / min matching-sequence count across all sites
- `Non-Maximum-Size_replicon_coverage.ipynb` — calculates coverage of the typical chromosome by non-maximum-size replicons
- `Replicon_link_statistic.ipynb` — link statistics between typical chromosomes and sub-replicons (typical plasmids, intermediate replicons)
- `Intermediate_replicon_split_site_cog.ipynb` — COG analysis around intermediate-replicon split sites

### 1.6 Replication Origin Prediction `Replication_origin_prediction/`
- `oriC_prediction.ipynb` — predicts the chromosomal replication origin **oriC**
- `oriV_prediction_NMS.ipynb` — predicts the plasmid replication origin **oriV**
- `ori_results.ipynb` — aggregates oriC / oriV prediction results

### 1.7 Genome Re-assembly `Genome_re-assemble/`
- `Genome_re-assemble_download_sra.ipynb` — downloads raw SRA sequencing data (short / long reads) from BioSample information
- `Genome_re-assemble_process.ipynb` — hybrid re-assembly using **Hybracter / Unicycler / Flye**, etc.
- `Genome_re-assemble_plasmidness.ipynb` — re-computes plasmidness for the re-assembled results

### 1.8 IMG/PR Plasmid Data `IMG_PR_plasmid/`
- `IMG_PR_plasmid-Extract plasmids.ipynb` — extracts plasmids from the IMG/PR database
- `IMG_PR_plasmid-Extract_metagenome_plasmids.ipynb` — extracts metagenome-derived plasmids
- `IMG_PR_plasmid-plasmidness.ipynb` — computes their plasmidness
- `IMG_PR_plasmid-mobility.ipynb` / `IMG_PR_plasmid-AMR_type.ipynb` — mobility and AMR-type analysis

### 1.9 Foundation-Model Embeddings `Foundation_models/`
- `evo2_embedding_vector.ipynb` — extracts sequence embeddings with the **Evo2** model
- `gLM2_embedding_vector.ipynb` — **gLM2** embeddings
- `nucleotide-transformer_embedding_vector.ipynb` — **Nucleotide Transformer** embeddings
- `embedding_vecter_PCA.ipynb` — **PCA** dimensionality reduction and visualization of the embeddings

### 1.10 Machine-Learning Validation `Machine_learning.ipynb`
- Features: k-mer frequency (3–5 mer) + COG functional features (optionally including `size`)
- Classifiers: **Logistic Regression**, **Random Forest**, **LinearSVM**
- Metrics: accuracy, **ROC-AUC**, **PR-AUC**, confusion matrix, classification report
- Goal: test whether "intermediate replicon" is a statistically distinguishable category independent of size

### 1.11 Clustering and Re-clustering `Scatter_recluster-bootstrap.ipynb`
- Hierarchical clustering and re-clustering based on plasmidness × size
- Uses **bootstrap permutation tests** to evaluate the robustness of the three-class scheme (typical plasmid / intermediate replicon / typical chromosome)

### 1.12 Chromosome Cluster Trees `Cluster_tree_for_chromosome/`
- `cluster_tree_construction.ipynb` — builds chromosome cluster trees from normalized bitscores (original / pident_90 / pident_95)
- `cluster_tree_bootstrap_analysis.ipynb` — bootstrap support analysis for the cluster trees

---

## 2. Main-Text Figures `2_figure_draw/`

| File | Content |
| --- | --- |
| `1_scatter_plot.ipynb` | Plasmidness × size scatter plot (with marginal density axes) showing the distribution of the three replicon classes |
| `2_annotation.ipynb` | Representative sequence selection + annotation heatmap of each predictor (PlasFlow / Plasmer / RFPlasmid / DeepPlasmid) |
| `3_similarity_and_defference.ipynb` | Multi-panel comparison: GC content, COG categories, COG PCA, pseudogenes, copy number, oriC/oriV, ML results |
| `4_1_tree_cluster.ipynb` | Cluster-tree display |
| `4_2_chromosome_example.ipynb` | Typical chromosome example |
| `4_3_sequence_align.ipynb` | Pairwise sequence alignment of representative intermediate replicons |
| `4_figure_merged.ipynb` | Merges the Fig. 4 subpanels |
| `5_circle_network.ipynb` | Circular network / genome circos plot showing synteny among replicons and inter-layer associations |
| `6_IMG_PR_result.ipynb` | IMG/PR results: ecosystem distribution, human-body systems, Sankey diagram, etc. |
| `figure_draw_data/` | Data-preparation scripts for the above main figures (e.g., GC/k-mer, gene-content-COG circular networks, network-layer examples, chromosome example data) |

---

## 3. Supplementary Figures `3_supfigure/`

| File | Content |
| --- | --- |
| `S01&S04_Scatter_plot-All_genera-Altered_threshold.ipynb` | Scatter plots for all genera + threshold sensitivity |
| `S02_Replicon_ratio_all_genera.ipynb` | Replicon proportions per genus |
| `S03_Reclusted_result.ipynb` | Re-clustering results |
| `S05_Intermediate_replicon_denominator_statistics.ipynb` | Scatter of average denominator (matching-sequence count per site) vs. contig size for intermediate replicons |
| `S06_Submission_date & sequencing_methods.ipynb` | Submission dates and sequencing methods |
| `S07_Identical_trans_rep.ipynb` | Sequence-identical intermediate replicons |
| `S08_Reassemble_results.ipynb` | Re-assembly validation results |
| `S09_Replicon_type_annotation.ipynb` | Replicon-category annotation |
| `S10_Chromid_result.ipynb` | Chromid analysis results |
| `S11_oriC_oriV_gene_content.ipynb` | Gene content and oriC / oriV statistics |
| `S12_COG_category_count.ipynb` | COG category counts |
| `S13_COG_PCA.ipynb` | PCA on COG features |
| `S14_Pseudogene_and_copy_number.ipynb` | Pseudogenes and copy number |
| `S15_Feature_compare-IR_vs_Chromid.ipynb` | Feature comparison: intermediate replicons vs. chromids |
| `S16_Foundation_model_embedding_vector_results.ipynb` | Foundation-model (Evo2 / gLM2 / NT) embedding results |
| `S17_Machine_learning_category_result.ipynb` | Machine-learning classification results |
| `S18_Riched_region_COG_compare.ipynb` | COG comparison of enriched regions |
| `S19_IR_split_site_result.ipynb` | Intermediate-replicon split-site results |
| `S20_Layer_bar_figure.ipynb` | Layered bar chart |
| `S21_Layer_alter_coverage.ipynb` | Layer-alternative coverage |
| `S22_IMG_PR_result.ipynb` | Supplementary IMG/PR results |

---

## 4. Basic Data Tables `4_basic_data/`

| File | Content |
| --- | --- |
| `Table1_Plasmidness_table.ipynb` | Plasmidness statistics table |
| `Table2_Replicon_category_prediction.ipynb` | Replicon-category prediction summary table |
| `Table3_Chromid_result.ipynb` | Chromid results table |
| `Table4_Mobility_and_AMR_profile.ipynb` | Mobility and AMR profile table |
| `Table5_IMG_PR_results.ipynb` | IMG/PR results table |

---

## 5. Data Tables `data/basic_data/`

The data tables produced by the `4_basic_data/` notebooks are saved in `data/basic_data/` as tab-separated (`.tsv`) files. Column names are self-explanatory and match the code in `4_basic_data/`.

| File | Rows | Description |
| --- | --- | --- |
| `Table1_replicon_plasmidness_data.tsv` | 72,264 | **Plasmidness statistics (Table 1)**: per-replicon plasmidness under the `original` / `pident_90` / `pident_95` labels, size, category (both original and `pident_90`), GC content and topology |
| `Table2_replicon_category_prediction_data.tsv` | 42,253 | **Replicon-category prediction (Table 2)**: per-replicon category (`category-pident_90`) with individual predictions from PlasFlow, Plasmer, RFPlasmid and DeepPlasmid, plus the initial type |
| `Table3_replicon_chromid_data.tsv` | 72,264 | **Chromid results (Table 3)**: chromid identification flag per replicon (chromid / not) with reference data |
| `Table4_replicon_mobility_and_AMR_profile_data.tsv` | 42,253 | **Mobility and AMR profile (Table 4)**: MOB-suite mobility (rep / relaxase / MPF / oriT types, predicted mobility) and AMRFinderPlus AMR profile per replicon |
| `Table5_IMG_PR_metagenome_plasmid_plasmidness_data.tsv` | 14,064 | **IMG/PR results (Table 5)**: IMG/PR metagenome plasmid plasmidness (`original` / `pident_90` / `pident_95`), category, ecosystem / host taxonomy, putatively-complete status, topology, mobility and AMR profile |

---

## Environment and Dependencies

### Python Environment
- Python 3.x
- Core libraries: `pandas`, `numpy`, `matplotlib`, `seaborn`, `scipy`, `scikit-learn`, `Biopython`, `tqdm`
- Machine / deep learning: `torch` (foundation-model embeddings)
- Figure fonts: main figures depend on the Arial family (`arial.ttf` / `ariali.ttf`), registered in code via `fontManager.addfont`

### External Bioinformatics Tools
- **Sequence alignment**: BLAST+ (`makeblastdb` / `blastn`)
- **Chromosome / plasmid prediction**: DeepPlasmid, RFPlasmid, PlasFlow, Plasmer, Chromid_finder
- **Functional annotation**: eggNOG-mapper, AMRFinderPlus, MOB-suite, Prodigal / pyrodigal
- **Replication-origin prediction**: ORCA (oriC), OriV-Finder (oriV)
- **Re-assembly**: SRA Toolkit (e.g., `fasterq-dump`), Hybracter, Unicycler, Flye
- **Foundation models**: Evo2, gLM2, Nucleotide Transformer (GPU recommended)

> Different notebooks require different tool sets; install them according to what you actually run.

---

## Data Notes

- Input genomes: NCBI RefSeq **complete bacteria** genomes (GBFF format, `genomic.gbff`), along with metadata files such as `assembly_data_report.jsonl` / `biosample_info.tsv`
- Reference plasmids: known plasmid sequence database (used for BLASTn-based plasmidness calculation)
- Covers multiple bacterial genera (e.g., *Escherichia*, *Streptococcus*), processed per genus
- **Important**: absolute paths in the code (e.g., `/active-data/...`) are server-specific; replace them with local paths when migrating to another machine
- Before running, confirm the `keep_genus` list, the `label` setting (`original` / `pident_90` / `pident_95`) and threshold parameters match your analysis design

---

## Output Conventions

Results for each genus are written to:

```
{base_folder}/genus/statistics_records/{genus_name}/
├── replicon-plasmid_fraction-self_bitscore_statistics.csv   # core table: plasmidness × size × classification
└── annotations/                                              # per-tool prediction and annotation results
```

Main-text and supplementary figures are output to the corresponding directories under `2_figure_draw/` and `3_supfigure/`.

---

## Usage Recommendations

1. Install the Python libraries and external tools listed in the dependency section;
2. Replace the data paths with your local / cluster paths;
3. It is recommended to run in directory order: `1_basic_process` → `2_figure_draw` → `3_supfigure` / `4_basic_data`;
4. Parameters such as `keep_genus`, `label`, and `BASE_FOLDER` in each notebook should be configured consistently according to your study design.
