# Results

This folder contains the final project results for the protein function prediction work.

## Main Results Artifact

- `W266_Final_Presentation_DL_KM.pdf` - final slide deck summarizing the resource-constrained RAG-like approach for protein function descriptions.

The slide deck includes:

- project motivation and compute constraints,
- the retrieval and T5 sequence-to-sequence workflow,
- cosine similarity visualizations,
- quantitative model results,
- qualitative summary examples,
- future work and next steps.

## Key Reported Results

| Model | ROUGE-1 | ROUGE-2 | ROUGE-L | BLEU |
|---|---:|---:|---:|---:|
| no-retrieval-t5-small | 0.123 | 0.021 | 0.089 | 0.031 |
| retrieval-t5-small | 0.454 | 0.285 | 0.429 | 4.743 |

The reported slide results show that retrieval-t5-small outperformed no-retrieval-t5-small on the listed metrics.

## Future Work Listed in the Slides

- Encodings: ESM2 variants, ProsT5, ensemble approach.
- Similarity: PAM, BLOSUM, e-score.
- Generation models: BioBERT, ProLLaMA, ProtGPT2.
