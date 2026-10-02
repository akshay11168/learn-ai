# 01 — Fit a line

Prerequisite: [00 Foundations](../../00-foundations/README.md).

## Purpose

Train `y = w x + b` from random `w` and `b` by gradient descent you write. This is the whole field with the smallest possible function.

## Explain before you code

1. If every prediction is too low, what sign do you expect on the gradient of `w` when `x` is positive?
2. What does the bias learn that the weight cannot, if the data's cloud does not pass through the origin?
3. Mean squared error averages the squared misses. Why square, instead of using the raw miss, which can be negative?

## Build

Generate 100 points from `y = 2.5 x - 1.0 + noise` with a fixed seed. Hold out 20 points as validation. Never update weights on those 20.

Initialize `w` and `b` at 0. For each epoch, loop over the 80 training points (or, better, compute the mean gradient over all 80) and update:

```text
y_hat = w * x + b
error = y_hat - y
grad_w = mean(error * x) * 2
grad_b = mean(error) * 2
w = w - lr * grad_w
b = b - lr * grad_b
```

The factor 2 comes from the derivative of the square. Dropping it only rescales the learning rate. Know which version you implemented.

Use learning rate `0.05` and about 50 epochs. Print train and validation mean squared error every epoch.

Then:

- Plot data, the true line, and your learned line. Save the figure here.
- Fit the same data with a closed form you code yourself: the normal equation for 1D, or even just the formulas for simple linear regression. Compare `w` and `b`.
- Only after that, fit with `numpy.polyfit` of degree 1 and confirm you land in the same place.

Record the validation error of your model and of a baseline that always predicts the training-set mean of `y`.

## Verify

Learned `w` is near 2.5 and `b` near -1, closer as you lower the noise. Validation error beats the mean baseline. Your hand-written gradient and a one-step numerical check agree: nudge `w` by `1e-4`, see how the loss changes, divide by `1e-4`, compare to `grad_w`.

## Stretch

Add a second feature `x2` and a weight `w2`. Generate `y = 2.5 x1 - 1.0 x2 + 0.3 + noise`. The update for each weight is `mean(error * that_feature) * 2`. This is linear regression as a dot product, which is lab 02's model before the sigmoid.

## Study this step

**Concepts to master**

- The model `y_hat = wx + b` has two scalars. The loss `mean((y_hat - y)^2)` is a surface over those two scalars.
- Partial derivatives: `dL/dw = mean(2 * error * x)` and `dL/db = mean(2 * error)`, with `error = y_hat - y`. The feature `x` scales the weight's gradient. The bias always sees the raw error.
- Learning rate sets the step length. Too small crawls. Too large oscillates or diverges. The right size depends on the scale of the gradients, which depends on the scale of `x`.
- A closed form exists here (normal equations, or the classic formulas for slope and intercept). Gradient descent is the method that still works when no closed form exists.
- A numerical gradient `(L(w+eps) - L(w-eps)) / (2 eps)` audits the analytic gradient.

**Study**

- Nielsen, chapter 1, the section that derives gradient descent on a simple cost: http://neuralnetworksanddeeplearning.com/chap1.html — re-derive his update with your own symbols.
- 3Blue1Brown, "Gradient descent, how neural networks learn", the first half, up through the cost landscape: https://www.3blue1brown.com/lessons/gradient-descent — then compute one step he did not show.
- Stanford CS229 notes, "Supervised learning" lecture note 1, the linear regression section only: https://cs229.stanford.edu/main_notes/cs229-notes1.pdf — read the least-squares derivation. You implement the iterative version, and you should recognize the same loss.

**Practice**

- One point, `x = 2`, `y = 4`, `w = 0`, `b = 0`, learning rate `0.1`, loss `(wx + b - y)^2` without the mean. Compute two updates by hand.
- Standardize `x` to zero mean and unit variance, rerun training, and compare how large a learning rate you can use.
- Check your `grad_w` against a central difference on a frozen batch.

**Practice questions**

1. `x = 1`, `y = 5`, `w = 1`, `b = 0`. Prediction, error, `dL/dw`, and `dL/db` for the squared error of this single point (include the factor 2).
2. All of your `x` values are between 0 and 0.01, and learning rate `0.05` barely moves `w`. Why?
3. You forget the factor 2 in the gradient and also halve the learning rate. What happens to the updates?
4. The validation error of the mean baseline is 4.0 and your line's validation error is 3.9 after 5 epochs, with the curve still falling. What do you do before you conclude the line is useless?
5. Why does fitting a degree-9 polynomial on 10 noisy points drive training error to zero and tell you little about the next point?

## You are done when

You can change the learning rate, predict whether training will oscillate or crawl, and confirm it on a run.
