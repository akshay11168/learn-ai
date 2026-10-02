# 04 — Autograd and the training loop

Prerequisite: [03 Backprop in NumPy](../03-a-network-and-backprop-in-numpy/README.md).

## Purpose

Move the same network onto PyTorch. Autograd replaces your backward function. You still write the loop that every later lab uses: data, batches, forward, loss, backward, step, validation, checkpoint.

## Explain before you code

1. What does `loss.backward()` write, and onto which objects?
2. Why do you call `optimizer.zero_grad()` before `backward`?
3. What is the difference between an epoch and a step?

## Concepts

A `Tensor` with `requires_grad=True` remembers the operations that produced it. `backward` walks that graph and accumulates `.grad` on the leaves. The optimizer reads `.grad` and updates the parameter. This is your NumPy backward pass, stored as a graph instead of as code you wrote.

`nn.Linear(in, out)` stores weight of shape `(out, in)` and computes `x @ W.T + b`. That transpose is the convention mismatch from the arrays lab. Print `layer.weight.shape` once and write it down.

An epoch is one pass over the training set. A step is one update, usually on a batch. Batch size 32 means each step sees 32 rows. Larger batches give a less noisy gradient and use more memory. On the 3060 you will eventually pick batch size by watching free VRAM. Here the model is tiny, so pick 32 because the estimate is stable, and also run batch size 1 to see a noisier loss curve.

`model.train()` and `model.eval()` change dropout and batch-norm behavior. This model has neither. Call them anyway so the habit exists before those layers do.

The validation pass must not update weights. Wrap it in `torch.no_grad()`.

## Build

Rewrite the two-layer network as `nn.Module` with two `nn.Linear` layers and `nn.ReLU`. Use `nn.BCEWithLogitsLoss`, which combines sigmoid and binary cross-entropy in a numerically safer way. Your model then outputs logits, not probabilities. Threshold at 0 for predictions, which is the logit version of probability 0.5.

Training loop, CPU first, then `.to("cuda")`:

```text
for epoch in epochs:
    model.train()
    for xb, yb in train_loader:
        optimizer.zero_grad()
        logits = model(xb)
        loss = criterion(logits, yb)
        loss.backward()
        optimizer.step()
    model.eval()
    with torch.no_grad():
        measure validation loss and accuracy
```

Use `torch.optim.Adam` with learning rate `1e-2`, and also run SGD with learning rate `1e-1`. Both should solve XOR and the blobs. Save a plot of the two training curves.

Checkpoint: `torch.save(model.state_dict(), "outputs/xor.pt")` under this lab, and a second script `predict.py` that loads the state dict and classifies four hand-written points. Paths in gitignore already skip `*.pt`. Saving under `outputs/` matches that.

Gradient check, one more time: pick one weight, compare `param.grad` after `backward` to a numerical nudge of the loss. They should match. This confirms you trust autograd because you measured it, which is the correct amount of trust.

Move the model and batches to CUDA and confirm the learned accuracy matches the CPU run.

## Verify

XOR is solved on the GPU. `predict.py` works in a fresh process. The numerical check against `.grad` passes. You can draw the loop from memory, including `zero_grad` and `no_grad`.

## Stretch

Read a batch from a `TensorDataset` and `DataLoader` with `shuffle=True`. Turn shuffle off and describe one way the gradient becomes a worse estimate (the same order every epoch, and if the data is sorted by class, long stretches of one class).

## Study this step

**Concepts to master**

- Autograd records operations on tensors that require gradients and, on `backward()`, fills `.grad` with the same quantities you computed in NumPy.
- `.grad` accumulates. `zero_grad()` clears it. Forgetting the clear mixes two batches' gradients.
- `no_grad()` and `eval()` are different. One stops graph building. The other switches dropout and batch-norm behavior.
- An epoch is a full pass over the training data. A step is one optimizer update. Batch size is how many rows contribute to that update.
- `nn.Linear` stores weight `(out, in)` and computes `x @ W.T + b`. BCE-with-logits takes raw scores, not probabilities.
- A checkpoint is a state dict loaded into an identical module. Saving the whole Python object is fragile. Saving the tensors is the contract.

**Study**

- PyTorch "Autograd mechanics": https://pytorch.org/docs/stable/notes/autograd.html — read "Excluding subgraphs" and the accumulation note. Then the official 60-minute blitz, the autograd and neural-network sections: https://pytorch.org/tutorials/beginner/deep_learning_60min_blitz.html
- PyTorch "What is a state_dict?": https://pytorch.org/tutorials/beginner/saving_loading_models.html — implement save and load in two processes.
- Karpathy, "A Recipe for Training Neural Networks": https://karpathy.github.io/2019/04/25/recipe/ — read sections 1–3 now (become one with the data, neural net first, overfit a single batch). The rest of the recipe is for path 06.

**Practice**

- Overfit a single batch of XOR until loss is about zero, before you train on the full set. If one batch will not overfit, the bug is in the code, not the learning rate schedule.
- Print `layer.weight.shape` and `layer.weight.grad.shape` after one backward. They match.
- Train on CPU and CUDA with the same seed. Compare validation accuracy, not exact loss bits.

**Practice questions**

1. You call `backward()` twice without `zero_grad()`. How does the second `.grad` relate to the two batches?
2. Validation loss is computed without `no_grad()`, and then you train another epoch. What extra work happened, and what bug appears if you also called `backward()` on the validation loss by mistake?
3. `nn.Linear(2, 8)` has a weight of which shape? Write the forward expression in the row-vector convention you used in NumPy.
4. Why does `BCEWithLogitsLoss` exist instead of sigmoid followed by `BCELoss`?
5. You load a state dict and the first prediction differs from the process that saved it. List three concrete mismatches (architecture, preprocessing, eval versus train mode).
6. Define epoch, step, and batch size for a training set of 1000 rows and a batch size of 32. How many steps are in one epoch, and what happens to the last incomplete batch in the default loader?

## You are done when

A new dataset of the same kind can be trained by editing the data loader and the input size, without editing the meaning of the loop.
