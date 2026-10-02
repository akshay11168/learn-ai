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

## Study this step

**Concepts to master**

- A neuron is an affine score `z = w · x + b` plus a nonlinearity. For two classes the nonlinearity is the sigmoid, and `p = σ(z)` is treated as `P(y = 1 | x)`.
- Binary cross-entropy plus sigmoid has the gradient `(p - y) x`. The sigmoid's derivative and the log's derivative combine into that simple error. You should be able to sketch this cancellation even if you look up one algebra step.
- Log loss punishes confident mistakes. Squared error on probabilities saturates when σ is flat, which is exactly when the prediction is confidently wrong.
- The decision threshold is not part of training. Training fits probabilities. You choose a threshold from the costs of false positives and false negatives.
- A linear score can only cut the plane once. That limit is the reason the next lab adds a hidden layer.

**Study**

- Nielsen, chapter 1, the parts that introduce the sigmoid neuron, then chapter 3's opening on cross-entropy (you may read chapter 3's cost-function section early): http://neuralnetworksanddeeplearning.com/chap3.html — the comparison of quadratic cost and cross-entropy is the one to reproduce with two numeric probabilities.
- CS229 note 1, the logistic regression section: https://cs229.stanford.edu/main_notes/cs229-notes1.pdf — follow the likelihood until the gradient. Their notation will not match yours. Write a translation table.
- 3Blue1Brown, "But what is a neural network?", the neuron and the handwritten-digit setup: https://www.3blue1brown.com/lessons/neural-networks — pause when he draws a linear boundary and write why XOR has no such boundary.

**Practice**

- By hand, `x = [1, 0]`, `w = [0, 0]`, `b = 0`, `y = 1`. Then `p = 0.5`. One update with learning rate 1. Where did `w` and `b` move?
- Plot `σ(z)` and its derivative `σ(z)(1-σ(z))` from `z = -8` to `8`. Mark where the derivative is almost zero.
- Compute a confusion matrix for a handful of predictions at threshold 0.5 and again at 0.8.

**Practice questions**

1. `z = 0`. What are `p` and the binary cross-entropy when `y = 1`? Use `-log(0.5) ≈ 0.693`.
2. `p = 0.99` and `y = 0`. Is the loss closer to 0 or to 5? Compute `-log(1 - 0.99)`.
3. Show that if `y = 1` and `p < 1`, the gradient `(p - y) x` points so that a positive feature increases the weight when you subtract the gradient. Use `x = 1`.
4. Class balance is 95% negative. Your model always predicts negative and scores 95% accuracy. What must you report instead, and what is the accuracy you have to beat before you claim skill?
5. Why can one neuron not solve XOR? Give the four points and the contradiction a single linear cut runs into.
6. You change the threshold from 0.5 to 0.9 and retrain nothing. Which of precision and recall on the positive class do you expect to move, and in which direction?

## You are done when

You can start from `z = w · x + b` and arrive at `grad_w = (p - y) * x` on a whiteboard.
