# Results

This folder contains the final results materials for the protein function prediction project.

## Contents

| File | Description |
|---|---|
| `Results.md` | Short summary of the results folder and the key reported model metrics. |
| `W266_Final_Presentation_DL_KM.pdf` | Final presentation slide deck for the project. |

## Final Presentation

The main results artifact is:

[`W266_Final_Presentation_DL_KM.pdf`](W266_Final_Presentation_DL_KM.pdf)

The slide deck is titled **"Resource Constrained RAG-Like Approach for Protein Function Descriptions"** and was prepared for W266 by Darren Lo and Kamron Mojabe.

The slides summarize:

- the project motivation and compute constraints,
- the retrieval-like workflow,
- the T5 sequence-to-sequence generation approach,
- cosine similarity visualizations,
- quantitative model results,
- qualitative summary examples,
- future work and next steps.

## Workflow Summary

The project workflow uses:

```text
Protein sequence and description examples
        ↓
Cosine similarity retrieval
        ↓
Text processing
        ↓
T5-small tokenization
        ↓
Sequence-to-sequence generation
        ↓
Generated protein function summary
```

The purpose of retrieval is to provide additional functional context from similar proteins before generating a candidate function description.

## Key Reported Results

The slide deck reports the following comparison:

| Model | ROUGE-1 | ROUGE-2 | ROUGE-L | BLEU |
|---|---:|---:|---:|---:|
| no-retrieval-t5-small | 0.123 | 0.021 | 0.089 | 0.031 |
| retrieval-t5-small | 0.454 | 0.285 | 0.429 | 4.743 |

Based on the reported metrics, `retrieval-t5-small` outperformed `no-retrieval-t5-small` across ROUGE-1, ROUGE-2, ROUGE-L, and BLEU.

## Additional Reported Model Comparisons

The presentation also includes a larger comparison table with T5-small and T5-base variants:

| Model | Trainable Parameters | ROUGE-1 | ROUGE-2 | ROUGE-L | BLEU | Inference Time (sec) |
|---|---:|---:|---:|---:|---:|---:|
| t5-small-FE | 41.6M | 0.735 | 0.528 | 0.692 | 43.923 | 968.11 |
| t5-small-HFE | 51.1M | 0.758 | 0.580 | 0.720 | 51.393 | 987.23 |
| t5-small | 60.5M | 0.781 | 0.620 | 0.750 | 56.392 | 996.58 |
| t5-base-FE | 139M | 0.843 | 0.719 | 0.817 | 66.579 | 1543.63 |
| t5-base-HFE | 180.4M | 0.869 | 0.769 | 0.853 | 73.574 | 1542.54 |
| t5-base | 222.9M | 0.890 | 0.809 | 0.876 | 78.015 | 1558.93 |

The same slide also reports BLEU efficiency comparisons:

| Model | BLEU / Trainable Parameter | BLEU / Inference Time |
|---|---:|---:|
| t5-small-FE | 1.060 | 0.0454 |
| t5-small-HFE | 1.006 | 0.0521 |
| t5-small | 0.932 | 0.0566 |
| t5-base-FE | 0.479 | 0.0431 |
| t5-base-HFE | 0.4078 | 0.0477 |
| t5-base | 0.350 | 0.0500 |

## Qualitative Summary Examples

The slide deck includes example original summaries and predicted summaries for three models:

- `no_retrieval_baseline_t5`
- `retrieval_baseline_t5`
- `t5_small_model`

The examples show that the no-retrieval baseline produced a less biologically useful predicted summary, while the retrieval-based and T5-small examples showed stronger overlap with the original summaries.

## Future Work / Next Steps

The slides list three main future work areas.

### Encodings

- ESM2 variants
- ProsT5
- Ensemble approach

### Similarity

- PAM
- BLOSUM
- e-score

### Generation Models

- BioBERT
- ProLLaMA
- ProtGPT2

The next stage of the project would be to compare how different encoders, similarity scoring methods, and domain-specific generation models affect the quality of generated protein function descriptions.
