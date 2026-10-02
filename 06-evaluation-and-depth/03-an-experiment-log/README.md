# 03 — An experiment log

Prerequisite: a run you might want to compare with another run. Start the file before the convnet lab.

## Purpose

Keep one row per training run so a later result can be traced to code, data, and a number. Memory is not a log.

## The row

Create `log.md` in this folder, or a CSV if you prefer columns. Every serious run appends:

| Field | What to write |
|---|---|
| Date | Day you started the run |
| Lab | Path and folder |
| Git commit | `git rev-parse --short HEAD` if the code is committed; otherwise say "uncommitted" and do not pretend |
| Data | Path on D:, split rule, row counts |
| Model | Name or a one-line architecture, parameter count |
| Init | Random, or a base-model id |
| Loss | Name of the function |
| Optim | Optimizer and learning rate |
| Batch and steps | Batch size, epochs or steps, sequence length if any |
| Seed | The number |
| Result | Validation metric, and baseline on the same split |
| Notes | One sentence: what this run was meant to test |
| Artifact | Checkpoint path, or "discarded" |

If you change two fields at once, you may still log the run, and you cannot claim you know which change mattered. The note should say "confounded."

## Build

1. Create the log with the header.
2. Backfill the best run from the line fit and from the logistic regression, as practice.
3. From the convnet lab onward, log before you trust the number. A run that crashed gets a row too, with the symptom from lab 02.
4. Once, rerun a logged configuration and see whether the validation metric lands near the logged one. Stochastic training will not match to many decimals. A large gap means the log missed a setting (data, seed, augmentation, or a code change). Fix the row format so the gap would have been explained.

## You are done when

Someone else, given one row and the repo, could rerun the comparison you claim, and you have felt that standard by rerunning one row yourself.
