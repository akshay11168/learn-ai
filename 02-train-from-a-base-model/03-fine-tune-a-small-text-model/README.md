# 03 — Fine-tune a small text model

Prerequisite: [01 Load a model](../01-load-a-model-and-run-it/README.md).

## Purpose

Continue training every parameter of a model small enough that full fine-tuning fits in 6 GB. Feel the difference between "the base model already could do this" and "the fine-tune changed it," using the three prompts you saved in lab 01.

## Explain before you code

1. If you fine-tune on next-token loss, what tokens are in the target? Why might you mask the loss on the prompt tokens and train only on the answer tokens?
2. A learning rate of `1e-3` was reasonable for a tiny net from scratch. Why is that a violent learning rate for a pretrained model?
3. What would you measure to notice that a narrow fine-tune damaged a general prompt from lab 01?

## Concepts

Full fine-tuning is the path 01 loop aimed at an existing checkpoint. The weights are not random. Gradients should be small updates around a working model, so learning rates are often `1e-5` to `5e-5` for full fine-tunes of small transformers. You will try one rate that is clearly too high and record the loss exploding or the samples turning to junk. That failed run is part of the lab.

Loss masking: the prompt is context, the answer is the skill. If the prompt dominates the token count, the model spends capacity reproducing the question. Masking prompt tokens in the loss (set their labels to `-100` so cross-entropy ignores them) focuses the update on the answer. Implement it once so you understand the shapes, even if a trainer class offers the option later.

Overfitting a dozen examples until the model repeats them verbatim is a useful control. It proves the loop updates the right weights. It is not a finished system. Your real split needs enough validation pairs that repetition would be visible as a gap between train and validation loss.

## Build

Pick a small pretrained model that fits in VRAM together with Adam for a short sequence. A model well under 500M parameters is the comfortable choice on 6 GB for full fine-tuning (gradients plus Adam state dominate). A distilled encoder like DistilBERT is appropriate if you would rather fine-tune a classifier (pair a sentence with a label) than a generator. Do one of these two, and write down which objective you chose:

- **Classifier:** a few thousand short texts, two or more labels you define (for example, bug report versus feature request, from sentences you write or a small public set). Head on top of the encoder, cross-entropy, metric is macro F1 because the classes may be uneven.
- **Generator:** a few hundred to a few thousand prompt/answer pairs in one narrow format. Causal language model loss, answers only. Keep sequences short (128–256 tokens) so the batch fits.

In both cases:

1. Score the untouched model on the validation set and on the three lab 01 prompts (for a classifier, the "score" on prompts can be a short generation if the model is generative; otherwise skip prompts and keep a handful of qualitative errors).
2. Fine-tune with a sane learning rate and with a deliberately high one.
3. Score again. Save a table: baseline, untouched model, fine-tuned model.
4. Read 20 validation mistakes. Write the pattern.

Use the training loop you wrote in path 01, or Hugging Face `Trainer` after your own loop has completed one epoch correctly. If you use `Trainer`, read the log and point at the line that is your `backward` and your `step`.

## Verify

The sane run beats the untouched model on validation. The too-high learning rate run is documented as a failure. The lab 01 general prompt is rerun, and your notes say whether the answer held up.

## Stretch

Fine-tune twice, once with answer-only loss and once with loss on the full sequence. Compare validation scores and a sample. This is a small empirical paper. Write it as five sentences in `notes.md`.

## You are done when

You can justify the learning rate from the failed run, and you can say which tokens received loss.
