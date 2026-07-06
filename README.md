# Protein Function Prediction from Amino Acid Sequences

This repository contains an exploratory research project on using language models to generate candidate protein function descriptions from amino acid sequences.

The project goal is to support field scientists who may encounter a novel or poorly annotated protein sequence and need an initial estimate of the protein's potential function. The work frames protein function prediction as a sequence-to-text generation problem: amino acid sequence information and retrieved functional context are used to generate a natural-language function description.

## Project Overview

The project investigates a resource-constrained, retrieval-like approach for protein function description generation. Instead of training a large protein description model from scratch, the workflow uses smaller models and retrieval-based context to improve generated descriptions.

The project materials describe the motivation as a compute-constrained alternative to larger protein description generation approaches. The slide deck notes that a referenced base model required 64 NVIDIA V100 GPUs, 12 hours of training, and 23,000 total computing hours for experimentation.

## Repository Structure

```text
proteinFunctionPrediction/
├── README.md
├── notebooks/
│   ├── BaselineModel_EsmBART_266.ipynb
│   ├── Copy_of_EMBeddingCosineSimilarity.ipynb
│   ├── GPT2_Lora_TextGeneration.ipynb
│   ├── Projectw266_ProteinFunctionGeneration.ipynb
│   ├── T5Summarization_small.ipynb
│   └── no_retrival_baseline.ipynb
├── results/
│   ├── Results.md
│   └── W266_Final_Presentation_DL_KM.pdf
└── .gitignore
```

## High-Level Workflow

The project workflow is:

```text
Input protein sequence and known description examples
        ↓
Embedding similarity using cosine similarity
        ↓
Retrieval of related protein function information
        ↓
Text processing
        ↓
T5-small tokenization
        ↓
Sequence-to-sequence generation
        ↓
Generated protein function summary
```

The slide deck describes the modeling pipeline as a retrieval and generation workflow. It starts with input data containing a protein sequence and description, uses cosine similarity for embedding similarity retrieval, applies text processing and T5-small tokenization, and then generates a summary using a sequence-to-sequence architecture.

## Notebooks

The notebooks document the main experimental work:

| Notebook | Purpose |
|---|---|
| `BaselineModel_EsmBART_266.ipynb` | Baseline experiment using ESM-2 sequence representations with a BART-style text generation approach. |
| `Copy_of_EMBeddingCosineSimilarity.ipynb` | Cosine similarity workflow for comparing protein embeddings and retrieving similar proteins. |
| `GPT2_Lora_TextGeneration.ipynb` | GPT-2 LoRA experiment for parameter-efficient text generation. |
| `Projectw266_ProteinFunctionGeneration.ipynb` | Project notebook for protein function generation work. |
| `T5Summarization_small.ipynb` | T5-small summarization experiment for protein function descriptions. |
| `no_retrival_baseline.ipynb` | No-retrieval T5 baseline for comparison against retrieval-based generation. |

## Results

Results materials are stored in the [`results/`](results/) folder.

The final presentation slide deck is available here:

[`results/W266_Final_Presentation_DL_KM.pdf`](results/W266_Final_Presentation_DL_KM.pdf)

The results folder also contains a short summary file:

[`results/Results.md`](results/Results.md)

### Reported Model Results

The slide deck reports the following comparison between a no-retrieval T5-small baseline and a retrieval-based T5-small approach:

| Model | ROUGE-1 | ROUGE-2 | ROUGE-L | BLEU |
|---|---:|---:|---:|---:|
| no-retrieval-t5-small | 0.123 | 0.021 | 0.089 | 0.031 |
| retrieval-t5-small | 0.454 | 0.285 | 0.429 | 4.743 |

The reported results show that the retrieval-based T5-small approach performed better than the no-retrieval T5-small baseline on the listed metrics.

The slide deck also includes additional comparisons across T5-small and T5-base variants, including trainable parameter counts, ROUGE scores, BLEU, and inference time.

## Qualitative Results

The presentation includes example original summaries and predicted summaries for:

- `no_retrieval_baseline_t5`
- `retrieval_baseline_t5`
- `t5_small_model`

These examples show that direct no-retrieval generation struggled to produce useful function descriptions, while retrieval-based and T5-small outputs showed stronger overlap with the original functional summaries.

## Key Takeaways

- Protein function prediction can be framed as a sequence-to-text generation problem.
- Amino acid sequences can be connected to candidate function descriptions using language-model-based generation.
- Retrieval based on embedding similarity can provide useful context for generation.
- The retrieval-based T5-small result outperformed the no-retrieval T5-small baseline in the reported metrics.
- Generated descriptions should be treated as candidate functional estimates, not experimentally validated annotations.

## Future Work / Next Steps

The slide deck lists three main directions for future work:

### Encodings

- Test ESM2 variants.
- Explore ProsT5.
- Try an ensemble approach.

Different encoders are trained differently, so changing the encoder may affect the quality of the retrieved context and generated summaries.

### Similarity Methods

- Compare PAM.
- Compare BLOSUM.
- Compare e-score.

Different similarity scoring methods could affect retrieval quality and downstream generation outcomes.

### Generation Models

- Explore BioBERT.
- Explore ProLLaMA.
- Explore ProtGPT2.

Domain-specific models could lead to better protein function description outputs.

## Project Status

This repository is an exploratory research project. The notebooks and results document the progression from baseline sequence-to-text generation toward a resource-constrained retrieval-like workflow for protein function description generation.
