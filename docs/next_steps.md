# Next Steps

## 1. Multi-seed tokenizer-only validation

Do not add new architecture yet. The next experiment is variance estimation:

```text
same frozen ESM caches
same trained adapter / no-aux adapter
same tokenizers
3 seeds for tokenizer heads only
```

Target table:

| Tokenizer | Test Spearman mean ± std |
|---|---:|
| random 64 | TBD |
| uniform 64 | TBD |
| full 256 | TBD |
| DMS 64 | TBD |
| DMS-aux pos_imp 64 | TBD |
| no-aux pos_imp 64 | TBD |

## 2. Preserve Kaggle artifacts

Save `/kaggle/working/delta_tokenizer_v5` as a Kaggle Dataset or external artifact. Especially preserve:

```text
cache/*.pt
adapter.pt
no_aux_adapter.pt
figures/*.png
*.csv
```

## 3. Final clean report

After multi-seed validation, create a clean final report with:

- problem statement
- dataset and preprocessing
- model architecture
- baselines
- main fitness results
- tokenizer compression results
- no-aux bridge result
- val/test difficulty diagnostics
- limitations
