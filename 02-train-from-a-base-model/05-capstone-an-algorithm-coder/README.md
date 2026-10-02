# 05 — Capstone: an algorithm coder in one language

Prerequisite: [04 LoRA and QLoRA](../04-lora-and-qlora/README.md), [path 06 labs 01 and 03](../../06-evaluation-and-depth/README.md).

## Purpose

Adapt a 1–3B code model so it writes algorithms in one language, and measure it by running the code, not by reading a few nice samples.

## Explain before you code

1. Why is "passes hidden tests" the metric, and why is a pretty explanation not a substitute?
2. Why train on one language only?
3. What is an honest test problem, given that contest data is widely copied into model training sets?

## Data

Choose one:

- **Python:** [BAAI/TACO](https://huggingface.co/datasets/BAAI/TACO). Use the train split. Each row has a problem, Python solutions, and tests. Prefer one correct solution per problem so a few thousand problems stay in memory. Filter difficulty if you want a first run on easier problems.
- **C++ or Java:** [deepmind/code_contests](https://huggingface.co/datasets/deepmind/code_contests). Keep solutions whose language field is the one you chose, and keep correct solutions. The dataset also contains incorrect ones; those are useful later as a contrast and poison a first run.

Hold out your own validation slice from the train split (by problem, not by solution), and do not touch the dataset's official test split until the final measurement. Also write 10 fresh problems yourself, small but not copied from the set. Models may have seen public contests during pretraining. Your 10 problems are a partial check against that. They are too few to be a leaderboard. They are enough to keep you honest.

Format each training row as a prompt (the problem statement, and a line that names the required language) and a completion (one solution). Mask the loss so it falls on the solution.

Put the processed data on `D:\learn-ai-data\algo\`.

## Model

A 1.5B-class code model in 4-bit with a LoRA of rank 8 or 16, the setup you already rehearsed. A 3B model is allowed if lab 04's dress rehearsal fit at your sequence length. Sequence length 512 or 1024. Batch size 1 with gradient accumulation if you want a smoother update. Plug the laptop in. Close the browser.

One epoch over a few thousand problems is the planned run. It is on the order of 1–3 hours for 1.5B and longer for 3B. If loss is still falling cleanly and validation is still improving, a second epoch is a logged decision, not a default.

## Measurement

1. Untouched base model versus your adapter, same prompts, same decoding (greedy, or temperature 0).
2. On the official test split, once: generate one solution per problem for a fixed subset you size to an evening (for example 50 problems), run the public tests in a subprocess with a timeout. Record pass rate. Never execute generated code outside a folder and a timeout you set. Do not install packages the generated code asks for.
3. The same protocol on your 10 hand-written problems.
4. The three general prompts from lab 01, to see whether ordinary answers survived.
5. Twenty failures, read by you, tagged: wrong language, syntax error, wrong algorithm, off-by-one, timeout, or gave up. This tag list is the real result of the capstone. It tells you what a bigger model or cleaner data would be for.

## Build

`notes.md` holds the design (base model id, rank, sequence length, dataset, counts). `train.py` runs the fine-tune. `eval.py` generates and executes with a timeout. A results table compares base and adapter. Save the adapter to `D:\learn-ai-data\algo\adapter`.

## You are done when

You can quote a pass rate on held-out problems for the base and the adapter, and you can describe the main failure tag with examples. That model can sit behind the study agent in path 03, as a tool that proposes a solution you still test.
