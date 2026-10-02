# 03 — Python and arrays

## Purpose

Models are arrays with shapes. This lab makes shape a thing you can predict. The Python you need beyond that is functions, loops, lists, dicts, and reading a file. If those are rusty, practice them inside the exercises below.

## Explain before you code

1. What does the shape `(4, 3)` mean for a batch of vectors?
2. Why is a Python list of numbers a bad container for a matrix multiply once the matrix is large?
3. What is broadcasting, in one sentence, and when is it a bug that still runs?

## Concepts

A tensor is a named block of numbers with a shape. A vector of 3 features has shape `(3,)`. A batch of 4 such vectors has shape `(4, 3)`. A linear layer that maps 3 features to 2 outputs stores a weight of shape `(3, 2)` or `(2, 3)`, depending on whether you multiply on the left or the right. Pick one convention in your notes and keep it: row vector on the left, `y = x @ W`, with `W` shaped `(in, out)`. PyTorch's `nn.Linear` stores the transpose of that. You will meet that mismatch in path 01 and it should already be familiar.

NumPy operations either combine equal shapes or broadcast a size-1 axis. ` (4, 3) + (3,) ` works. ` (4, 3) + (4,) ` fails or, worse, succeeds with a meaning you did not intend if a later reshape made the sizes line up. The habit: print `array.shape` before the operation and write the expected output shape as a comment.

Indexing: `X[0]` is the first row. `X[:, 0]` is the first column. `X[1:3, :]` is a slice of rows. Confusion between those three shows up again as soon as you batch data.

## Build

In this folder, with the venv active, write `arrays.py` that does all of the following and prints shapes plus a few values.

1. Build a `(4, 3)` matrix of small integers by hand. Compute the mean of all values, the mean of each column, and the mean of each row. The column mean has shape `(3,)`.
2. Implement a dot product with a Python loop, then with `np.dot`. Compare the results.
3. Multiply a `(4, 3)` input by a `(3, 2)` weight using `X @ W`. Predict `(4, 2)` before you run it.
4. Add a bias of shape `(2,)` and explain which axis broadcast.
5. Write one expression that you expect to throw, run it, and record the error in `notes.md`.
6. Time a looped dot product against `@` on vectors of length 100_000. One sentence on why the gap exists (NumPy calls into compiled code; the loop pays Python per element).

Also load a CSV you write yourself, `tiny.csv`, with four rows and three numeric columns plus a label column. Load it with the stdlib `csv` module into a float array `X` and an int array `y`. You will reuse this habit for every dataset after this.

Plot the four points' first two features with matplotlib, colored by label. A picture of four dots is enough. The point is that you can see the array.

## Verify

For each operation in `arrays.py`, the comment's predicted shape matches the printed shape. The loop dot product and `np.dot` agree to many decimals. The deliberate bad expression fails for the reason you named.

## Stretch

Replace NumPy with `torch` tensors on CPU for the same multiply. Confirm `@` agrees. Move the tensors to `cuda` and confirm again. Write down that the math did not change when the device changed.

## You are done when

Someone can give you two shapes and ask whether `A @ B` is legal, and you answer with the output shape or the reason it is illegal, without running code.
