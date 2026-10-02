# 04 — The math under training

## Purpose

Compute, by hand, every operation the first training loop will hide: dot product, matrix multiply, sigmoid, mean squared error, a derivative, and one gradient step. After this lab those words refer to arithmetic you have done.

## Explain before you code

1. A dot product is large when two vectors point the same way. Give a 2D numeric example where the dot product is positive, and one where it is zero.
2. The slope of `(w - 3)^2` at `w = 5` is 4. What does that say about which way to move `w` if you want the value to fall?
3. Why does a learning rate of 10 misbehave on that same function when a learning rate of 0.1 walks toward 3?

## Concepts, with numbers

**Dot product.** `[1, 2] · [3, 4] = 1*3 + 2*4 = 11`.

**Matrix multiply.** Each output entry is a dot product of a row and a column.

```text
[1 2]   [5 6]   [1*5+2*7,  1*6+2*8]   [19 22]
[3 4] @ [7 8] = [3*5+4*7,  3*6+4*8] = [43 50]
```

**Model with one weight.** Predict `y_hat = w * x`. Loss for one point: `L = (y_hat - y)^2`. At `x = 2`, `y = 6`, `w = 1`: prediction is 2, loss is 16.

**Derivative.** `dL/dw = 2 * (w*x - y) * x = 2 * (2 - 6) * 2 = -16`. The slope is negative, so increasing `w` decreases the loss. One step with learning rate `0.01`: `w_new = 1 - 0.01 * (-16) = 1.16`. Prediction becomes 2.32, closer to 6. Do this on paper before you code it.

**Chain rule.** If `y_hat = w * x` and `L = (y_hat - y)^2`, the slope of L with respect to w is the slope of L with respect to y_hat, times the slope of y_hat with respect to w. That is the entire idea of backpropagation. A deep net is the same multiplication along a longer chain.

**Sigmoid.** `σ(z) = 1 / (1 + e^{-z})`. It maps any real number into (0, 1). `σ(0) = 0.5`. Large positive z approaches 1. You will use it as a probability for "class 1".

**Softmax.** For scores `[1, 2, 3]`, exponentiate to `[e, e^2, e^3]`, then divide by the sum so the three numbers add to 1. Subtract the max score before the exp so the values do not overflow. The result is a distribution over classes.

**Log and cross-entropy.** If the true class has predicted probability `p`, the loss `-log(p)` is near 0 when `p` is near 1, and large when `p` is near 0. `-log(0.9)` is about 0.105. `-log(0.1)` is about 2.3. Predicting the wrong class with confidence is expensive. That is the loss language models and classifiers actually train on.

## Build

Write `by_hand.py` that:

1. Prints the matrix product above and checks it against NumPy.
2. Runs 20 steps of the one-weight example from `w = 1`, learning rate `0.01`, and prints `w` and the loss each step. It should walk toward `w = 3`.
3. Repeats with learning rate `2`. Record what happens in `notes.md`.
4. Implements `sigmoid` and `softmax` and checks `sigmoid(0)`, plus that softmax outputs sum to 1.
5. Prints `-log(p)` for `p` in `{0.9, 0.5, 0.1}`.

Keep the code boring. Loops and floats are the point. Hide nothing inside a library except `math.exp` and a NumPy check.

## Verify

Your paper calculation of the first update (`w` from 1 to 1.16, slope -16) matches the program's first step. After many steps with learning rate 0.01, `w` is near 3 and the loss is near 0.

## Stretch

Two weights: `y_hat = w1 * x1 + w2 * x2`, one data point `x = [1, 1]`, `y = 4`, start at zeros, learning rate 0.1, five steps on paper and in code. Both weights should move equally. Write the two partial derivatives.

## Study this step

**Concepts to master**

- A derivative is a local slope. The gradient is the list of partial slopes, one per weight.
- Gradient descent steps against the slope: `w := w - lr * dL/dw`. The minus sign is the whole algorithm.
- The chain rule multiplies local slopes. Backprop is that multiplication along a computation graph, from the loss back to each weight.
- The sigmoid squashes a score into (0, 1). Softmax turns a vector of scores into a distribution. Subtracting the max before `exp` does not change the result and prevents overflow.
- Cross-entropy `-log(p_true)` is small when the true class is probable and large when it is not. It is the loss you will actually train classifiers and language models with.

**Study**

- 3Blue1Brown, "Essence of calculus", chapter 2 (the derivative as a slope) and chapter 3 (the chain rule), then "Neural networks", chapters 1 through 4: https://www.3blue1brown.com/topics/neural-networks — after each chapter, write one numeric example the video did not use.
- Michael Nielsen, *Neural Networks and Deep Learning*, chapter 1, through the section on gradient descent: http://neuralnetworksanddeeplearning.com/chap1.html — read it slowly. This is the book to return to in path 01.
- Mathematics for Machine Learning, chapter 2, sections on the dot product and matrix multiplication only: https://mml-book.github.io/book/mml-book.pdf — do the small numeric exercises, skip the proofs.

**Practice**

- By hand, five steps of `w := w - 0.1 * 2 * (w - 3)` starting at `w = 0`. This is descent on `(w - 3)^2`.
- Compute softmax of `[1, 1, 1]` and of `[10, 0, 0]` with the max subtracted. Confirm the rows sum to 1.
- Compute `-log(p)` at `p = 0.99, 0.5, 0.01` using a calculator. Rank them.

**Practice questions**

1. `L = (2w - 6)^2` at `w = 1`. What is `dL/dw`, and what is `w` after one step with learning rate `0.01`?
2. The slope `dL/dw` is positive. Which way do you move `w`, and why?
3. Learning rate `100` on `(w - 3)^2` starting at `w = 0`. Does the next `w` land nearer to 3 or farther? Compute it.
4. Softmax of `[2, 2, 2]` equals softmax of `[0, 0, 0]`. Show why, using the "subtract the max" form.
5. A model assigns probability `0.2` to the true next character. Another assigns `0.8`. Which has the larger cross-entropy, and by roughly how much? Use `-log(0.2) ≈ 1.61` and `-log(0.8) ≈ 0.22`.
6. In one sentence, where does the chain rule sit between "the loss depends on the prediction" and "the prediction depends on the weight"?

## You are done when

You can teach the one-weight update to someone else from a blank page, including the moment you subtract `learning_rate * gradient`.
