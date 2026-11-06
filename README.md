# LLM feature-generator variation study

Code, data, figures, and manuscript sources for *Large Language Model Interchangeability in Actuarial Feature Engineering*. All required inputs are stored locally; no submission archives are needed.

## Contents

- `s0_get_data.ipynb`: verifies the public claim files, prepares the data, and reproduces the peril-only baseline.
- `s1_generate_labels.ipynb`: defines the original inference experiments and reuses the saved classifications and compatibility checks.
- `s2_analyze.ipynb`: reproduces the core regressions, Figure 2, restored incident examples, and supplementary full-description diagnostics.
- `s3_glm_extensions.ipynb`: adds categorical-label sensitivities, coefficient standard errors, ranking and dollar diagnostics, and within-/between-model prediction comparisons.
- `s4_prompt_sensitivity.ipynb`: compares prompts, input order, and batch size using the complete cached experiment.
- `s5_agreement_diagnostics.ipynb`: adds weighted agreement, clustered intervals, alignment, fixed-input rerun references, illustrative claims routing, and the current Figure 1.
- `data/`: raw claims, prepared data, frozen classifications, prompts, and inference audit records.
- `tables/`: reference benchmarks and generated analytical tables and predictions.
- `figures/`: data overview; `paper_naaj/figures/`: manuscript and diagnostic PDF figures.
- `paper_naaj/`: author/anonymous manuscript wrappers and PDFs, bibliography, style, main text, and one `appendix.tex`.
- `manuscript_sections.py`: updates notebook-owned passages in the main text and appendix while preserving other text.
- `paper_reporting.py`: selects the compact primary appendix from the complete saved analysis tables.
- `SUPPLEMENT.md`: indexes full prompts, exhaustive comparisons, and specialized diagnostics retained in the repository.

Modal-label ties use maximum frequency, then distance to the median encoded label, then the earliest numbered run. The analyses verify frozen input hashes before fitting.

## Run the analysis

The tested environment is Python 3.12. Install the pinned dependencies and register a notebook kernel:

```sh
python3.12 -m venv .venv
.venv/bin/python -m pip install -r requirements.txt
.venv/bin/python -m ipykernel install --user --name llm-risk-variability
```

From the repository root, execute the notebooks in order:

```sh
for notebook in s[0-5]_*.ipynb; do
  .venv/bin/jupyter nbconvert --to notebook --execute --inplace "$notebook" \
    --ExecutePreprocessor.kernel_name=llm-risk-variability \
    --ExecutePreprocessor.timeout=900
done
```

The supplied data and complete inference caches allow reproduction without API keys or new requests. Notebooks 3–5 can also run independently. Notebook 2 produces Figure 2 and supplementary plots; Notebook 5 is the sole writer of Figure 1. Notebooks 3–4 include inline diagnostic plots of the published results without adding publication figures. Each notebook identifies the paper sections and tables it supports.

Generating missing classifications requires `OPENAI_API_KEY`, `GOOGLE_API_KEY`, and/or `DEEPINFRA_API_KEY` in a local `.env`, depending on the providers with pending requests. The credential file is ignored by Git. Cached classifications and raw inference records are retained to avoid repeating paid inference.