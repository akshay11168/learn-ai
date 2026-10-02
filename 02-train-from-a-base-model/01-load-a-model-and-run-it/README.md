# 01 — Load a model and run it

Prerequisite: [path 01 lab 04](../../01-train-from-scratch/04-autograd-and-the-training-loop/README.md) and [lab 06](../../01-train-from-scratch/06-tokens-embeddings-and-a-tiny-language-model/README.md).

## Purpose

Run a pretrained model before you change it. You need to see the tokenizer, the chat or completion format, the sampling knobs, and the model card. Fine-tuning a model you have never run is how people "improve" a metric and break the behavior they cared about.

## Explain before you code

1. What is a token id, and why can the same word be one token in one tokenizer and three in another?
2. Temperature and top-k change the sample. Which one would you set if you wanted a repeatable, nearly greedy answer?
3. What does the model card claim about training data and acceptable use? If the card is thin, what do you not know?

## Build

Install, in the course venv:

```powershell
pip install transformers accelerate
```

`HF_HOME` should already point at `D:\hf-cache`.

Load a small model that fits in 6 GB for inference with room to spare. A 1B–1.5B class model is the right call (`Qwen2.5-Coder-1.5B` or a newer 1–1.5B coder or general model with a clear model card). Use the Hugging Face id printed on the model card you actually read, and pin that id in `notes.md` so the lab stays reproducible.

Write `generate.py` that:

1. Loads the tokenizer and the model in float16 on CUDA.
2. Prints the number of parameters and the tokenizer vocabulary size.
3. Encodes one sentence and prints the token ids and the decoded pieces (`convert_ids_to_tokens`).
4. Generates a continuation of a plain text prompt at temperature 0 (greedy) and at temperature 0.8.
5. If the model card documents a chat template, send one system-style instruction and one user question through `apply_chat_template`, and generate again. Write down the special tokens the template inserted.
6. Times a short generation and records tokens per second roughly (new tokens divided by seconds).

Try three prompts you will reuse after every fine-tune in this path: one general question, one "write a Python function to binary-search a sorted list," and one in the language you think you want for the capstone if it is not Python. Save the outputs in `notes.md` under "before any training."

Read the model card end to end. Copy into your notes: parameter count, license, languages if stated, and any limit on context length.

## Verify

Greedy decoding is deterministic across two runs. The token printout shows that you know where words were split. The three saved prompts exist and will be the regression checks for labs 03–05.

## Stretch

Run the same prompt through your path 01 tiny transformer and through this model. The point of the comparison is scale and data, which you already estimated in the capstone. Write three concrete differences in the outputs (syntax, stopping, following the question).

## You are done when

You can take a new model id, load it, and explain its prompt format from the card and a single encoded example.
