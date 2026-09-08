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

## Submissions

| Date | Config | Ref | Public score |
|------|--------|-----|--------------|
| 09-08 | det=0.99, div=1.0 (baseline, v5) | 56089783 | **0.810** |
| 09-08 | det=0.95, div=1.0 (v10) | 56094892 | 0.794 |
| 09-08 | det=0.99, div=0.2 (v9) | 56094319 | 0.772 |
| 09-09 | det=0.95, div=0.2 (v11) | 56108764 | pending |

Sweep takeaway: the official default (det=0.99, div=1.0) is the best quadrant —
looser detection or cheaper divisions both lose score. Training kernel set to
DET_GRID=[0.99, 0.995], DIV_GRID=[1.0]. Leaderboard leader: 0.962.

## Workspace notes

- Kaggle input mounts vary; the notebook resolves competition and artifacts paths dynamically, pins `machine_shape: NvidiaTeslaT4` (P100/sm_60 crashes the image's PyTorch), and fails fast on incompatible GPUs.
- This is a code competition: CSV upload via `kaggle competitions submit` is rejected (400). Submit a kernel version's output instead via `KaggleApi().competition_submit_code(kernel=<owner>/<slug>, kernel_version=N, file_name="submission.csv")`.
- Baseline inference (4 test videos, T4): ~12 min GPU.

## Next

1. Download the competition training data into `data/train/`.
2. Create embryo-disjoint folds; samples sharing the prefix before `_` must stay in the same fold.
3. Reproduce the official baseline locally and record edge, division, and combined CV scores.
4. Tune detection count/threshold and linking/division costs against CV before using a submission slot.
