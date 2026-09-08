# Biohub Cell Tracking

## Competition

- Task: detect cells, link them over time, and recover divisions in 3D zebrafish microscopy.
- Final deadline: September 29, 2026 at 23:59 UTC (September 30 at 08:59 JST).
- Team merger deadline: September 22, 2026 at 23:59 UTC.
- Submission: Kaggle notebook only, internet off, maximum 12 hours, `submission.csv` required.
- Allowance: five submissions per day; two final submissions may be selected.
- Metric: adjusted edge Jaccard + `0.1 * division Jaccard`.
- Public leaderboard leader observed August 31, 2026: `0.962`.

## Workspace

- Foundation: Biohub's official `tracking-cellmot` baseline at commit `075fc5f5a52d11077f9dc2b074644618f26939e2`.
- Environment: `.venv` with Python 3.12 and the baseline dependencies.
- Verification: 107 self-contained tests pass; 16 data-backed checks await the training data.
- Submission conversion: the official sample CSV completes a CSV -> GEFF -> CSV round trip with 20 rows across four datasets.

## Next

1. Download the competition training data into `data/train/`.
2. Create embryo-disjoint folds; samples sharing the prefix before `_` must stay in the same fold.
3. Reproduce the official baseline locally and record edge, division, and combined CV scores.
4. Tune detection count/threshold and linking/division costs against CV before using a submission slot.
