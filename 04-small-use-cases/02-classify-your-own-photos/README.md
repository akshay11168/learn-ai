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

## Study this step

**Concepts to master**

- The prediction script and the training script share one preprocessing function. Drift between them is a silent accuracy drop.
- A confidence threshold is chosen on validation to trade false certainty against extra abstentions. It is not 0.5 by tradition.
- A probability from softmax is not a calibrated frequency unless you checked calibration. You may still threshold it. You may not tell a user "70% chance" as if you had measured that frequency.
- Photos taken after the project are a different distribution. They are the honest demo.

**Study**

- torchvision transforms tutorial, the section on inference transforms matching validation: https://pytorch.org/vision/stable/transforms.html — compare your `predict.py` transforms to the validation transforms line by line.
- Guo et al., "On Calibration of Modern Neural Networks", the abstract and Figure 1 only: https://arxiv.org/abs/1706.04599 — the claim to take: confidence and accuracy are not the same plot. You will not implement temperature scaling unless you want the stretch.
- Your path 02 lab 02 notes, especially which experiment produced the checkpoint.

**Practice**

- Run the same image through training-time preprocessing and through `predict.py` and assert the tensors match.
- On validation, sweep thresholds 0.5, 0.7, 0.9. For each, count abstentions and errors among the non-abstentions. Pick one and write the reason.
- Feed a photo of a class you did not train. Record the confidence. If it is high, the threshold is not protecting you enough; write that limit down.

**Practice questions**

1. Training resized to 224 with the ResNet mean, and `predict.py` resizes to 256 with no normalization. What do you expect on the demo, and how do you detect it without eyeballing labels?
2. Why is "probability 0.99" not yet a promise that 99 of 100 such photos are that class?
3. You pick the threshold on the test photos. What did you spend?
4. An `uncertain` output is a product behavior. What should the human do next, in one sentence of the script's help text?
5. The checkpoint file loads and the class list order differs from training. How can every prediction be wrong while the code "works"?

## You are done when

You can drop a new file onto the script and explain the output, including an `uncertain` case.
