# learn-ai

A from-scratch study of how models learn, how a trained model is adapted, how an agent is built around a model, and how the same training loop shows up in text, images, and sound. The last three paths turn that skill into paid work: ship a system to a standard you can defend, consult, and only then open a firm.

This folder is the course. Each numbered directory is a path. Each directory inside a path is one lab: read it, implement it in that folder, and move on only when you can answer its questions out loud.

You are starting with no AI background. The order below is the order that builds understanding. Skipping ahead produces code that runs and a mental model that does not.

## The machine this course is written for

- CPU: AMD Ryzen 7 5800H (8 cores)
- RAM: 16 GB
- GPU: NVIDIA GeForce RTX 3060 Laptop, 6 GB VRAM
- Disk: keep code in this repo; put datasets and model caches on `D:` (`D:\hf-cache`, `D:\learn-ai-data`)
- The NVIDIA driver on this PC is old (512.74, CUDA 11.6). Update it before installing PyTorch. The lab for that is [00-foundations/02-environment](00-foundations/02-environment/README.md).

Every project in this course is sized so it can run here, plugged in, with other heavy apps closed. Training a frontier model from scratch needs a cluster. That limit is part of the study, and it is written up in [01-train-from-scratch/08-capstone-pretrain-a-tiny-model](01-train-from-scratch/08-capstone-pretrain-a-tiny-model/README.md).

## How a lab works

1. Read the lab once before writing code.
2. In that folder, create `notes.md` and answer the "explain before you code" questions in your own words.
3. Implement the smallest version that can run.
4. Record the numbers the run printed (loss, accuracy, a few examples).
5. Change one thing. Predict the effect, then run it, then write whether you were right.
6. Leave the lab when you can answer "you are done when" without looking at the notes.

Code, notes, and plots for a lab live in that lab's folder. Datasets and weights live on `D:` and are gitignored.

## Paths

| Path | What you learn | When you start it |
|---|---|---|
| [00 Foundations](00-foundations/README.md) | The machine, the tools, arrays, the math the training loop uses, and what "learned" means | First |
| [01 Train from scratch](01-train-from-scratch/README.md) | You write the model and the update rule, from a line fit through a small transformer | After 00 |
| [02 Train from a base model](02-train-from-a-base-model/README.md) | You start from weights someone else trained and adapt them | After 01 labs 01–04. The coder capstone waits until 01 is finished |
| [03 Build agents](03-build-agents/README.md) | A loop around a model: instructions, tools, retrieval, memory | After 02 lab 01, and after you have a retriever from 04 lab 03 |
| [04 Small use cases](04-small-use-cases/README.md) | Finished, narrow tools on your own data | Each lab names the technique lab it depends on |
| [05 Modalities and trends](05-modalities-and-trends/README.md) | Language, images, and sound as three views of one loop, plus how to follow the field | Read alongside 01 and 02. Do the experiments when the linked lab is done |
| [06 Evaluation and depth](06-evaluation-and-depth/README.md) | Metrics, baselines, failed runs, an experiment log, and reproducing one idea from a paper | Start 06 lab 01 as soon as you have a first trained model (01 lab 02) |
| [07 Production practice](07-production-practice/README.md) | The smallest system that solves a real task, then serving, cost, security, release evals, and a handover | After 04 has one finished use case, and after 03 if the system is an agent |
| [08 Consulting](08-consulting/README.md) | A wedge, a portfolio, discovery, a statement of work, pricing, and a rehearsed engagement | After 07 labs 01–03. The full rehearsal waits until one use case is packaged (04 lab 05) |
| [09 The firm](09-the-firm/README.md) | When a company is useful, contracts, client data, a repeatable offer, and the first hire | After you have delivered at least one paid pilot, or a full rehearsal you would show a buyer |

Paths 00 and 06 exist because the modeling work depends on them. Paths 07–09 exist because a buyer pays for a system that holds up, a clear engagement, and a firm that can sign and deliver. Company form, tax, and insurance in path 09 are questions for a chartered accountant and a lawyer in your country. The labs teach you what to ask them. They are not legal or tax advice.

## Order to follow

```text
00 Foundations
        |
        v
01 Train from scratch -----> 06 Evaluation (start early, keep using it)
        |
        +----> 04 Use cases, as each prerequisite lab is done
        |
        v
02 Train from a base model
        |
        +----> 05 Modalities, one experiment per area
        |
        v
03 Agents
        |
        v
07 Production practice
        |
        v
08 Consulting
        |
        v
09 The firm   (after a paid pilot, or a rehearsal good enough to sell)
```

Stay inside path 01 until the NumPy network and the PyTorch training loop are yours (labs 03 and 04). That is the point where the rest of the field becomes readable. Agents come after the model is understandable. Production, consulting, and the firm come after you can ship one narrow system and measure it. Opening a company does not create either of those.

## What each path is for

**Train from scratch.** A model is a function with numbers in it. Training is changing those numbers so the function's mistakes get smaller. You will fit a line, derive a small network by hand, check your gradient against a numerical one, then let PyTorch do the bookkeeping. After that you will train a convnet, a tiny language model, and a small transformer on this GPU. The last lab states, with arithmetic, why a 7B code model is a cluster job.

**Train from a base model.** Most useful models are not trained from random weights. Someone already spent the cluster time. You load those weights, see what they do, then adapt them: a new image head, a full fine-tune of a small text model, and LoRA / QLoRA when the base model does not fit in 6 GB as a full fine-tune. The capstone is the algorithm coder discussed earlier: a 1–3B code model adapted on contest problems in one language.

**Agents.** You will build the loop yourself before you adopt a framework. The model proposes the next action. Your code runs the tool. The tool's result goes back into the model. You will watch it call the wrong tool, loop, and invent a result, and you will write tests that catch those failures.

**Small use cases.** A classifier, a photo sorter, a notes question-answerer, a short-sound recognizer, and one of those wrapped as a tool you can run next month without rereading the lab. Narrow on purpose. Depth here means your own data, a baseline, and an error analysis.

**Modalities and trends.** Text, images, and audio are different inputs to the same idea: turn the input into tensors, predict, measure the miss, update. You will do one serious experiment in each, and you will learn a way to read model cards and papers so the map stays current after this course.

**Production practice.** Industry work is a decision about the smallest reliable system, then an interface, a cost, a threat model, a golden-set eval that can block a release, and a runbook another person can operate. You will put one of your own use cases through that sequence.

**Consulting.** You will specialize, turn two projects into case studies a buyer can read, run a discovery, write a statement of work with a metric, price a short pilot, and rehearse the whole engagement on a problem you already solved. The technical depth from paths 01–07 is what makes the proposal true.

**The firm.** A firm is a way to contract, insure, and hire around work you already know how to sell. You will list the decisions a lawyer and an accountant must make for your country, write the data and IP rules you will not negotiate away, and define one offer you can deliver twice.

## Time, roughly, at a few evenings a week

| Block | Calendar time |
|---|---|
| 00 Foundations | 2–3 weeks |
| 01 Through the training loop (labs 01–04) | 3–5 weeks |
| 01 Vision, language, transformer, capstone | 6–10 weeks |
| 02 Adaptation and the coder | 4–6 weeks |
| 04 Use cases, interleaved | a weekend to a week each |
| 03 Agents | 3–4 weeks |
| 07 Production practice | 3–5 weeks, on one use case you already built |
| 08 Consulting | 3–4 weeks of writing and rehearsal, beside the technical work |
| 09 The firm | After the first paid pilot. The labs themselves are one to two weeks of preparation, plus the professionals you hire |
| 05 and 06 | continuous, a few hours on top of the lab you are in |

These are study estimates, not training-time estimates. A single training run in the later labs is minutes to a few hours on the 3060.

## Working rules

- Implement the mechanism in the smallest code that shows it, then use the library that hides it. The library is earned.
- One change per run. Write the prediction first.
- Keep a baseline. A model that cannot beat "always guess the most common class" has not learned the task.
- Plug the laptop in for every training run. On battery the 3060 clocks down and the timings in these labs stop meaning anything.
- Contest datasets (TACO, CodeContests, APPS) are for study on this machine. Read the dataset card before you redistribute data or a model trained on it.
