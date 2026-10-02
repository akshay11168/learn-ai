# 03 — A network and backprop in NumPy

Prerequisite: [02 One neuron](../02-one-neuron-and-logistic-regression/README.md).

## Purpose

Train a two-layer network without autograd. This is the lab where "deep learning" becomes the chain rule applied twice. Spend real time here. Later frameworks are a way to not repeat this bookkeeping.

## Explain before you code

1. A single neuron draws one linear cut through the plane. Name a 2D label pattern a single cut cannot separate. (Four points of XOR are enough.)
2. What does a hidden layer buy you, in terms of cuts or features?
3. If you nudge one hidden weight by `1e-5` and the loss changes, that ratio is a gradient. What does it mean if your backprop code disagrees with that ratio?

## Architecture

Use a small MLP:

- Input size 2 for the blob/XOR data, then a second experiment with input size 10.
- Hidden size 8, ReLU activation: `max(0, z)`.
- Output size 1, sigmoid, binary cross-entropy. For the 3-class stretch, output size 3 and softmax.

Forward pass, one row, column-vector convention written as row vectors with `@`:

```text
h_pre = x @ W1 + b1
h = relu(h_pre)
p = sigmoid(h @ W2 + b2)
```

Shapes: `x` is `(batch, 2)`, `W1` is `(2, 8)`, `b1` is `(8,)`, `W2` is `(8, 1)`, `b2` is `(1,)`.

Backward pass, batch-averaged. Let `dL/dp` flow into `dz_out = (p - y) / batch` for the sigmoid-plus-bce combination (confirm the factor against your loss, which might already be a mean). Then:

```text
dW2 = h.T @ dz_out
db2 = sum(dz_out)
dh = dz_out @ W2.T
dh_pre = dh * (h_pre > 0)
dW1 = x.T @ dh_pre
db1 = sum(dh_pre)
```

Derive each line in `notes.md` from "local slope times upstream slope". The ReLU's local slope is 1 where the pre-activation was positive and 0 where it was not. Dead units (always zero on your data) get no gradient. If every hidden unit dies, training stalls. Small random initial weights avoid that; zeros do not, because every ReLU unit then shares the same input and the same fate.

## Build

1. Implement forward, loss, and backward.
2. Numerical gradient check: for several entries of `W1` and `W2`, compare backprop to a central difference `(L(w+eps) - L(w-eps)) / (2 eps)` with `eps = 1e-4`. Relative error should be small (around `1e-4` to `1e-6`, looser near ReLU kinks). Fix the backward pass until this passes. Do not proceed on a failed check.
3. Train on a blob dataset that overlaps, and on XOR, where a single neuron fails and the hidden layer succeeds. Plot both decision regions.
4. Initialize `W1` at all zeros, run a few steps, and record why the hidden units stay identical.

Save plots and a short `notes.md` section titled "where my first backward pass was wrong." Everyone's is wrong once. The numerical check is how you notice.

## Verify

The gradient check passes on random weights before you train. XOR validation accuracy is high. A one-neuron model on the same XOR data stays near the baseline. You can point at a hidden unit's weights and describe the direction in the input plane it responds to.

## Stretch

Add a second hidden layer of 8 units and extend the backward pass. The new block is the same pattern: upstream gradient, multiply by the local ReLU mask, then an outer product to get `dW`. If this still feels mechanical, the lab has done its job.

## Study this step

**Concepts to master**

- A computation graph: each node has a local derivative with respect to its inputs. Backprop multiplies those local derivatives by the upstream gradient.
- Shapes in the backward pass are the transpose of the forward story. If forward is `h @ W2`, the gradient `dW2` is an outer product of `h` and the upstream gradient, and the gradient sent back to `h` is the upstream gradient times `W2.T`.
- ReLU's derivative is the mask `h_pre > 0`. A unit that is always off on your batch receives a zero gradient. That is a dead unit.
- Initialization matters because symmetry plus identical inputs keeps identical units identical. Zeros are a bad initialization for ReLU layers.
- A numerical check is the definition of the derivative. When it disagrees with your code, your code is wrong, including when training loss still decreases.

**Study**

- Nielsen, chapter 2, all of it: http://neuralnetworksanddeeplearning.com/chap2.html — this is the main text for the lab. Do the math in his notation, then translate every equation into your shapes.
- Karpathy, "The spelled-out intro to neural networks and backpropagation" (the micrograd lecture): https://www.youtube.com/watch?v=VMj-3S1tku0 — watch through the scalar backward pass. Then read `micrograd`'s `backward` in https://github.com/karpathy/micrograd and write what `self.grad +=` is accumulating.
- CS231n, "Backpropagation, intuitions": https://cs231n.github.io/optimization-2/ — study the patterns for add, multiply, and max. Those three cover your graph.

**Practice**

- Scalar network: `h = relu(x * w1)`, `y_hat = h * w2`, loss `(y_hat - y)^2`. Pick `x = 2`, `w1 = 0.5`, `w2 = -1`, `y = 1`. Forward and backward by hand. Then nudge `w1`.
- Implement the lab's backward pass and immediately run the numerical check before any training loop. Save the first bug you find.
- Initialize ten ReLU units with the same weight vector and one batch of identical rows. Print the hidden activations after a few steps.

**Practice questions**

1. Forward: `h_pre = -2`, ReLU, then a later layer. What is `dL/dh_pre` if the upstream `dL/dh` is `4`?
2. `h` is `(32, 8)`, `dz_out` is `(32, 1)`, `W2` is `(8, 1)`. Give the shapes of `dW2`, `db2`, and `dh`.
3. Why does checking only that the loss went down fail to prove the backward pass is correct?
4. All hidden activations are zero after initialization. What do you change, and why is "train longer" the wrong first move?
5. You replace ReLU with `tanh`, whose derivative is `1 - tanh(z)^2`. Write the one line in the backward pass that changes.
6. XOR is not linearly separable. After training, pick one hidden unit and say what input direction makes it positive. If you cannot, the hidden layer is unused and the check is incomplete.

## You are done when

You can compute the backward pass for a new activation whose derivative you just looked up, and the numerical check passes on the first or second try.
