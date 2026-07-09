# Results Summary

## Fitness prediction

| Model | Val Spearman | Test Spearman | Test Pearson | Test MSE |
|---|---:|---:|---:|---:|
| ESM mean-pool Ridge | 0.6267 | 0.5834 | 0.6204 | 1.2378 |
| DeltaMamba DMS-aux | 0.8019 | 0.6777 | 0.9049 | 0.2864 |
| DeltaMamba no-aux | 0.7977 | 0.6790 | 0.9011 | 0.2969 |

## Tokenizer compression

| Tokenizer | Tokens | Val Spearman | Test Spearman | Retained vs FullTokenHead |
|---|---:|---:|---:|---:|
| FullTokenHead | 256 | 0.6092 | 0.5939 | 1.000 |
| RandomTokenizer | 64 | 0.5726 | 0.5757 | 0.969 |
| UniformTokenizer | 64 | 0.5986 | 0.5916 | 0.996 |
| DMS-guided tokenizer | 64 | 0.6091 | 0.6081 | 1.024 |
| DMS-aux pos_imp tokenizer | 64 | 0.6223 | 0.6007 | 1.011 |
| **No-aux pos_imp tokenizer** | **64** | **0.6770** | **0.6302** | **1.061** |

## Split diagnostics

| Split | n | Fluorescence mean | Hamming mean | Hamming median |
|---|---:|---:|---:|---:|
| train | 21,464 | 3.184 | 2.42 | 2 |
| val | 5,366 | 3.160 | 2.47 | 2 |
| test | 27,217 | 2.083 | 5.41 | 5 |

## Per-bin DeltaMamba performance

| Split | Mutation bin | n | Spearman |
|---|---|---:|---:|
| test | 3-5 | 16,974 | 0.7969 |
| test | 6-10 | 10,018 | 0.3117 |
| test | >10 | 225 | 0.0714 |
| val | 1 | 97 | 0.6867 |
| val | 2 | 2,627 | 0.7444 |
| val | 3-5 | 2,642 | 0.8395 |

## Interpretation

The no-aux pos_imp tokenizer is the current strongest compression result. It was trained without DMS auxiliary supervision and had low/negative DMS correlation (`rho = -0.1435`), which weakens the circularity critique that applied to the DMS-aux importance head.

The result remains single-seed. Multi-seed tokenizer-only validation is the next required experiment.
