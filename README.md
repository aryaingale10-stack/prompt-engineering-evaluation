# Prompt Engineering Evaluation Framework

An end-to-end experimental framework for evaluating prompt-engineering strategies across **one-sentence summarization** and **reading-comprehension question answering (QA)** using structured prompting, decoding parameters, automated evaluation, and visualization.

## Project Overview

This project investigates how different prompt strategies and decoding configurations affect LLM outputs across two NLP tasks:

- One-sentence summarization
- Reading-comprehension question answering

The framework supports Hugging Face inference and includes a deterministic MOCK fallback for reproducible pipeline testing when remote inference is unavailable.

Five prompt-engineering strategies were evaluated:

1. Baseline summarization
2. Role-based summarization
3. Reasoning-inspired summarization
4. Refusal-aware summarization
5. Strict-JSON question answering

## Experimental Design

The generated dataset contains **120 examples**, including:

- 70 summarization examples
- 50 question-answering examples

The final experimental grid evaluated:

- Temperature: `0.0` and `0.3`
- Top-p: `0.9`
- Maximum generation length: `196 tokens`
- 40 examples per applicable task condition
- 400 total generations

The notebook was configured for the open-source Hugging Face model `Qwen/Qwen3.8-27B`.

During the complete experiment, available Hugging Face inference credits were exhausted. Therefore, the final full experimental grid used the deterministic **MOCK fallback**. The reported quantitative results should consequently be interpreted as pipeline-validation results rather than Qwen performance.

## Evaluation Metrics

### Summarization

Summarization quality is evaluated using **lexical overlap** between generated and reference summaries.

### Question Answering

QA performance is evaluated using:

- Exact Match (EM)
- Token-level F1
- Valid JSON rate
- Execution success rate

The QA pipeline parses the structured JSON response and evaluates the extracted `answer` field against the reference answer.

## Results

The final complete MOCK experiment produced **400 successful generations with zero execution errors**.

| Metric | Result |
|---|---:|
| Total Generations | 400 |
| Execution Success Rate | 100% |
| Mean Summarization Lexical Overlap | 0.8104 |
| QA Exact Match | 0.125 |
| QA F1 | 0.125 |
| QA Valid JSON Rate | 100% |

One key observation is that **successful execution and valid output formatting do not necessarily imply answer correctness**. The QA pipeline achieved 100% execution success and JSON validity while EM and F1 remained 0.125.

Because the final grid used MOCK inference, identical results across temperature settings should not be interpreted as evidence that temperature has no effect on real LLMs.

## Project Structure

```text
prompt-engineering-evaluation/
│
├── PA4-DTSC5525-Arya-Ingale.ipynb
│
├── results/
│   ├── outputs_20261008_233921.csv
│   ├── metrics_20261008_233921.csv
│   └── human_eval_sample_20261008_233921.csv
│
└── report/
    └── Prompt_Engineering_Evaluation_Report.pdf
```

## Key Features

- Multi-strategy prompt evaluation
- Summarization and reading-comprehension QA
- Structured JSON output validation
- Hugging Face inference integration
- MOCK fallback for pipeline reliability
- Multiple decoding configurations
- Automatic EM and F1 evaluation
- Lexical-overlap analysis
- Execution-success monitoring
- Human-evaluation sample generation
- Comparative visualizations and heatmaps
- Reproducible CSV experiment artifacts

## Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- Hugging Face Hub / Inference API
- Jupyter Notebook / Google Colab
- JSON

## Repository Artifacts

The `results/` directory contains:

- **outputs** — raw generated responses for each experimental condition
- **metrics** — aggregated automatic evaluation metrics
- **human evaluation sample** — 30 selected examples for qualitative review

The `report/` directory contains the detailed experimental report covering methodology, results, error analysis, ethics, limitations, and practitioner takeaways.

## Limitations

The primary limitation was the Hugging Face inference-credit restriction, which prevented completion of the full experimental grid using the real Qwen model.

The final complete evaluation therefore uses a deterministic MOCK implementation. MOCK outputs are useful for validating the experiment pipeline but should not be used to draw conclusions about real-model prompt sensitivity, temperature effects, diversity, or hallucination behavior.

Future work could repeat the complete experiment with multiple real open-source models and incorporate semantic similarity metrics, factuality evaluation, and expanded human assessment.

## Key Takeaway

> Reliable LLM evaluation requires measuring more than whether a response was successfully generated. Output structure, factual correctness, semantic quality, robustness, and failure behavior should be evaluated separately.
