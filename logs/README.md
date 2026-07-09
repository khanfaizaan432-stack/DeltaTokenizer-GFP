# Run logs

Full Kaggle logs were generated during development, but the main values are captured in `results/` and the README.

Important completed runs:

| Run | Purpose | Status |
|---|---|---|
| v4.2 | DeltaMamba baseline + ablations | completed |
| v5.1 | first DeltaTokenizer run | completed |
| v5.2 | FullTokenHead same-head baseline | completed |
| v5.3 master | no-aux pos_imp bridge + diagnostics | completed |

Key v5.3 outputs:

- DeltaMamba DMS-aux test Spearman: 0.677679
- DeltaMamba no-aux test Spearman: 0.679027
- FullTokenHead 256 test Spearman: 0.593935
- DMS tokenizer 64 test Spearman: 0.608109
- DMS-aux pos_imp tokenizer 64 test Spearman: 0.600682
- no-aux pos_imp tokenizer 64 test Spearman: 0.630232
- no-aux pos_imp vs DMS Spearman rho: -0.143491

Large raw logs and `.pt` artifacts should be stored externally as Kaggle outputs/datasets rather than committed directly.
