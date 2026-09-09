# Designing-for-Reflection-and-Connection-Evaluating-InterLeaf-s-Multi-Stage-AI-Pipeline

This repository contains the code used for the offline evaluation reported in:

**Designing for Reflection and Connection: Evaluating InterLeaf's Multi-Stage AI Pipeline on Chronic Pain Narratives**

The evaluation examines a multi-stage AI pipeline for extracting semantic metadata from naturalistic chronic-pain narratives. The reported benchmark contains 250 posts sampled from the **Reddit Reports of Chronic Pain (RRCP)** dataset introduced by Nunes et al. (2023).

## Repository Contents

### `Interleaf_RRCP_Sampling_and_Preparation.ipynb`

Documents the sampling design used for the frozen 250-post evaluation benchmark and validates the researcher-held sampling record.

The benchmark consists of:

- 200 posts in a naturalistic core sample
- 50 posts in a safety-enriched challenge sample
- one sampled post per author
- no duplicate source IDs or normalized texts
- random seed `20260903`

The notebook also prepares the two-column input expected by the prediction notebook:

- `post_id`
- `post_text`

The notebook documents and validates the frozen study sample. It does not recreate the exact 250-post sample from the complete RRCP corpus unless the original researcher-held sampling workbook is supplied.

### `Interleaf_AI_Pipeline_Predictions_Reddit.ipynb`

Runs AI-pipeline inference only and does not read the human gold annotations.

The pipeline includes:

1. narrative normalization
2. harm and safety screening
3. LLM-based contextual tagging
4. ontology-based keyword tagging
5. embedding similarity matching
6. confidence fusion and final tag selection

Key configuration used in the reported evaluation:

- contextual and safety model: `gemini-2.5-flash`
- embedding model: `gemini-embedding-001`
- safety decision threshold: `0.80`
- ontology similarity threshold: `0.60`
- final tag threshold: `0.60`
- maximum contextual candidates: `15`
- maximum final tags: `5`

The notebook outputs:

`reddit_250_predictions.jsonl`

A Google Gemini API key is required to run this notebook.

### `Interleaf_AI_Pipeline_Evaluation_Reddit.ipynb`

Evaluates saved model predictions against the adjudicated human reference annotations.

It reports:

- **RQ1:** safety detection
- **RQ2:** ontology-based multi-label tagging
- **RQ3:** free-form theme recovery
- **RQ4:** confidence alignment and reliability

Free-form semantic matching uses:

- encoder: `all-MiniLM-L6-v2`
- cosine-similarity threshold: `0.60`

The evaluation notebook does not regenerate predictions.

## Recommended Execution Order

1. `Interleaf_RRCP_Sampling_and_Preparation.ipynb`
2. `Interleaf_AI_Pipeline_Predictions_Reddit.ipynb`
3. `Interleaf_AI_Pipeline_Evaluation_Reddit.ipynb`

File paths in the notebooks may need to be adjusted to match the user's local or Google Drive directory structure.

## Data Availability and Privacy

The source corpus used in this study is the Reddit Reports of Chronic Pain (RRCP) dataset:

> Nunes, D. A. P., Ferreira-Gomes, J., Neto, F., & Martins de Matos, D. (2023). *Modeling Chronic Pain Experiences from Online Reports Using the Reddit Reports of Chronic Pain Dataset*. Information, 14(4), 237. https://doi.org/10.3390/info14040237

The exact 250-post evaluation dataset, sampling workbook, and adjudicated annotation files are **not publicly released**.

Although the source material was collected from publicly accessible Reddit content, the study sample contains user-generated health narratives and source metadata. Verbatim social-media text can remain searchable and may allow individual posts or users to be re-identified even after direct identifiers are removed.

To reduce unnecessary redistribution of sensitive health-related narratives and re-identification risk, this repository releases the sampling, inference, and evaluation code without redistributing the sampled Reddit post text.

## Researcher-Held Files

The following files are used internally for an exact reproduction of the reported benchmark but are not included in the public repository:

- the frozen 250-post sampling workbook
- the prepared 250-post prediction input
- the adjudicated gold-annotation workbook
- saved model predictions, unless released separately

Researchers without these files can inspect and adapt the pipeline and evaluation procedures, but cannot reproduce the exact benchmark-level results from this repository alone.

## Reproducibility Notes

The prediction stage uses hosted Gemini models. Outputs may vary if the provider changes model behavior or service implementation over time. The model identifiers and thresholds used in the reported evaluation are therefore stated explicitly in the manuscript and notebooks.

The safety-threshold sweep reported in the evaluation notebook is a post hoc diagnostic analysis. The pre-specified threshold used for the primary safety result is `0.80`.

## Competing Interests

The authors declare no competing interests.

## Citation

If you use this code, please cite the InterLeaf paper and the original RRCP dataset paper.
