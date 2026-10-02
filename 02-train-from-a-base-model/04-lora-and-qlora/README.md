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

## Study this step

**Concepts to master**

- LoRA replaces a full update `ΔW` with a low-rank product `BA`, rank `r`. For a square matrix of width `d`, full fine-tuning trains `d²` numbers and LoRA trains `2dr` per adapted matrix, times the modules you target.
- Only `A` and `B` receive optimizer state. `W` is frozen. If `W` has a gradient, it is not LoRA.
- Alpha scales the update, commonly `alpha / r`. Doubling rank without thinking about alpha changes the step size.
- QLoRA quantizes the frozen base, usually to 4-bit NF4, and dequantizes for the matmul. The adapter stays in higher precision. Quantization error is the price of the memory cut.
- Merging adds `BA` into `W` for deployment. Keeping the adapter separate lets you swap tasks without copying the base.
- Target-module choice is part of the method. Query and value projections are the usual default, not a law of nature. Your write-up names the modules.

**Study**

- LoRA paper, abstract, Figure 1, and Section 4 (the low-rank hypothesis discussion): https://arxiv.org/abs/2106.09685 — recompute their parameter-reduction example with `d = 1024`, `r = 8`.
- QLoRA paper, abstract and the memory Figure: https://arxiv.org/abs/2305.14314 — extract what is quantized and what is not.
- Hugging Face PEFT docs, the LoRA conceptual guide and the configuration fields: https://huggingface.co/docs/peft/main/en/conceptual_guides/lora — then print `get_nb_trainable_parameters()` and match it to your hand count.
- bitsandbytes 4-bit integration notes in the Transformers quantization guide: https://huggingface.co/docs/transformers/main/en/quantization/bitsandbytes — read the "we do not quantize the adapters" implication. If the library errors on your GPU, the driver note in your lab log is the study artifact.

**Practice**

- Hand-count trainable parameters for rank 8 on `q_proj` and `v_proj` only, given the hidden size from `model.config`. Compare to PEFT's number.
- Watch `nvidia-smi` during a 4-bit step and a float16 LoRA step on the small model. Write both peaks.
- Save the adapter, restart Python, load base plus adapter, and check one prompt against the session that trained it.

**Practice questions**

1. `d = 2048`, `r = 16`, one matrix. How many LoRA parameters, and what fraction is that of `d²`?
2. You double `r` and leave `alpha` fixed, with scale `alpha / r`. What happens to the initial magnitude of `ΔW` if `A` and `B` start small?
3. Why does QLoRA not cut the activation memory by 4× when the sequence gets long?
4. Adam state is about 8 extra bytes per trainable parameter in a common mixed-precision setup, on top of the trainable weights. For `1e6` trainable adapter parameters, how much optimizer memory is that, and why is the 1.5B base still the thing that had to be quantized?
5. A merged checkpoint and an adapter-plus-base pair should produce the same greedy tokens on a fixed prompt. What bug does a mismatch reveal?
6. Rank 32 beat rank 4 on training loss and tied on validation. What do you ship, and why?

## You are done when

You can explain, with a parameter count and a VRAM reading from your own machine, why the capstone is QLoRA.
