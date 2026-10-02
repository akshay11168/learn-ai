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

## Study this step

**Concepts to master**

- VRAM holds the tensors of the current step: weights, gradients, optimizer state, and activations. System RAM holds the Python process, the data loader, and copies you have not moved to the GPU.
- A weight file's size is not the training memory. Optimizer state is several times the weights.
- Disk, RAM, and VRAM fail differently. A crash that names CUDA out-of-memory is VRAM. A crash that freezes the whole machine is usually system RAM. A download that stops because the drive is full is disk.
- Laptop GPUs share a power and heat budget with the CPU. Clocks drop on battery and when the chip is hot, so timings are only comparable when the machine is plugged in.

**Study**

- NVIDIA's own `nvidia-smi` manual page, the sections on memory usage and GPU utilization: https://developer.nvidia.com/nvidia-smi — run each flag you don't know and write what it printed.
- PyTorch's note "CUDA semantics", the part on memory: https://pytorch.org/docs/stable/notes/cuda.html — read "Memory management" only. You want the idea that caching allocator reserved memory is not the same as allocated memory.
- Tim Dettmers' "Which GPU for deep learning?" (timdettmers.com) — read the sections on memory bandwidth and why VRAM capacity dominates hobby training. Ignore the shopping list. Extract the rule that capacity, not the logo, decides which model fits.

**Practice**

- Fill a three-row table from a live `nvidia-smi`: total VRAM, used, free. Repeat with a browser open and with it closed.
- Write the path you will use for `HF_HOME` and for datasets. Create the directories.
- For an imaginary 100 MB weight file, state which resource is fine and which you have not measured yet (optimizer state).

**Practice questions**

1. A checkpoint is 2 GB on disk. Why can training still fail on a 6 GB GPU?
2. Training is slow, `nvidia-smi` shows 0% GPU utilization, and the Python process is busy. Which resource is the bottleneck, and what is it probably doing?
3. You have 16 GB of system RAM and Windows is using 6 GB. A data pipeline wants to pin 12 GB of batches. What fails?
4. Why is "the GPU has 6 GB, so I can train a 6 GB model" a wrong sentence?
5. Name one lab later in this course that is limited by VRAM, one limited by system RAM, and one that is fine on the CPU. Justify each in a clause.

## You are done when

You can point at 6 GB, 16 GB, and drive D: and say which resource a given lab is about to run out of.
