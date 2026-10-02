# 06 — Evaluation and depth

This path runs beside the others. It is the difference between collecting scripts and learning the subject. A falling loss is a fact about the optimizer. A useful model is a fact about data the optimizer was not allowed to fit. You need habits that keep those facts apart.

Start lab 01 as soon as the first classifier exists (path 01 lab 02). Use lab 02 the first time a run goes NaN or diverges. Use lab 03 for every run from the convnet onward. Do lab 04 near the end, when you can read a method section and shrink it.

## Labs

| Lab | Habit |
|---|---|
| [01 Metrics, baselines, and error analysis](01-metrics-baselines-and-error-analysis/README.md) | Beat a dumb predictor, then read the mistakes |
| [02 When training goes wrong](02-when-training-goes-wrong/README.md) | A checklist for NaNs, flat losses, and leaked splits |
| [03 An experiment log](03-an-experiment-log/README.md) | One row per run, enough to rerun it |
| [04 Reproduce one idea from a paper](04-reproduce-one-idea-from-a-paper/README.md) | Shrink a published idea until this GPU can test it |

## Done with this path when

Your capstone folders cite a baseline, a validation protocol, an error tag list, and a log row. The paper lab ends with a result that can disagree with your expectation, and you wrote down which happened.
