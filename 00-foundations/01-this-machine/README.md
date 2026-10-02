# 01 — This machine

## Purpose

Every later decision about model size, batch size, and where files live follows from this PC. Write the constraints down once so the rest of the course can refer to them.

## The hardware

| Part | What you have | What it constrains |
|---|---|---|
| GPU | GeForce RTX 3060 Laptop | All serious tensor math goes here |
| VRAM | 6 GB (~5.8 GB free when the desktop is idle) | The weights, gradients, optimizer state, and activations of a training step must share this |
| CPU | Ryzen 7 5800H, 8 cores / 16 threads | Data loading, tokenization, and NumPy labs |
| RAM | 16 GB | The OS, the browser, and the data pipeline share this with Python. A 3B model load can fail here before the GPU is involved |
| Disk C: | about 75 GB free | Too tight for datasets and Hugging Face caches |
| Disk D: | about 931 GB free | Datasets, caches, checkpoints |

The GPU is Ampere, compute capability 8.6. It can train in float16 and bfloat16. That matters from path 02 onward: half precision is how a small adapter fine-tune fits.

## What each kind of work costs here

- NumPy labs and models under about 100 million parameters: comfortable, including training a tiny language model.
- A convnet on CIFAR, BERT-sized fine-tunes, YOLOv8 nano, Whisper-small fine-tunes, Stable Diffusion 1.5 LoRA: hours, and they fit.
- QLoRA on a 1–1.5B code model: the practical "adapt a coder" project. A few hours for a few thousand examples.
- QLoRA on a 3B model: possible with batch size 1, short sequences, and other apps closed.
- Full fine-tune of a 7B model, or pretraining one from random weights: the weights and optimizer state do not fit in 6 GB. You will compute that number in the from-scratch capstone.

## Rules for this PC

- Plug in before any training run. On battery the laptop GPU drops its clocks, so a "slow model" is sometimes just a hot, unplugged machine.
- Close the browser when a run is tight on RAM.
- Put caches on D: before the first download.

```powershell
$env:HF_HOME = "D:\hf-cache"
$env:HF_DATASETS_CACHE = "D:\hf-cache\datasets"
```

Set those in your user environment once you are in the environment lab, so a new terminal still sees them.

## Explain before you code

Write answers in `notes.md`.

1. What lives in VRAM during a training step, as distinct from system RAM?
2. Why can a fine-tune of a 1.5B model fit when training that same model from random initialization does not? (A full answer arrives in path 02. Write your current guess, and correct it later.)
3. Which drive will hold TACO or CodeContests if you download them, and why?

## Build

There is no model in this lab. Produce `notes.md` with the table above checked against a fresh `nvidia-smi` run, plus the three answers.

```powershell
nvidia-smi
```

Confirm the GPU name and that free memory is near 6 GB. If a game or browser is holding a large slice, write that down too. Training budgets assume a mostly idle GPU.

## You are done when

You can point at 6 GB, 16 GB, and drive D: and say which resource a given lab is about to run out of.
