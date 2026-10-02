# 08 — Capstone: pretrain a tiny model

Prerequisite: [07 A small transformer](../07-attention-and-a-small-transformer/README.md) and [06 Evaluation](../../06-evaluation-and-depth/README.md) labs 01–03.

## Purpose

Pretrain a small model from random weights on code in one programming language, measure what it can and cannot do, and compute the hardware required for the next sizes up. This is the "train from scratch" destination on the machine you actually have.

## Explain before you code

1. What is the training objective, in one sentence, and how does it differ from "answer this programming problem"?
2. Roughly how many bytes does Adam store per parameter if you keep a float32 master copy and two float32 optimizer moments, and the forward/backward values are float16? Use 2 (weights) + 2 (gradients) + 4 + 4 + 4 = 16 bytes as the working figure for model state, and remember activations are extra.
3. At 16 bytes of state per parameter, how much memory is the state of a 1.5B model, before activations?

## What you are training

A decoder-only transformer, the lab 07 design scaled up as far as 6 GB allows with room for activations. A practical target is on the order of 10–50 million parameters, context 128 or 256, one language, character-level or a small byte-level encoding you implement yourself.

Data: a few tens of megabytes of real code in one language is enough to see structure (indentation, brackets, common keywords). A few hundred megabytes is better if you have it. Sources that are appropriate for study: your own code, a permissively licensed repository you clone, or a public code dataset whose card you have read. Put it on `D:\learn-ai-data\code\`. Do not commit it.

Split by file, not by random lines, so a function's body is not in both splits.

Train until validation loss flattens. Checkpoint the best validation loss, not the last epoch.

## What to measure

Next-token loss on the validation files is the training metric. Also evaluate tasks the loss does not fully describe:

- Given a line with the last few characters blanked, how often does a temperature-0 continuation match the file?
- Given `"def "` or a C++ `"for ("`, do samples continue with plausible syntax?
- Hand-write 10 tiny prompts ("write a loop that sums a list" as a comment, or the first line of a function). Save the completions. Mark each as syntax-ok or not, by compiling or by eye against a checklist you wrote beforehand.

This model will pick up local syntax and repetitive idioms. It will not reliably solve algorithm problems. That gap is the reason path 02 exists: the models that do solve simple problems were pretrained at a size and token count this GPU cannot host, then adapted. Write the observed gap in `notes.md` with your 10 prompts as evidence.

## The hardware line

Using about 16 bytes of optimizer-and-weight state per parameter:

| Model | State alone | On this 6 GB GPU |
|---|---|---|
| 30 million | ~0.5 GB | Fits, with room for activations. This capstone |
| 300 million | ~4.8 GB | State barely fits; activations push you to tiny batches and short context |
| 1.5 billion | ~24 GB | Does not fit. A 24 GB desktop GPU is the entry for a small from-scratch pretrain |
| 7 billion | ~112 GB | A multi-GPU server |

Published reference points, so the table stays attached to real runs:

- TinyLlama, 1.1B parameters, was pretrained on 16 A100 40 GB GPUs (on the order of a trillion tokens, far past the compute-optimal minimum, which is why the model is strong for its size).
- StarCoder, 15.5B parameters, was pretrained on 512 A100 80 GB GPUs for 24 days on 1 trillion tokens of code.

Activations, sequence length, and batch size sit on top of the state column. That is why "the weights file is 3 GB" does not mean "training needs 3 GB."

## Build

1. A short design note in `notes.md`: parameter count, context length, vocabulary, dataset size in bytes and tokens, train/validation file split.
2. The training run, logged (path 06 lab 03).
3. The 10-prompt qualitative test and the blanked-line score.
4. The memory table, filled with your model's measured parameter count (`numel`) rather than only the estimate.
5. A paragraph: what this model learned, what the path 02 base model will already know before you fine-tune it, and which hardware row you would rent if the goal were a 1.5B model from random weights.

## Study this step

**Concepts to master**

- Pretraining here means next-token training from random initialization on a raw corpus. It does not mean the model can follow instructions. Instruction following is a later stage on different data.
- Parameter count is `sum of numel`. Training state is on the order of 16 bytes times that count for mixed-precision Adam, plus activations that grow with batch size and context length.
- Data order and the split unit matter more than another layer. Splitting by file tests whether you learned the language of the codebase. Splitting by random lines tests whether you memorized the other half of the function.
- Qualitative prompts are a behavioral test, not a benchmark. A benchmark needs a frozen set and a script. You owe yourself both.
- Scale laws are a reason, not a mood: loss falls as you add parameters, tokens, and compute together. A 30M model on tens of megabytes is a different point on that surface from a 1B model on 100B tokens.

**Study**

- Karpathy, "Let's build GPT" / nanoGPT readme, the section on training a small model and the hyperparameter table: https://github.com/karpathy/nanoGPT — compare his 6-layer Shakespeare config to yours. Write down context length, batch, and parameter count side by side.
- The Chinchilla paper's abstract and the figure that trades parameters against tokens (Hoffmann et al., 2022): https://arxiv.org/abs/2203.15556 — you only need the claim: for a fixed compute budget there is a best model size, and undertrained large models are common. Do not implement their formula. Do compute 20 tokens per parameter for your model and for a 1.5B model.
- TinyLlama report, the training setup section: https://arxiv.org/abs/2401.02385 — extract GPU count, tokens, and parameter count. That is the "next size" in concrete nouns.
- StarCoder model card, the training section: https://huggingface.co/bigcode/starcoder — extract the same three numbers for a real code model.

**Practice**

- Count parameters with code and compare to a hand estimate `layers * (attention + MLP + norms)`. Reconcile to within 10%.
- Measure one step's wall time and the tokens in the step. Extrapolate to one epoch of your corpus. Write the extrapolation next to what the run actually took.
- Take five blanked lines from the validation files and score exact-match of a greedy completion. That number goes in the notes beside the loss.

**Practice questions**

1. Your model has `2e7` parameters. At 16 bytes of Adam state per parameter, how many gigabytes is the state, and what else is still uncounted?
2. A 1.5B model needs about how much state memory at that same 16 bytes? Why is "load it in 4-bit and train all weights" still a different and larger number?
3. You split by randomly assigning lines of each file to train or validation. Give a concrete example of a leak.
4. Validation loss improved and 0 of 10 hand prompts compile. What claim are you allowed to make?
5. Chinchilla's rough 20 tokens per parameter says a 30M model wants on the order of how many tokens? If your corpus is 5M tokens, which side of that balance are you on, and what behavior should you expect (memorization or undertraining)?
6. Why does a low next-token loss on Python not imply the model can solve a new programming problem?

## You are done when

You have a checkpoint, a validation loss, a small behavioral test, and a written account of why the next serious size is a different computer. Then start [path 02](../../02-train-from-a-base-model/README.md) if you have not already opened lab 01 there.
