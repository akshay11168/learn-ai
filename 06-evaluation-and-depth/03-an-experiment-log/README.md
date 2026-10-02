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

## Study this step

**Concepts to master**

- A result is a tuple: code version, data version, config, seed, metric. Missing any one of them, you have an anecdote.
- Confounded runs change two things. They may be logged. They may not support a causal sentence.
- Rerunning a row is how you audit the log. A large unexplained gap means a missing field.
- Crashes get rows too. The absence of a failed run is how people pretend the first try worked.

**Study**

- "Papers with code" reproducibility checklist, the items on data, hyperparameters, and seeds: https://github.com/paperswithcode/releasing-research-code — read the checklist once and adopt the items that fit a solo lab.
- Karpathy's recipe, the note-taking habit in the opening: https://karpathy.github.io/2019/04/25/recipe/

**Practice**

- Backfill two early runs until a stranger could rerun them. Where you cannot, write "lost."
- Rerun one logged configuration. Record the delta.
- Add a crashed run's row from memory of the last OOM or NaN, as far as the log's fields allow.

**Practice questions**

1. Two rows differ in learning rate and in data version, and the metric improved. What sentence are you not allowed to write?
2. The seed is missing and the metric wobbles by 3 points. What can you claim?
3. Why log "uncommitted" instead of a fake hash?
4. The checkpoint path is "somewhere on D:." Why is that not a field?
5. You reran a row and the metric moved from 0.71 to 0.62. What does the log almost certainly omit?

## You are done when

Someone else, given one row and the repo, could rerun the comparison you claim, and you have felt that standard by rerunning one row yourself.
