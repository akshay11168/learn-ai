# 02 — Classify your own photos

Prerequisite: [path 02 lab 02](../../02-train-from-a-base-model/02-transfer-learning-on-images/README.md).

## Purpose

Package the transfer-learning model as a tool: point it at a new photo, get a class, and know the conditions under which that class is meaningless.

## Build

The training stays in the path 02 lab. This folder adds:

1. `predict.py` that takes an image path, applies the same resize and normalization as training, loads the checkpoint you selected, and prints the class and the probabilities.
2. A `README` section in `notes.md` that states the classes, how many photos, the validation and one-time test scores, and the comparison to the from-scratch control.
3. Five photos taken after the project, in lighting or angles you did not train on. Run them. Write which ones you trust.
4. One refusal rule in the script or in the write-up: if the top probability is below a threshold you choose on the validation set, print `uncertain` instead of a class. Describe how you picked the threshold (for example, the value that catches half of the known validation errors without discarding half of the correct ones). You will not get this perfect. You will have a reason.

## You are done when

You can drop a new file onto the script and explain the output, including an `uncertain` case.
