# Ablations & Rejected Approaches

Approaches explored during development that didn't make it into the final
pipeline, and the reasoning behind each call.

## Retrieval-based prediction
Implemented when we were working with a dataset with only 19k images, slightly lower performance - we expect this happened beacuse there were not enough images in every geocells for retrieval-based prediction to work effectively. We plan to implement this along with top-k geocell next after we trained the head using dataset of sixe 126k images. 

## CLIP + DINOv2 multimodal fusion
Concatenated CLIP and DINOv2 embeddings as a richer feature vector. Result
came out worse than CLIP alone - we expect this happened because of unnormalized
concatenation across two different embedding spaces and  higher overfitting risk due to doubled embedding dimensionality especially at this training-set size.

## SigLIP vs. CLIP as the backbone
SigLIP preformed slightly worse than CLIP and took longer for getting embedding.

## Naive confidence-based radius scaling
First attempt: set the radius straight from the model's confidence score. That didn't work - confidence barely tracked actual accuracy, and some predictions missed by over 2,500km even when the model looked very sure of itself. This is why the radius is instead based on how spread out the model's guesses are across all geocells, not just how confident it is in its single top pick.

## Post-hoc radius multiplier (tried, then removed)
Blending the top-k cells' centroids by their probabilities, rather than a hard argmax, was flagged as a promising direction - higher expected payoff than retrieval, since it directly targets the same inter-cell error mode - but wasn't implemented before this repo was finalized. Worth trying as a follow-up: predicted point = probability-weighted average of the top-k centroids instead of the single argmax cell.

## Top-k geocell blending (not implemented)
Blending the top-k cells' centroids by their probabilities, rather than a
hard argmax. Worth trying as a
follow-up: predicted point = probability-weighted average of the top-k
centroids instead of the single argmax cell.

