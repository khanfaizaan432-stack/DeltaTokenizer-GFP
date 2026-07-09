# Methodology

## Dataset

The experiment uses the `InstaDeepAI/true-cds-protein-tasks` fluorescence task. The source examples are CDS/DNA-like sequences, so the notebook translates sequences to protein before passing them to ESM-2.

Observed split sizes in the v5.3 run:

- train: 21,464
- validation: 5,366
- test: 27,217
- translated train sequences: 21,464 / 21,464
- protein length: min = 237, median = 237, max = 237

## Representation

Frozen ESM-2 150M produces residue embeddings with shape `[B, 256, 640]`. Embeddings are cached as float16 tensors to reduce repeated runtime.

## DeltaMamba adapter

A lightweight Mamba adapter is trained on frozen ESM embeddings. The adapter outputs:

1. a sequence-level fitness prediction;
2. a per-position importance score.

Two adapter variants are important:

- `DeltaMamba DMS-aux`: trained with fluorescence loss plus DMS sensitivity auxiliary loss.
- `DeltaMamba no-aux`: trained only on fluorescence loss.

The no-aux variant is used to test whether task-learned position importance can support token compression without copying DMS supervision.

## DeltaTokenizer

DeltaTokenizer selects K anchor residues and performs local-window pooling around each anchor. In the v5.3 master run:

- K = 64
- window = ±2 residues
- tokenizers compared: random, uniform, DMS, DMS-aux pos_imp, no-aux pos_imp
- same-head `FullTokenHead` baseline uses all valid ESM tokens.

## Evaluation

Main metrics:

- Spearman correlation
- Pearson correlation
- MSE

Main comparisons:

1. ESM mean-pool Ridge baseline vs DeltaMamba.
2. DeltaTokenizer K=64 vs FullTokenHead K=256.
3. no-aux pos_imp tokenizer vs DMS/aux/random/uniform baselines.
4. train/val/test split diagnostics by fluorescence distribution and Hamming distance to parent.
