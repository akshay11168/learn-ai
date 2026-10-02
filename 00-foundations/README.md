# 00 — Foundations

Nothing later in this course is optional-math or optional-Python. The training loop is a small piece of linear algebra, a loss number, and a careful habit with data. This path installs those three things, plus a working PyTorch that can see the GPU.

You can treat the names (gradient, softmax, epoch) as vocabulary to memorize, and the later labs will feel like magic tricks. Or you can compute one example of each by hand here, and the later labs will feel like the same trick with more numbers. This path is the second option.

## Labs

| Lab | You come out able to |
|---|---|
| [01 This machine](01-this-machine/README.md) | Say what 6 GB of VRAM allows, and where large files go |
| [02 Environment](02-environment/README.md) | Run a tensor on the RTX 3060 from a virtualenv |
| [03 Python and arrays](03-python-and-arrays/README.md) | Predict the shape of an array expression before running it |
| [04 The math under training](04-the-math-under-training/README.md) | Walk one weight update with actual numbers |
| [05 Data, loss, and generalization](05-data-loss-and-generalization/README.md) | Split data, name a baseline, and overfit on purpose |

## How deep to go

- **Arrays.** Shape bugs cause more failed labs than bad ideas. Stay in lab 03 until a broadcast no longer surprises you.
- **Math.** You need the derivative as a slope, the chain rule as "multiply the local slopes," and a matrix multiply as many dot products. You do not need a proof course. If a formula appears later, you should be able to compute it on a 2-by-2 example.
- **Data.** A model that memorizes the training set and fails on new rows has done exactly what the math asked. Generalization is a property of the split and the data, and you will demonstrate the failure yourself in lab 05.

## Done with this path when

- `torch.cuda.is_available()` is true and the device name is the RTX 3060 Laptop GPU.
- You can hand-compute a dot product, a small matrix multiply, a sigmoid, and one step of gradient descent on a one-weight model.
- You can explain train, validation, and test with a dataset you split yourself, and you can say what leaked if a test row was also in train.

Then start [01 Train from scratch](../01-train-from-scratch/README.md).
