# Model Performance Metrics

Computed from a 12,606-image validation split - images from Streetview by country data set in kaggle (link: https://www.kaggle.com/datasets/sylshaw/streetview-by-country), using the reference model
(CLIP backbone, 2005-cell geocell scheme — the same `head.pt` /
`geocell_summary.csv` shipped in `reference/`).

## Headline results

| Metric | Value |
|---|---|
| Median haversine error | 173.3 km |
| Mean haversine error | 837.0 km |
| Country accuracy | 71.8% |
| Coverage (error ≤ predicted radius) | 70.3% |
| Median predicted radius | 330.6 km |

Coverage here is the radius exactly as produced by the model's `radius_a`/
`radius_b` — no post-hoc scaling is applied (see `ablations.md` for why a
radius multiplier was tried and then removed).

## Error distribution

| Percentile | Error (km) |
|---|---|
| 10th | 8.7 |
| 25th | 46.3 |
| 50th (median) | 173.3 |
| 75th | 489.0 |
| 90th | 1,413.2 |
| 95th | 3,747.6 |
| 99th | 15,067.8 |

The gap between median (173 km) and mean (837 km) error is real, not a
computation artifact — a small share of predictions miss badly (worst case:
19,539 km, essentially antipodal), which pulls the mean far above the
median. This matches the "catastrophic misses at high stated confidence"
pattern noted during development (see `ablations.md`) — most predictions are
good, a long tail is very wrong, and the median is the more representative
single number for typical performance.

## Reproducing these numbers

Run the notebook's Section 8 (Evaluation) against the full dataset (not the
`data/sample/` demo set) — see Section 4 for how to point the notebook at a
full dataset or a Kaggle-mounted one.
