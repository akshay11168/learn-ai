# 04 — LoRA and QLoRA

Prerequisite: [03 Fine-tune a small text model](../03-fine-tune-a-small-text-model/README.md) and [path 01 lab 07](../../01-train-from-scratch/07-attention-and-a-small-transformer/README.md).

## Purpose

Understand the adapter math, then train a LoRA (and a 4-bit QLoRA if you move to a larger base) on this GPU. The capstone depends on this lab.

## Explain before you code

1. A weight matrix `W` is shape `(d, d)`. A LoRA pair `B @ A` with rank `r` uses matrices of shape `(d, r)` and `(r, d)`. How many parameters is that, compared with `d * d`, for `d = 1024` and `r = 8`?
2. During training, which of `W`, `A`, and `B` receive gradients?
3. QLoRA stores `W` in 4 bits and keeps the adapter in higher precision. What does that change about VRAM, and what does it leave unchanged about the adapter?

## Concepts

Full fine-tuning stores gradients and Adam moments for every weight. At ~16 bytes of state per parameter, a 1.5B model wants on the order of 24 GB before activations. The 3060 has 6 GB.

LoRA freezes `W` and learns a low-rank update `ΔW = B A`, scaled by a constant (`alpha / r`). The forward pass uses `W x + B A x`. Because `A` and `B` are thin, the trainable state fits easily. Rank `r` is the capacity of the update. Rank 4 is a small nudge. Rank 64 is a much larger one, and it costs more memory and data to estimate well. Start at 8 or 16.

You attach LoRA to attention projections (query and value are the common first choice) and sometimes to the MLP. Write down which modules you targeted. "I trained LoRA" is incomplete without that.

QLoRA quantizes the frozen base to 4 bits (typically NF4), dequantizes tiles of it on the fly for the matmul, and trains the LoRA weights in float16 or bfloat16. The base model's quality drops slightly from quantization. The memory drop is what makes 1–3B adaptation possible here. A 7–8B QLoRA is a maybe on 6 GB and often runs out of memory once the sequence is long. Do not plan the capstone on it.

Inference after training can merge `ΔW` into `W` so the deployed model is a normal checkpoint, or it can keep the adapter as a small file beside the frozen base. The adapter file is the artifact you will save. It should be megabytes, not gigabytes. Check the file size and write it down.

## Build

Use the small model from lab 03 first, so you can compare a LoRA fine-tune to the full fine-tune on the same data and split.

1. Implement a toy LoRA by hand on one `nn.Linear`: frozen `W`, trainable `A` and `B`, forward `F.linear(x, W) + (x @ A.T @ B.T) * scale` or the equivalent that matches your shapes. Train it on a silly regression or on the lab 03 task's last layer only. Confirm `W.grad is None` and `A.grad` is not.
2. Then use the `peft` library for the real model. `pip install peft`. Configure rank, alpha, dropout, and target modules in code, not from memory of a blog. Print the number of trainable parameters versus total parameters. You want a fraction of a percent to a few percent trainable.
3. Train on the lab 03 dataset with the same answer-only loss if it was a generator. Compare validation score to the full fine-tune and to the untouched base.
4. Save the adapter. Load the base and the adapter in a fresh process and regenerate the three lab 01 prompts plus a validation sample.
5. If step 3 fits easily, repeat the same adapter setup on a ~1.5B model in 4-bit (`bitsandbytes`) for a few hundred steps only, as a dress rehearsal for the capstone. `pip install bitsandbytes`. If the import or the GPU kernel fails, record the error and the driver version; do not skip the note. A successful short run is enough. The full dataset belongs to lab 05.

Watch VRAM with `nvidia-smi` during the 4-bit run and record the peak you observe.

## Verify

The hand-written LoRA shows a frozen `W`. The PEFT run reports trainable parameter count that matches your rank calculation up to the list of target modules. The adapter reloads in a new process. You have one table comparing base, full fine-tune, and LoRA on the same validation set.

## Stretch

Train rank 4 and rank 32 for the same number of steps. Compare validation score and adapter file size. Write what extra rank bought you on this particular dataset.

## You are done when

You can explain, with a parameter count and a VRAM reading from your own machine, why the capstone is QLoRA.
