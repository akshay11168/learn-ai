# 02 — When training goes wrong

Prerequisite: one real training loop. You will return to this page.

## Purpose

A checklist of failures you will hit, and the single next measurement that distinguishes them. Change one thing at a time. The log in lab 03 is where the attempt goes.

## Symptoms

**Loss is NaN.** Often the learning rate is too high, or a softmax saw a huge score, or a learning-rate spike hit mixed precision. Next measurement: rerun one batch in float32 at a learning rate 10 times smaller. If it holds, the bug is numerical. If it is NaN on step 0, print the logits and look for an uninitialized layer or a label outside the class range.

**Loss is flat.** The gradient may be zero (dead ReLUs, a frozen parameter you meant to train, a detached tensor), the learning rate may be tiny, or the labels may be constant. Next measurement: print the mean absolute gradient of each parameter group. Zero means the update will do nothing, and you look upstream. Nonzero and tiny means you try a higher learning rate on a fresh run.

**Training loss falls, validation loss rises.** The model is fitting the training split, including its noise. Next measurement: plot both curves. Then reduce capacity, add the augmentation or dropout you already understand, or get more data. Do not quote the training accuracy.

**Validation looks excellent and a new batch in the wild fails.** Suspect leakage: duplicate rows, a random split of dependent samples, normalization fit on the full dataset, or a test you already tuned on. Next measurement: search for duplicate inputs across splits and rewrite the split rule. The audio and photo labs are where this shows up in real life.

**The GPU is idle and the epoch is slow.** Data loading is the bottleneck, or the batch is so small that kernels never fill the device. Next measurement: time one batch's load versus one batch's step. On this laptop, also check that you are plugged in and that `nvidia-smi` shows the process on the 3060 rather than the CPU build of PyTorch.

**Out of memory.** The activation memory grew with batch size and sequence length. Next measurement: halve the batch or the sequence, before you change the model. For adapters, confirm the base is frozen and quantized and that optimizer state exists only for the adapter.

**The fine-tune forgot the base behavior.** Rerun the three prompts you saved before training. If they degraded and the new task improved, the update was real and broad. Prefer a smaller learning rate, fewer steps, or an adapter.

## Build

You do not need to force every symptom. The first time one of them happens, write a short incident in `notes.md`: symptom, measurement, cause, change, result. Two incidents are the minimum before you call the lab done. If your runs are suspiciously clean, provoke a NaN with a huge learning rate on the line-fit lab and write that up. Provoked incidents count if you label them as provoked.

## You are done when

You have two written incidents, and you reach for "what do I measure next" before you reach for a random hyperparameter.
