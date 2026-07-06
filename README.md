# Protein Function Prediction from Amino Acid Sequences

This project explores how protein language models and sequence-to-sequence LLMs can generate candidate protein function descriptions from amino acid sequences. The goal is to support field scientists who may encounter a novel or poorly annotated protein sequence and need an initial estimate of its biological role before deeper experimental validation.

The workflow combines protein sequence embeddings, similarity-based retrieval, and generative text models. Given a protein sequence, the system attempts to generate a natural-language functional summary similar to the annotations found in curated protein databases.

## Project Motivation

Protein function annotation is a major bottleneck in biological research. Sequencing technologies can identify novel proteins faster than scientists can experimentally characterize them. This project investigates whether pretrained models can use amino acid sequence patterns, similar known proteins, and existing functional descriptions to produce useful first-pass function estimates.

This work is not intended to replace wet-lab validation. Instead, it provides a computational starting point that can help researchers prioritize experiments, compare likely functions, and surface related proteins.

## High-Level Pipeline

```text
Protein sequence
   ↓
Sequence preprocessing and filtering
   ↓
ESM-2 protein embeddings
   ↓
Cosine-similarity retrieval of related proteins
   ↓
Generative model input construction
   ↓
T5 / BART / GPT-style function generation
   ↓
Evaluation with BLEU and ROUGE
```

## Data

The notebooks use protein records containing sequence and functional annotation fields. The observed dataset columns include:

- `accession`
- `name`
- `Full Name`
- `taxon`
- `sequence`
- `function`
- `AlphaFoldDB`

The main prediction task is framed as:

```text
Input: amino acid sequence and/or retrieved descriptions from similar proteins
Output: generated protein function description
```

## Modeling Approaches

### 1. ESM-2 + BART Baseline

The baseline encoder-decoder experiment uses `facebook/esm2_t12_35M_UR50D` as the protein sequence encoder and `facebook/bart-base` as the text decoder. ESM-2 creates contextual embeddings for amino acid sequences, and a projection layer maps those embeddings into the dimensionality expected by BART.

This experiment helped test whether a direct protein-sequence-to-text architecture could generate functional summaries.

### 2. ESM-2 Embedding Similarity Retrieval

A second workflow creates ESM-2 embeddings for proteins and computes pairwise cosine similarity across proteins. Similar proteins are filtered using a similarity threshold of `0.95`, and the top three most similar proteins are extracted for each query protein.

This retrieval step is used to augment the generative model with functional context from biologically similar proteins.

### 3. No-Retrieval T5 Baseline

The no-retrieval baseline fine-tunes `t5-small` directly on protein sequence text and target function summaries. This establishes a baseline for function generation without similarity-based context.

### 4. Retrieval-Augmented T5-small

The strongest project direction uses a retrieval-augmented T5-style summarization setup. Instead of giving the model only the raw sequence, the input includes retrieved descriptions from similar proteins. This gives the model additional functional clues and makes the task closer to biological annotation by homology.

### 5. GPT-2 + LoRA Experiment

The repo also includes a GPT-2 LoRA fine-tuning experiment. This notebook explores parameter-efficient text-generation training as another possible route for protein function description generation.

## Evaluation

The project evaluates generated function descriptions using text-generation metrics:

- **BLEU**: measures n-gram overlap between generated and reference function descriptions.
- **ROUGE-1 / ROUGE-2 / ROUGE-L**: measures unigram, bigram, and longest-common-subsequence overlap.
- **Validation loss**: used during model training to monitor learning.

Early baseline results show that direct sequence-to-text generation is difficult, especially when the model lacks retrieval context. Retrieval-augmented inputs are the most promising direction because they provide functional examples from similar known proteins.

## Current Repository Contents

```text
proteinFunctionPrediction/
├── BaselineModel_EsmBART_266.ipynb
├── Copy_of_EMBeddingCosineSimilarity.ipynb
├── GPT2_Lora_TextGeneration.ipynb
├── Projectw266_ProteinFunctionGeneration.ipynb
├── T5Summarization_small.ipynb
├── no_retrival_baseline.ipynb
└── README.md
```

## Recommended Repository Organization

A cleaner structure would make the work easier to understand and reproduce:

```text
proteinFunctionPrediction/
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_esm2_embedding_similarity.ipynb
│   ├── 03_esm2_bart_baseline.ipynb
│   ├── 04_no_retrieval_t5_baseline.ipynb
│   ├── 05_retrieval_augmented_t5_small.ipynb
│   └── 06_gpt2_lora_experiment.ipynb
├── src/
│   ├── data_preprocessing.py
│   ├── embedding_retrieval.py
│   ├── train_t5.py
│   ├── evaluate_generation.py
│   └── inference.py
├── data/
│   └── README.md
├── results/
│   ├── metrics_summary.md
│   └── sample_predictions.md
└── models/
    └── README.md
```

Recommended cleanup steps:

1. Move all notebooks into a `notebooks/` folder.
2. Rename notebooks so their order and purpose are clear.
3. Add a `requirements.txt` file for reproducibility.
4. Add a `.gitignore` file to avoid committing datasets, model checkpoints, generated CSV files, pickle files, and notebook checkpoints.
5. Clear notebook outputs before committing, especially any outputs that reference private paths, tokens, or local credentials.
6. Add a `results/metrics_summary.md` file with final BLEU and ROUGE scores.
7. Add a `results/sample_predictions.md` file showing a few input sequences, reference functions, and generated summaries.

## Suggested Requirements

```text
accelerate
bioseq
datasets
nltk
numpy
pandas
peft
rouge_score
sacrebleu
scikit-learn
sentencepiece
torch
tqdm
transformers
```

## Example Usage

After the notebooks are converted into scripts, a typical workflow could look like this:

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Generate ESM-2 embeddings
python src/embedding_retrieval.py --input data/proteins.csv --output data/esm2_embeddings.pkl

# 3. Train a T5-small function generator
python src/train_t5.py --train data/train.csv --valid data/valid.csv --output models/t5_small_function_generator

# 4. Generate predictions on a test set
python src/inference.py --model models/t5_small_function_generator --input data/test.csv --output results/predictions.csv

# 5. Evaluate generated summaries
python src/evaluate_generation.py --predictions results/predictions.csv --output results/metrics_summary.md
```

## Key Takeaways

- Protein function prediction can be framed as a sequence-to-text generation problem.
- ESM-2 embeddings provide a useful representation of amino acid sequences.
- Direct sequence-to-description generation is challenging with small models and limited compute.
- Retrieval from similar known proteins improves the biological context available to the generator.
- T5-small offers a practical balance between model size, training cost, and summarization ability.

## Future Work

- Convert the notebooks into reusable Python scripts.
- Add a reproducible training and evaluation pipeline.
- Compare T5-small, T5-base, BART, and GPT-style models under the same dataset split.
- Add more biologically meaningful evaluation, such as GO-term overlap or expert review.
- Package an inference workflow where a user can submit a novel amino acid sequence and receive a candidate functional summary plus the top retrieved similar proteins.

## Project Status

This repository is currently an exploratory research project. The notebooks document multiple modeling attempts and show the progression from baseline sequence-to-text generation toward retrieval-augmented protein function summarization.
