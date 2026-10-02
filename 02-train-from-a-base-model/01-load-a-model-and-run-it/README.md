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

## Study this step

**Concepts to master**

- A pretrained checkpoint is a stack of tensors plus a config. Code rebuilds the modules from the config and fills the tensors. The tokenizer is a separate artifact and must match the checkpoint.
- Special tokens mark boundaries: beginning, end, padding, and often a chat turn. The chat template is a function that inserts those tokens. Skipping it means you are not running the model the way it was tuned.
- Generation is a loop: forward, choose a token, append, forward. Greedy is argmax. Sampling draws from the softmax of logits divided by temperature. `do_sample=False` is the reproducible setting.
- Context length is a hard window. Tokens beyond it are not "remembered worse". They are absent, unless the software truncates with a policy you should print.
- The model card's license and training-data statement constrain what you may ship later. "Open weights" is not one license.

**Study**

- Hugging Face NLP course, chapter 2 (using transformers) and the tokenizer chapter: https://huggingface.co/learn/nlp-course/chapter2/1 and https://huggingface.co/learn/nlp-course/chapter6/1 — run their pipeline example, then remove the pipeline and call the tokenizer and model yourself so you see the ids.
- Jay Alammar, "The Illustrated GPT-2": https://jalammar.github.io/illustrated-gpt2/ — the section on picking the next token. Your generator is that picture.
- The model card of the exact checkpoint you load. Read every heading. Copy parameter count, context length, and license into `notes.md` with the date.

**Practice**

- Encode one sentence, print tokens with `convert_ids_to_tokens`, change one word, and see which ids moved.
- Generate twice with `do_sample=False` and twice with temperature `0.8` and a fixed seed if the API allows. Record which pair matched.
- Feed a prompt longer than the context and print what the tokenizer or the model call actually kept.

**Practice questions**

1. Two tokenizers split "binary search" differently. Why can you not swap tokenizers between checkpoints?
2. The chat template adds a role token your manual string omitted. What behavior are you no longer testing?
3. Greedy decoding returned two different strings. What did you fail to fix: temperature, dropout in train mode, or a random seed in the sampler?
4. A card says context 2048. Your prompt is 3000 tokens. What does the model condition on?
5. Parameter count is 1.5B and you load in float16. About how many bytes are the weights, and why is that not the training cost?
6. The license forbids using the model to train a competing model, or requires attribution. Where in your later capstone notes does that constraint have to appear?

## You are done when

You can take a new model id, load it, and explain its prompt format from the card and a single encoded example.
