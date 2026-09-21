# Ablations & Rejected Approaches

Approaches explored during development that didn't make it into the final
pipeline, and the reasoning behind each call.

## Retrieval-based prediction
Tried scoring candidate locations by nearest-neighbor similarity in embedding
space, as an alternative to pure geocell classification. Consistently
underperformed plain centroid prediction. Root cause: per-cell training pools
are sparse at this dataset size (a few dozen images per cell, median), and
most prediction errors are *inter*-cell (wrong cell entirely) rather than
*intra*-cell — retrieval scoped to the top-1 predicted cell structurally
cannot fix a wrong-cell prediction. Dropped in favor of the classification
head alone.

## CLIP + DINOv2 multimodal fusion
Concatenated CLIP and DINOv2 embeddings as a richer feature vector. Result
came out roughly tied with CLIP alone — no measurable gain to justify the
added complexity (doubled embedding dimensionality, higher overfitting risk
at this training-set size, and similarity distortion from unnormalized
concatenation across two different embedding spaces). Dropped.

## SigLIP vs. CLIP as the backbone
SigLIP slightly outperformed CLIP as a standalone backbone in earlier
experiments (~626km vs. ~703km median validation error), plausibly because
SigLIP's sigmoid-based per-pair training objective produces embeddings whose
global pairwise distances are more meaningful than CLIP's batch-normalized
softmax contrastive loss. This repo standardizes on CLIP alone to keep a
single, clean pipeline rather than maintaining two backbones end-to-end —
swapping in SigLIP (`google/siglip-so400m-patch14-384`) as a drop-in backbone
is a natural, low-risk follow-up experiment on this same codebase.

## Naive confidence-based radius scaling
Initial approach assumed the model's own softmax confidence would track
prediction accuracy closely enough to derive the radius directly from it.
Measured correlation was weak (Spearman ρ ≈ -0.23 between predicted-cell
probability and actual error), and a meaningful fraction of validation
predictions were catastrophic misses (>2500km) even at high stated
confidence. This motivated the current design: deriving the radius from the
*probability-weighted geographic spread* of the full distribution rather than
just the top prediction's confidence, plus a separate post-hoc calibration
pass against measured coverage (see `metrics.md`).

## Top-k geocell blending (not implemented)
Blending the top-k cells' centroids by their probabilities, rather than a
hard argmax, was flagged as a promising direction — higher expected payoff
than retrieval, since it directly targets the same inter-cell error mode —
but wasn't implemented before this repo was finalized. Worth trying as a
follow-up: predicted point = probability-weighted average of the top-k
centroids instead of the single argmax cell.

