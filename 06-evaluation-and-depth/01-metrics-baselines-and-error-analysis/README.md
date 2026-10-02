# 01 — Metrics, baselines, and error analysis

Prerequisite: any model that outputs a class. The logistic regression lab is the natural first time.

## Purpose

Choose a metric that matches the decision, put a baseline next to it, and spend as much time on the errors as on the score.

## Explain before you code

1. Accuracy is 95% on a dataset where 95% of the rows are the negative class. What might the model be doing?
2. Why can two models with the same accuracy be different to a person who pays a cost for false positives?
3. Why read individual mistakes after you have a confusion matrix, instead of only staring at the matrix?

## Concepts

**Accuracy** is the fraction correct. It matches a task where classes are balanced and errors are symmetric.

**Precision** is the fraction of predicted positives that are truly positive. **Recall** is the fraction of true positives you found. A spam filter that never marks spam has undefined or empty precision and zero recall, and it can still post a high accuracy if spam is rare. **F1** is the harmonic mean of precision and recall. **Macro F1** averages F1 across classes so a rare class counts.

**A regression metric** is different. Mean squared error punishes large misses. Mean absolute error treats a miss of 10 as ten times a miss of 1, not a hundred times. Pick the one whose punishment matches the task, and say why in the log.

**The baseline** is the best predictor you can write without the model: most common class, training-set mean, a keyword rule, a bigram, a linear model on pixels. If the neural net does not beat it, the net is not the story. The data or the setup is.

**Confusion matrix.** Rows are true classes, columns are predicted classes. The off-diagonal cells tell you which mistakes dominate. Then open 20 examples from the worst cell and tag them. Tags you invent from the examples ("label is wrong", "class definition overlaps", "input is truncated", "rare wording") are more useful than another epoch.

**Validation versus test.** You may look at validation errors while you change the model. You look at test errors after you stop. If you tune on test errors, write in the log that the number is now a validation number, and collect a new test set before you quote it.

## Build

Using the logistic regression from path 01, or the text classifier:

1. Compute accuracy, and precision/recall/F1 for each class, on the validation split, in code you write (scikit-learn's `classification_report` is allowed as a check, not as the first implementation).
2. Compute the most-common-class baseline on the same split.
3. Print a confusion matrix.
4. Tag 20 errors. Summarize in five lines: the main failure, whether more data of a certain kind would address it, and whether the label guide is ambiguous.

## You are done when

You can look at a new project's headline accuracy and ask for the baseline, the class balance, and the worst confusion, without being told to.
