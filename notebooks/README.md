# Notebooks

This repository currently contains:

- `DeltaTokenizer_GFP_Project_Summary.ipynb` — lightweight notebook with the key results and run configuration.

The full executable master notebook from the ChatGPT/Kaggle workflow is:

```text
DeltaTokenizer_GFP_Master_v5_3_validation.ipynb
```

It should be copied into this folder from the saved artifact when updating the repo locally. It is intentionally kept separate from large generated `.pt` caches/checkpoints, which should not be committed to GitHub.

## Master notebook sections

The master notebook contains:

1. setup/config
2. dataset loading and CDS→protein translation
3. ESM-2 cache generation/loading
4. ESM Ridge baseline
5. DeltaMamba DMS-aux adapter
6. compression benchmark
7. FullTokenHead baseline
8. DeltaTokenizer random/uniform/DMS/pos_imp
9. no-aux pos_imp bridge experiment
10. val/test diagnostics
11. optional multi-seed tokenizer flags
