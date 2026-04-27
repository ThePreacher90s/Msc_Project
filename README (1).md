# EC Level-2 Enzyme Classification from Protein Sequences: From Classical Machine Learning to Protein Language Models

**Author:** Pleasant Omomolawa Ogundowole  
**Student ID:** 23104179  
**Programme:** MSc Data Science, University of Hertfordshire  
**Supervisor:** Hyungrok Kim  
**Module:** 7PAM2002-0206-2025  
**Year:** 2025/2026  

---

## Project Overview

This project presents a systematic comparison of nine machine learning models across four modelling paradigms for Enzyme Commission (EC) Level-2 classification directly from raw protein sequences. A similarity-aware evaluation protocol using MMseqs2 cluster-level splitting at 70% sequence identity was applied throughout to prevent sequence leakage between training and test partitions.

The study makes five original contributions:

1. A systematic comparison of nine models across four modelling paradigms evaluated on an identical similarity-aware test set
2. The first explicit quantification of random-split inflation across multiple model tiers in EC Level-2 classification
3. CNN ablation studies providing empirical justification for every architectural decision
4. Biological interpretation of per-class errors connected to known enzyme biochemistry
5. Empirical separation of two distinct failure modes through class imbalance experiments

---

## Results Summary

| Model | Val Accuracy | Test Accuracy |
|---|---|---|
| AA Composition + LR | 8% | 6% |
| TF-IDF + SGD (baseline) | 73% | 76% |
| TF-IDF + SGD (tuned) | 82% | 83% |
| BiLSTM 512/60ep | 87% | 87% |
| BiLSTM 1024/60ep | 86% | 87% |
| CNN 512/60ep | 93% | 94% |
| CNN 1024/60ep | 93% | 94% |
| ESM-2 150M | 97% | 97% |
| **ESM-2 650M** | **98%** | **98%** |

All models trained and evaluated on similarity-aware splits. Random seed 26 used throughout.

---

## Dataset

- **Source:** UniProtKB/SwissProt, downloaded January 2026 via REST API
- **Total sequences:** 246,331
- **EC Level-2 classes:** 65
- **Train / Val / Test split:** 196,472 / 24,363 / 25,496
- **Splitting method:** MMseqs2 cluster-level at 70% sequence identity, 80% coverage
- **Class imbalance ratio:** approximately 160-fold between largest and smallest class

---

## Repository Structure

```
Msc_Project/
│
├── PSP_(5)_To_My_Supervisor.ipynb   # Main experimental notebook
│
│   The notebook is organised into the following parts:
│
├── Preamble      — Setup, imports, Google Drive mount, plot style
├── Part A        — Data download via UniProt REST API
├── Part A(ii)    — Data cleaning and preprocessing
├── Part B(i-vii) — MMseqs2 installation, clustering, splitting,
│                   leakage verification, saving partitions
├── Part B        — Classical baseline models (AA Composition, TF-IDF)
├── Part C        — CNN and BiLSTM models (512 and 1024)
├── Part D        — ESM-2 150M frozen embeddings and MLP
├── Part D(ii)    — ESM-2 650M frozen embeddings and MLP
├── Part G        — Random split vs similarity-aware comparison
├── Part H        — Biological interpretation of per-class results
├── Part I        — CNN ablation studies
├── Part J        — Class imbalance experiments
│
└── README.md
```

---

## Models Implemented

**Classical Baselines — Part B**
- Amino acid composition features with Logistic Regression
- TF-IDF character n-gram features (3,5) with SGD classifier (baseline and tuned)

**Deep Learning Sequence Models — Part C**
- CNN with three convolutional blocks (kernel sizes 3, 7, 15), two configurations: MAX_LEN 512 and 1024
- BiLSTM with hidden sizes 512 and 1024, 60 epochs

**Protein Language Models — Parts D and D(ii)**
- ESM-2 150M (esm2_t30_150M_UR50D) frozen embeddings with MLP head (512, 256)
- ESM-2 650M (esm2_t33_650M_UR50D) frozen embeddings with MLP head (512, 256)

---

## Environment

- **Platform:** Google Colab
- **GPU:** NVIDIA A100
- **Python:** 3.10
- **Key libraries:** PyTorch, scikit-learn, fair-esm, MMseqs2, pandas, numpy, matplotlib, seaborn, biopython

---

## How To Run

1. Clone the repository:
```
git clone https://github.com/ThePreacher90s/Msc_Project.git
```

2. Open the notebook in Google Colab:
```
PSP_(5)_To_My_Supervisor.ipynb
```

3. Mount your Google Drive and set the PROJECT path:
```python
from google.colab import drive
drive.mount('/content/drive')
PROJECT = "/content/drive/MyDrive/MSc_Protein_EC_Project"
```

4. Run the cells sequentially from Part A through Part I. Each part is clearly labelled in the notebook.

---

## Data Access

The full dataset, saved model weights, and intermediate outputs are available on Google Drive:

https://drive.google.com/drive/folders/1lzJV2oKU07FxD4rLUb5EZ5n61yDxDG9R?usp=sharing

The Google Drive contains large files that cannot be stored directly in this repository including the cleaned dataset CSV, MMseqs2 cluster files, ESM-2 embeddings, and trained model checkpoints.

---

## Key Findings

- ESM-2 650M achieves 98% test accuracy under similarity-aware evaluation, a 67% reduction in error rate compared to the best CNN
- Random splitting inflates reported accuracy by 0.8 to 6.4 percentage points depending on model type, with classical models most affected
- The kernel-15 convolutional block is the most critical CNN component — removing it causes a 10.8 percentage point drop in accuracy
- Two distinct failure modes identified: data scarcity in rare sub-classes, and biochemical ambiguity in well-populated sub-classes sharing conserved structural folds
- EC 6.3 (carbon-nitrogen ligases, F1=0.571) is the hardest sub-class to classify due to ATP-grasp fold ambiguity shared across multiple EC classes

---

## References

Ryu, J.Y., Kim, H.U. and Lee, S.Y. (2019) 'Deep learning enables high-quality and high-throughput prediction of enzyme commission numbers', *Proceedings of the National Academy of Sciences*, 116(28), pp. 13996–14001.

Lin, Z. et al. (2023) 'Evolutionary-scale prediction of atomic-level protein structure with a language model', *Science*, 379(6637), pp. 1123–1130.

Steinegger, M. and Söding, J. (2017) 'MMseqs2 enables sensitive protein sequence searching for the analysis of massive data sets', *Nature Biotechnology*, 35(11), pp. 1026–1028.

UniProt Consortium (2023) 'UniProt: the Universal Protein Knowledgebase in 2023', *Nucleic Acids Research*, 51(D1), pp. D523–D531.

---

## Academic Integrity

This project was completed independently as part of the MSc Data Science programme at the University of Hertfordshire. All code was written by the author. All experiments were conducted by the author. The dissertation report was written by the author.
