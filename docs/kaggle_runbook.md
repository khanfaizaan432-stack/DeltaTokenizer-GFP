# Kaggle Runbook

## Fresh 12-hour session

Use the master notebook:

```text
notebooks/DeltaTokenizer_GFP_Master_v5_3_validation.ipynb
```

Recommended fresh-session flags:

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

Expected runtime from prior successful runs: about 5.5-5.8 hours.

## After a successful run

Preserve:

```text
/kaggle/working/delta_tokenizer_v5
```

Important generated files:

```text
cache/*.pt
adapter.pt
no_aux_adapter.pt
delta_tokenizer_results_with_no_aux.csv
delta_tokenizer_pos_imp_no_aux_results.csv
no_aux_fitness_eval.csv
split_distribution_summary.csv
delta_mamba_performance_by_mutation_bin.csv
figures/*.png
```

Save these as a Kaggle Dataset or external artifact. Do not commit the `.pt` files to GitHub.

## Multi-seed tokenizer-only run

After saving the embeddings/checkpoints as Kaggle inputs, run tokenizer heads across seeds while loading the cached adapter outputs. The goal is to estimate variance without retraining ESM/Mamba.
