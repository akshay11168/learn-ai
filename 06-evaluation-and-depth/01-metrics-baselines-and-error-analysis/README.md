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

## Study this step

**Concepts to master**

- Accuracy, precision, recall, and F1, and when each one hides a class.
- A confusion matrix is the count of true-versus-predicted pairs. The worst off-diagonal cell is where you start reading examples.
- The baseline is part of the result. A number without it is unfinished.
- The test set is not for choosing thresholds, features, or checkpoints.

**Study**

- Scikit-learn, "Precision, recall, and F-measures": https://scikit-learn.org/stable/modules/model_evaluation.html — implement the formulas for one binary example before you call `classification_report`.
- Google's Machine Learning Crash Course, "Classification" threshold and ROC/precision-recall introduction: https://developers.google.com/machine-learning/crash-course/classification — do the threshold exercise in your head on a class imbalance of 95%.
- Chris Olah's visual style is optional here. The crash course and sklearn are the texts.

**Practice**

- By hand, 10 binary predictions: compute accuracy, precision, and recall. Then confirm with code.
- Build a confusion matrix for 3 classes with made-up counts and name the worst cell.
- Write a five-line error analysis from 20 real mistakes once the lab's model exists.

**Practice questions**

1. 100 patients, 5 have the condition. The model predicts nobody has it. Accuracy, precision, recall?
2. You care about missing a positive more than a false alarm. Which of precision and recall do you watch first, and what threshold move increases recall?
3. Two models have accuracy 0.80. One never predicts class B. Why can they be different products?
4. Why tag individual errors after seeing the matrix, instead of stopping at the matrix?
5. Define the majority-class baseline for a set with counts `{0: 70, 1: 20, 2: 10}`.

## You are done when

You can look at a new project's headline accuracy and ask for the baseline, the class balance, and the worst confusion, without being told to.
