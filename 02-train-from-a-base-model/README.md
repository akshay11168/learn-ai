# 02 — Train from a base model

Pretraining a large model spends a cluster's time so that the weights already predict language, code, or visual features. Adaptation spends your evening moving those weights toward a task you care about. This path is that second stage. It is the practical way to get an algorithm coder on the RTX 3060.

You should already be able to read a training loop (path 01 lab 04) and explain next-token prediction (path 01 lab 06). The from-scratch capstone can still be in progress. Do lab 01 here as soon as you want to see a real pretrained model answer a prompt. Save labs 04 and 05 until attention is no longer a black box.

## What is being transferred

Early in training, a vision net's first layers become edge and texture detectors. Later layers become more tied to the classes it was trained on. If you keep the early layers and replace the last linear map, a small set of your own photos can steer the model. That is transfer learning, lab 02.

A language model pretrained on next-token prediction already represents syntax and a lot of common knowledge in its weights. Full fine-tuning continues the same kind of update on every weight, on your examples. On a small model that fits in 6 GB, that is lab 03. On a 1.5B code model, the Adam state for every weight does not fit. LoRA trains a low-rank update beside the frozen weights. QLoRA stores the frozen weights in 4 bits so the base model itself fits. That is lab 04, and it is how the algorithm-coder capstone runs on this laptop.

Continued updates pull the model toward the new data. Skills that the new data never exercises can fade. Adapters limit the fade because the original matrix stays put and a small update is added at runtime. You will measure this, not just repeat it: in lab 03, score the base model on a handful of general prompts before and after a narrow fine-tune.

## Labs

| Lab | You come out able to |
|---|---|
| [01 Load a model and run it](01-load-a-model-and-run-it/README.md) | Tokenize text, generate a completion, and read a model card |
| [02 Transfer learning on images](02-transfer-learning-on-images/README.md) | Freeze a pretrained convnet and train a new head on your photos |
| [03 Fine-tune a small text model](03-fine-tune-a-small-text-model/README.md) | Continue training every weight of a model that fits |
| [04 LoRA and QLoRA](04-lora-and-qlora/README.md) | Explain the memory math and train an adapter |
| [05 Capstone: an algorithm coder](05-capstone-an-algorithm-coder/README.md) | Adapt a 1–3B code model to one language on contest problems |
| [06 Preference tuning](06-preference-tuning/README.md) | Explain the training stage that teaches a model which answer to prefer |

## Datasets you will actually use

Keep downloads under `D:\hf-cache` and `D:\learn-ai-data`.

- Image lab: your own photo folders, plus a public set only if you need a dry run (CIFAR is already on disk from path 01).
- Small text fine-tune: a few thousand pairs you can read, or a slice of a public instruction set whose card you accept.
- Algorithm coder: [BAAI/TACO](https://huggingface.co/datasets/BAAI/TACO) for Python (problems, solutions, tests, algorithm tags), or [deepmind/code_contests](https://huggingface.co/datasets/deepmind/code_contests) if you want C++ or Java. CodeContests includes wrong solutions; keep the correct ones. Leave the official test split untouched until the end.
- Use one language per run. A mixture teaches the model to switch languages mid-answer.

A few thousand pairs are enough for a 1.5B QLoRA run and finish in a few hours. Multi-million-row synthetic sets with long reasoning traces (for example the large Nemotron competitive-programming releases) do not fit this machine's RAM or VRAM. You may read about them in path 05.

## Done with this path when

- You can load a model, name its tokenizer, and generate with a temperature you chose on purpose.
- A frozen-backbone image classifier beats a from-scratch convnet on a small photo set you collected, or you can explain with your logs why it did not.
- A QLoRA run on a 1–3B coder improves your held-out algorithm checks over the untouched base model, in the language you chose.
- You can say what full fine-tuning, LoRA, QLoRA, and a preference stage each update.
