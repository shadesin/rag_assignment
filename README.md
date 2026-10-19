# RAG assignment: baseline vs. re-ranking

One self-contained notebook compares dense retrieval with Cross-Encoder re-ranking on a complex PDF. Both pipelines start with the same ten chunks and give three passages to the same local answer model.

- Baseline: normalized dense embeddings → FAISS cosine search → Top 3 → Answer 1.
- Re-ranking: same Top 10 → joint question/passage Cross-Encoder → Top 3 → Answer 2.
- Source: **NIST SP 800-63B**, June 2017 with updates through March 2020 and its withdrawal cover; 80 physical PDF pages. This withdrawn edition is a fixed historical experiment corpus, not current security guidance.
- Models: `sentence-transformers/all-MiniLM-L6-v2`, `cross-encoder/ms-marco-MiniLM-L-6-v2`, and `Qwen/Qwen2.5-1.5B-Instruct`.
- No API keys or paid model service required.

## Run

The prepared project is already in this local folder. The clone command below applies only after the owner chooses to publish it; an empty GitHub repository will not contain these files.

Use Python 3.11 or 3.12. Allow approximately 4 GB for model downloads plus space for PyTorch and the environment. A GPU with sufficient memory helps; the experiment also supports CPU. A Mac with 16 GB of memory and MPS is suitable for this small answer model.

```bash
git clone https://github.com/shadesin/rag_assignment.git
cd rag_assignment
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python scripts/run_notebook.py
```

The runner executes the notebook with the active Python environment and saves cell outputs as it proceeds. The first run downloads fixed model revisions into `.cache/huggingface/`. To rerun using only cached models:

```bash
python scripts/run_notebook.py --offline
```

You can also open `rag_assignment.ipynb` in VS Code or Jupyter, select this virtual environment, and run all cells from the repository root. The notebook holds all extraction, chunking, retrieval, re-ranking, generation, and experiment code; the Python script only runs it.

## Experiment and artifacts

The notebook verifies the included PDF checksum, removes front matter and running headers, and packs sentences into page-local chunks of at most 220 embedding tokens with up to 40 tokens of overlap. It normalizes vectors and uses an exact FAISS inner-product index, equivalent to cosine similarity.

The 32 exploratory questions cover keyword distractors, exceptions, negation, and document-specific distinctions. Critical source phrases and pages are defined before retrieval. All exploratory rankings are retained, including evidence missing from the Top 10 and regressions. Positive case studies are selected transparently from observed promotions, with non-improving controls. This is a stress-test demonstration, not an unbiased general accuracy estimate. A critical-phrase label is not an exhaustive annotation of all potentially relevant chunks.

Both answers use the same system instructions, passage formatting, model, and greedy decoding settings. Expected answers, relevance labels, and rank scores are never shown to the answer model. Raw prompts and outputs are saved for inspection.

| File | Purpose |
| --- | --- |
| `rag_assignment.ipynb` | Main implementation with executed outputs |
| `data/nist_sp_800_63b.pdf` | Frozen knowledge-source PDF |
| `outputs/document.json`, `pages.json`, `chunks.json` | Source metadata and traceable corpus |
| `outputs/queries.json` | Questions, critical evidence, and source-derived answer checklists |
| `outputs/exploration.json` | All ten candidates, passage texts, and both rankings for every question |
| `outputs/rank_summary.csv`, `candidate_ranks.csv` | All observed rank changes |
| `outputs/selected_cases.json` | Transparent selection of final answer comparisons |
| `outputs/answers.json`, `evidence.md` | Actual LLM answers, prompts, contexts, and generation measurements |
| `outputs/latency_samples.csv`, `latency_summary.csv` | Raw repeated retrieval timings and medians |
| `outputs/comparison.csv` | Selected-case ranks, contexts, answers, and latency side by side |
| `outputs/qualitative_review.json` | Separate inspection of actual answers and context limitations |
| `outputs/environment.json`, `requirements-tested.txt` | Recorded hardware, package versions, model revisions, settings, and setup times |
| `outputs/validation.json` | Runtime checks and experiment counts |
| `REPORT.pdf` | Complete three-page report |

Physical PDF page numbers are one-based; body printed page numbers are 11 lower. Each chunk records both. The knowledge corpus starts at PDF page 12 and retains subsequent informative sections and references as realistic distractors.

## Timing and validation

Five warmed retrieval measurements are recorded for each selected question. Baseline latency includes question embedding and dense search; re-ranking latency includes the same retrieval plus scoring and sorting all ten candidates. PDF extraction, model loading, and document indexing are excluded and recorded separately where measured. Retrieval uses CPU with four threads; answer generation uses CUDA, MPS, or CPU as available.

Generation timings are single observations, measured with accelerator synchronization where relevant. End-to-end columns are explicitly **estimates** formed from median retrieval plus the measured generation call. They are not repeated full-pipeline benchmarks.

The notebook checks vector normalization, chunk and pair token limits, stable IDs, source phrases, exact candidate reuse, shared prompt structure, non-empty/untruncated answers, timing arithmetic, and at least two promotions of critical evidence into the Top 3. Read the answers and original source before concluding that a retrieval improvement fixed an answer.

## Recorded run

The executed notebook passed its runtime checks on an Apple Silicon Mac using CPU retrieval and MPS answer generation. It indexed **215 chunks**, explored **32 questions**, and generated both answers for **8 selected cases**. Across the exploratory questions, critical evidence entered the Top 3 in **6 cases**, left it in **1 case**, and was absent from the initial Top 10 in **7 cases**.

| Case | Critical-evidence rank | Observed answer behavior |
| --- | --- | --- |
| `q11`: out-of-band deadline | 6 → 1 | Answer 2 supplies the ten-minute deadline missing from Answer 1. |
| `q02`: monthly password changes | 7 → 3 | Re-ranking supplies the periodic-change rule and compromise exception; Answer 1's claim that compromise does not mandate a change is corrected, although Answer 2 could state the mandatory exception more precisely. |
| `q28`: first-pet password prompt | 5 → 1 | Answer 2 uses the explicit prohibition; the baseline had only general password-predictability discussion. Both give the same yes/no conclusion. |
| `q06`, `q13`, `q29` | 6 → 1; 5 → 2; 4 → 1 | Relevant evidence is promoted, but the small LLM still misreads or omits important conditions. |
| `q25`: biometric secrecy | 1 → 5 | Re-ranking removes the direct supporting passage and degrades the answer's grounding. |

For selected questions, median baseline retrieval took roughly **5–6 ms**, while retrieval plus re-ranking took **90–102 ms**. See the raw samples and per-query medians for the exact measurements. These are this machine's observed timings, not a speed guarantee. The separate qualitative review records remaining answer mistakes and missing citations rather than treating every retrieval promotion as an answer improvement.

## Source and technical references

- [NIST source PDF](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-63b.pdf), included with attribution and its withdrawal notice. Source SHA-256 is recorded in the notebook and `outputs/document.json`.
- [NIST publication record](https://www.nist.gov/publications/digital-identity-guidelines-authentication-and-lifecycle-management).
- [Sentence Transformers Bi-Encoder API](https://www.sbert.net/docs/package_reference/sentence_transformer/model.html).
- [Sentence Transformers Cross-Encoder API](https://www.sbert.net/docs/package_reference/cross_encoder/model.html).
- [Qwen model card](https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct).

The original PDF remains a NIST publication; no ownership of it or of model weights is claimed. Model weights are downloaded separately and are not committed to the repository.

## Submission

The report covers architecture, actual rank/answer changes, measured latency, scaling, and limitations.
