# DeltaTokenizer-GFP

**DeltaTokenizer-GFP** is a research prototype for task-aware compression of protein language model embeddings.

The project trains a lightweight **DeltaMamba** adapter over frozen **ESM-2 150M** residue embeddings for GFP fluorescence / fitness prediction, then tests whether a **DeltaTokenizer** can compress protein representations from 256 ESM tokens to 64 local-window tokens while preserving predictive signal.

## Core idea

```text
CDS/DNA sequences
→ translate to protein
→ frozen ESM-2 residue embeddings [L, 640]
→ DeltaMamba adapter
→ learned residue importance
→ DeltaTokenizer K=64 local-window token compression
→ fitness prediction head
```

## Current headline result

Single-seed v5.3 master run:

| Method | Tokens | Val Spearman | Test Spearman | Notes |
|---|---:|---:|---:|---|
| ESM mean-pool Ridge | pooled | 0.6267 | 0.5834 | baseline |
| DeltaMamba DMS-aux | 256 | 0.8019 | 0.6777 | strongest main adapter with DMS auxiliary loss |
| DeltaMamba no-aux | 256 | 0.7977 | 0.6790 | fluorescence-only adapter; slightly better test result |
| FullTokenHead | 256 | 0.6092 | 0.5939 | same head, all valid ESM tokens |
| RandomTokenizer | 64 | 0.5726 | 0.5757 | random local-window tokens |
| UniformTokenizer | 64 | 0.5986 | 0.5916 | evenly spaced local-window tokens |
| DMS-guided tokenizer | 64 | 0.6091 | 0.6081 | DMS-selected tokens |
| DMS-aux pos_imp tokenizer | 64 | 0.6223 | 0.6007 | importance from DMS-aux adapter |
| **No-aux pos_imp tokenizer** | **64** | **0.6770** | **0.6302** | best tokenizer result; not trained to copy DMS |

The no-aux position-importance tokenizer retained **106.1%** of the same-head FullTokenHead test Spearman in this run and had low/negative correlation with DMS sensitivity (`rho = -0.1435`), so the best tokenizer is not simply copying DMS.

## Why this matters

The earlier DMS-aux importance head had a circularity problem: if a model is trained to predict DMS sensitivity, then high correlation between predicted importance and DMS is not independent biological discovery.

The v5.3 bridge experiment addresses this by training a **no-aux DeltaMamba adapter** using only fluorescence labels, extracting its position importance, and using that for DeltaTokenizer compression. In the current single-seed run, this no-aux tokenizer outperformed random, uniform, DMS-guided, DMS-aux importance, and same-head full-token baselines.

## Split diagnostics

The test split is substantially harder than train/validation:

| Split | n | Fluorescence mean | Hamming mean to parent | Median Hamming |
|---|---:|---:|---:|---:|
| train | 21,464 | 3.184 | 2.42 | 2 |
| val | 5,366 | 3.160 | 2.47 | 2 |
| test | 27,217 | 2.083 | 5.41 | 5 |

Per-bin DeltaMamba performance shows the overall test drop is mostly caused by high-order mutants:

| Split | Mutation bin | n | Spearman |
|---|---|---:|---:|
| test | 3-5 | 16,974 | 0.7969 |
| test | 6-10 | 10,018 | 0.3117 |
| test | >10 | 225 | 0.0714 |
| val | 2 | 2,627 | 0.7444 |
| val | 3-5 | 2,642 | 0.8395 |

## Repository layout

```text
notebooks/
  DeltaTokenizer_GFP_Master_v5_3_validation.ipynb
  archive/
    older experimental notebooks

results/
  key_metrics.csv
  split_distribution_summary.csv
  per_bin_performance.csv
  results_summary.md

docs/
  methodology.md
  limitations.md
  kaggle_runbook.md

logs/
  selected Kaggle run logs
```

## Running on Kaggle

The master notebook is designed for a Kaggle GPU session with a 12-hour limit. The default v5.3 run completed in about 5.5-5.8 hours in prior runs.

Use the master notebook:

```text
notebooks/DeltaTokenizer_GFP_Master_v5_3_validation.ipynb
```

Default recommended flags for a fresh session:

```python
RUN_DELTA_TOKENIZER = True
RUN_FULL_TOKEN_HEAD = True
RUN_NO_AUX_POSIMP_TOKENIZER = True
RUN_TOKENIZER_MULTI_SEED = False
RUN_VAL_TEST_DIAGNOSTICS = True
RUN_ADAPTER_ABLATIONS = False

LOAD_EXISTING_MAIN_ADAPTER = False
LOAD_EXISTING_NO_AUX_ADAPTER = False
FORCE_RETRAIN_NO_AUX = True
```

After a successful run, preserve `/kaggle/working/delta_tokenizer_v5` as a Kaggle Dataset or external artifact. Do **not** commit large `.pt` caches/checkpoints to GitHub.

## What is intentionally not committed

Large generated artifacts are excluded:

- ESM cache `.pt` files
- trained adapter checkpoints `.pt`
- large generated output folders
- Kaggle runtime directories

These should be saved as Kaggle outputs/datasets, not in Git.

## Status

Current status: **strong single-seed research prototype**.

Next experiment before making strong statistical claims:

```text
multi-seed tokenizer-only validation
```

The key remaining question is whether the no-aux pos_imp tokenizer remains top or tied-top across 3 seeds.

## Safe claim

> DeltaTokenizer compresses frozen ESM-2 protein representations into 64 task-selected local-window tokens. On GFP fitness prediction, a no-aux Mamba-derived tokenizer retained and exceeded same-head full-token performance in a single-seed run while outperforming random, uniform, DMS-guided, and DMS-aux importance tokenizers. Split diagnostics show the test set is substantially harder than validation because it contains higher-distance mutants.

## Caveat

The main result is currently single-seed. Treat small tokenizer gaps as promising until multi-seed variance is available.
