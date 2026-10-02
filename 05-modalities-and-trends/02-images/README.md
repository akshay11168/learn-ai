# 02 — Images

Prerequisite to the experiment: [path 01 lab 05](../../01-train-from-scratch/05-images-and-convolutions/README.md) and [path 02 lab 02](../../02-train-from-a-base-model/02-transfer-learning-on-images/README.md).

## The map

**Classification and detection.** A convnet applies shared local filters. A vision transformer cuts the image into patches and runs attention over the patch sequence, which is the language architecture on a grid. Both can be pretrained on a large labeled or self-supervised set and adapted with a new head. On a few dozen photos per class, a frozen pretrained body is the method this GPU and your time budget favor. You measured that against a from-scratch net in path 02.

Detection (drawing boxes) adds a head that predicts coordinates and a class. YOLOv8 nano is small enough to fine-tune here on a custom set if you collect one. It is an optional experiment, not a requirement, and it reuses the training discipline you already have. The extra idea is the target: a box is a regression plus a classification, and the matching between predicted boxes and true boxes needs a rule (IoU). Read that rule before you train one.

**Generation.** Diffusion models learn to remove noise. At sample time they start from noise and repeatedly denoise, conditioned on a text encoding. Stable Diffusion 1.5 LoRA is the adaptation that fits in 6 GB: the base stays frozen, a low-rank update learns a style or a subject from a handful of images, in on the order of one to three hours. Full training of a diffusion model, and comfortable SDXL fine-tunes, sit above this machine's VRAM.

## Experiment

Do one of the following and write it up with a baseline.

- **Subject LoRA:** 10–20 photos of one object you own, Stable Diffusion 1.5, a LoRA, and a prompt that puts the object in a new setting. Include failed samples. Judge them against the prompt with a checklist you wrote first (object present, setting present, obvious artifacts). This is qualitative. The checklist is what keeps it from being a vibe.
- **Or a tiny detector:** if you would rather stay in classification-shaped training, label boxes for one class with a free labeling tool, fine-tune YOLOv8 nano, and report precision and recall on held-out photos taken on a different day.

Also write one paragraph that places convnets, vision transformers, and diffusion on the map, in your own words, tied to what you have run.

## You are done when

The experiment's checklist or metric is in `notes.md`, and you can say which image jobs this 6 GB GPU is suited to.
