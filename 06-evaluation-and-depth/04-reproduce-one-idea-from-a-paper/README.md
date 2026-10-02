# 04 — Reproduce one idea from a paper

Prerequisite: the rest of path 06, plus the architecture the paper's idea sits on. This is the last lab of the course for a reason.

## Purpose

Take one idea from a paper and test a small version of it on this machine. The goal is not the original number. The goal is a controlled comparison you understand well enough to defend.

## How to shrink a paper

Authors train the largest model their cluster allowed, on the benchmark that makes the figure. You train the smallest model that still contains the idea, on data you can inspect. You change one factor: the idea on, the idea off. Everything else stays logged.

Ideas that shrink cleanly onto work you have already done:

- **Residual connections.** A 6-layer MLP on a vision or tabular task, with and without residual adds, same parameter budget as far as you can manage. This is the idea inside the transformer block, isolated.
- **Causal masking.** Your attention head, with the mask and without it, on the next-token task. Without the mask the loss will look better and the task will be invalid. Measuring that cheat is a real reproduction of why the mask exists.
- **LoRA rank.** You already have this as a stretch in path 02. Turning it into a paper-style write-up, with the original LoRA paper's claim in your own words, counts if the comparison is clean.
- **Learning-rate warmup.** A transformer run with and without a short warmup, loss curves overlaid.
- **Data augmentation.** The CIFAR convnet with and without flips, which you may already have run. Writing it up properly still counts if the log is complete and you reread a paper section that argues why augmentation helps.

Pick one. Do not pick "train the model in the paper." That is a different budget.

## Build

1. Choose the paper and copy the citation (title, authors, year, url) into `notes.md`.
2. Quote the claim you will test, in one or two sentences from the paper, and then in your own words.
3. Write the shrunk protocol before you run it: model, data, the single switch, the metric, the baseline (the switch off).
4. Run both settings. Log both rows.
5. Write a half-page: what matched the claim, what did not, what your setup cannot speak to (scale, data, tuning budget). If your result disagrees with the paper, the interesting question is whether the idea depends on scale. You are allowed to stop with that question clearly stated.

## You are done when

You can explain the paper's idea, your smaller test, and the limit of what your test shows, without inflating it into the paper's result.
