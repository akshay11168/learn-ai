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

## You are done when

You can change the learning rate, predict whether training will oscillate or crawl, and confirm it on a run.
