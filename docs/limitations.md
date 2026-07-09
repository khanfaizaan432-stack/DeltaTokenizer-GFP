# Limitations

## Single-seed results

The current strongest results are from a single seed. The no-aux pos_imp tokenizer outperforms all tokenizer baselines in the v5.3 run, but multi-seed variance is still required before making strong statistical claims.

## DMS circularity in auxiliary variant

The DMS-aux adapter's position-importance head is explicitly trained with DMS sensitivity as an auxiliary target. Therefore, high correlation between DMS-aux `pos_imp` and DMS is not independent biological discovery.

The no-aux bridge experiment addresses this by training the adapter without DMS auxiliary loss. In the v5.3 run, no-aux `pos_imp` had `rho = -0.1435` against DMS sensitivity and produced the best tokenizer test Spearman.

## Test split difficulty

The validation and test splits are not equally difficult. The test set contains substantially higher-distance mutants from the GFP parent sequence, with mean Hamming distance 5.41 versus ~2.4 for train/val.

This explains why overall test Spearman is lower than validation Spearman. Per-bin analysis should be reported alongside headline metrics.

## Not a drug-discovery claim

This project is about protein representation compression and GFP fitness prediction. It should not be framed as a drug-discovery system without additional task-specific validation on drug-relevant datasets.

## Large artifacts excluded

ESM caches and adapter checkpoints are not tracked in Git because they are large generated files. They should be stored as Kaggle outputs or datasets.
