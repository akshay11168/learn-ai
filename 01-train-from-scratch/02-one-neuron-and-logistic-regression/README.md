# 02 — One neuron and logistic regression

Prerequisite: [01 Fit a line](../01-fit-a-line/README.md).

## Purpose

Classify points with a single neuron: a weighted sum, a sigmoid, and a loss that treats the sigmoid output as a probability. This is logistic regression. Every later network is many of these units stacked.

## Explain before you code

1. Why does mean squared error on a sigmoid output give weak gradients when the prediction is confidently wrong? (Compute the sigmoid slope far from zero.)
2. What does `-log(p)` do to a confident wrong answer?
3. If 90 of 100 points are class 0, what accuracy must you beat, and why is accuracy alone a slippery report?

## Concepts

The neuron computes `z = w · x + b`, then `p = sigmoid(z)`. You want `p` near 1 for class 1 and near 0 for class 0.

Binary cross-entropy for one row is `- (y log p + (1 - y) log(1 - p))`. The useful gradient, which you should derive once in `notes.md`, simplifies to:

```text
grad_w = (p - y) * x
grad_b = (p - y)
```

averaged over the batch. The `(p - y)` term is the same shape as the error in the line fit. The sigmoid's derivative got absorbed. Write that derivation; it is short and it is the pattern backprop keeps using.

A decision threshold of 0.5 is a choice. If false positives are expensive you might require 0.8. The model produces a probability. The threshold is a separate decision.

## Build

Reuse the two-blob data from foundations lab 05, or generate a fresh set with a fixed seed. Features are 2D. Add a column of ones if you fold bias into the weights; otherwise keep `b` separate. Be consistent with the convention in your foundations notes.

Implement training in NumPy for 200 epochs. Log train and validation loss and accuracy. Compare against the most-common-class baseline.

Plot the points and the decision boundary, the line where `z = 0`. Save it here.

Numerical gradient check on `w` for a tiny subset, the same nudge method as the line fit.

After your implementation learns, train `sklearn.linear_model.LogisticRegression` on the same split and compare weights up to sign and scaling conventions. Write down any difference in the feature normalization sklearn applies by default. If you compare weights, turn that default off or normalize both ways yourself.

## Verify

Validation accuracy beats the baseline by a margin you can see on the plot. The numerical gradient matches your analytic `(p - y) * x` within a small relative error. Loss on the training set falls. If validation loss rises while training loss falls, you have overfit a set that is too small: make the set larger and note it.

## Stretch

Three classes. Replace sigmoid with softmax and binary cross-entropy with `-log p_true_class`. The gradient on the scores is `p - one_hot(y)`. Implement it for a toy 3-class blob problem. This is the classification head you will reuse on images and tokens.

## You are done when

You can start from `z = w · x + b` and arrive at `grad_w = (p - y) * x` on a whiteboard.
