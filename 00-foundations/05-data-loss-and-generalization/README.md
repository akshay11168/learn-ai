# 05 — Data, loss, and generalization

## Purpose

Training minimizes a number on the rows the optimizer can see. "It learned the task" means the number also falls on rows it cannot see. This lab separates those two ideas, and it introduces the baseline you will use for the rest of the course.

## Explain before you code

1. What question is the training split allowed to answer, and what question is reserved for the test split?
2. Why does accuracy on the training set rise when the model memorizes noise?
3. A baseline that always predicts the most common class scores 80% on a dataset. A model scores 82%. What, specifically, did the model add?

## Concepts

**Splits.** Training rows update weights. Validation rows are for decisions you make while developing (which learning rate, when to stop). Test rows are touched once, at the end. Using the test set to pick a learning rate turns it into a second validation set, and the final number is no longer an honest estimate.

**Leakage.** If the same photo, or a near-duplicate, sits in train and test, the test score measures memory. If you normalize using the mean of the full dataset before splitting, statistics from the test set have already entered the pipeline. Compute statistics on the training split only.

**Loss versus the metric you care about.** Mean squared error and cross-entropy are smooth and give gradients. Accuracy, F1, and "did the code pass the tests" are what you tell a person. They often move together and sometimes do not. Log both.

**Baseline.** Before a neural net, write the dumb predictor: predict the mean (regression) or the most common class (classification). Every later model has to beat that number on the validation split. This is lab 01 of path 06, started early because you need it immediately.

**Overfitting, demonstrated.** A flexible model trained on 8 points can drive training loss near zero and still miss the next 8. That is success at the optimization problem and failure at the task. You want to see both numbers side by side.

## Build

Create a synthetic binary classification set in `splits.py`: 200 points in 2D, two blobs with slight overlap, labels 0 and 1. Fix the random seed.

1. Split 70% / 15% / 15% with a shuffle. Print the sizes and the class balance of each split.
2. Baseline: on the training split, find the most common class. Report its accuracy on train and on validation.
3. A one-feature threshold rule you choose by hand (for example "predict 1 when x0 > c"). Try three values of `c` using only the training split, pick the best by training accuracy, and record validation accuracy. This is a tiny model-selection loop.
4. Overfit demo: take 8 training points and fit a high-degree polynomial (NumPy `polyfit` of degree 7 is fine) to predict a noisy 1D function. Plot the curve against the 8 points and against 50 new points drawn from the same function. Save the plot in this folder. In `notes.md`, describe where the curve hits the training points and where it leaves the true function.

Also write five rows of a toy text task ("email text", "spam or not") and show one way the test set would be leaked (sorting all messages so duplicates of the same text land in both splits, or choosing words to count after reading the test emails).

## Verify

The three splits are disjoint. You can point at the plot and say which region is memorization. The baseline accuracy is written down and will be reused as the number to beat.

## Stretch

Move 20 copies of one validation point into the training set and describe, before rerunning anything, which accuracy moves and which one becomes less meaningful.

## Study this step

**Concepts to master**

- The training loss is the quantity the optimizer reduces. Generalization is performance on rows that did not contribute to any update or to any decision you made.
- Validation chooses your settings. Test is reported once. Using the test set to pick a setting spends it.
- Leakage is any path by which information from the held-out rows enters training: duplicate rows, preprocessing fit on all rows, features that contain the label, or a split that puts two crops of one photo on both sides.
- A baseline is the score of the simplest honest predictor. A model that does not beat it has not earned its complexity.
- Overfitting is low training loss and worse held-out loss. It is a successful optimization of the wrong objective.

**Study**

- Google's "Rules of Machine Learning", rules 1–4 and the section on data leakage thinking: https://developers.google.com/machine-learning/guides/rules-of-ml — read them as engineering rules, not slogans. Write rule 1 in your own words.
- Scikit-learn's user guide, "Cross-validation: evaluating estimator performance", the opening on train/test split only: https://scikit-learn.org/stable/modules/cross_validation.html — stop before the catalog of splitters. The picture of a held-out set is the point.
- Chapter 5 of *Dive into Deep Learning*, the section on overfitting and underfitting: https://d2l.ai/chapter_multilayer-perceptrons/underfit-overfit.html — reproduce their conceptual curve as a sketch before you read their experiment.

**Practice**

- Make a 20-row table with one repeated row. Split randomly. Count how often the duplicate lands in both sides. That count is the leakage demo.
- Fit the degree-7 polynomial in the lab and also a degree-1 polynomial on the same 8 points. Write which one wins on the 8 points and which one wins on the fresh 50.
- Compute a majority-class baseline by hand on a set with 15 zeros and 5 ones.

**Practice questions**

1. You try five learning rates and keep the one with the best test accuracy. What have you turned the test set into, and what number can you no longer quote?
2. You compute the mean and variance of every feature on the full dataset, then split. Name the leak.
3. Training accuracy is 100% and validation accuracy is 55% on a balanced two-class problem. What is the baseline, and what is the model doing?
4. Why can adding columns of random noise improve training accuracy and hurt validation accuracy?
5. A classmate sorts the file so all similar items are adjacent, then takes the first 80% as train. What dependence did the split fail to break?
6. Write one sentence you are willing to say about a model, and one sentence you will not say, using only a validation score.

## You are done when

You can define generalization using your own plot, and you refuse to quote a test score for a decision you already used the test set to make.
