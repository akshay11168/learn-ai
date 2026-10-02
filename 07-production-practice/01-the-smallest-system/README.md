# 01 — The smallest system

Prerequisite: one finished project in [path 04](../../04-small-use-cases/README.md). If you are deciding for an agent, also finish [path 03 lab 03](../../03-build-agents/03-tools/README.md).

## Purpose

Consultants get hired to pick a method, not to use the newest one. This lab is the decision record you will reuse in every proposal: the smallest system that can hit the metric, and the methods you rejected with a reason.

## Explain before you code

1. A task has a fixed form, a few dozen patterns, and a high cost when it is wrong. Which method do you reach for first, and what would justify a model later?
2. Retrieval answers from documents you provide. Fine-tuning changes weights. Which one do you want when the facts change every week?
3. An agent is a loop that can take actions. What extra failure appears the moment the system can call a tool, compared with a single prediction?

## The ladder

Use this order. Stop at the first rung that meets the metric on your validation set. Write down why you did not stop earlier, and why you did not climb higher.

| Rung | What it is | When it is enough |
|---|---|---|
| Rules | Keywords, thresholds, regular expressions, a checklist a person follows | The patterns are stable and you can list them |
| Classic model | Linear or tree model on features you define, including the baselines from path 01 and path 06 | Features are known, data is tabular or counts, you need a probability and a reason |
| One prompt | A fixed instruction to a model you do not train | The task is fuzzy, the volume is low, and you can check the outputs |
| Retrieval | Search your documents, then answer from the passages | The knowledge lives in files and changes |
| Fine-tune or adapter | Update weights on your pairs | Format or skill is stable, and prompting plus retrieval miss a measured bar |
| Agent | A loop with tools | The task needs several actions, and a single call cannot see the tool result |

Climbing the ladder adds cost, latency, and ways to be wrong. A fine-tune that loses to a keyword rule on your set is a failed decision even if the training curve looked healthy.

## Build

Pick the use case you will take through the rest of path 07. In `notes.md` write a one-page decision:

- The user, the input, the output, and the mistake that matters most.
- The metric and the minimum score you would ship (you may revise the number in lab 06, but write a draft now).
- The rung you stopped on, with the validation number.
- One sentence each for the rungs you skipped.
- What would force you up a rung later (volume, a new language, a measured miss).

If your existing project skipped a rung, run the simpler baseline now and put both numbers in the experiment log. You are allowed to keep the heavier system only when it wins on the metric you named.

## Study this step

**Concepts to master**

- The ladder: rules, classic model, one prompt, retrieval, fine-tune, agent. Climb only when the rung below misses a metric you wrote down first.
- Cost of a miss decides the rung as much as accuracy does. A rare, expensive miss argues for a rule or a human, not for a larger model.
- Facts that change live in documents or tables, not in weights. That is the retrieval-versus-fine-tune cut.
- An agent adds action risk. A single prediction cannot delete a file. A tool-using loop can, if you let it.

**Study**

- Google, "Rules of Machine Learning": https://developers.google.com/machine-learning/guides/rules-of-ml — rules about starting without ML, and about keeping a heuristic beside the model.
- Eugene Yan, "Patterns for Building LLM-based Systems": https://eugeneyan.com/writing/llm-patterns/ — read the patterns that match the ladder. Skip product theater.
- Applied LLMs essay, the sections on choosing where the model sits in the system: https://applied-llms.org/ — read part 1. Take the architectural choices, not the vendor names.

**Practice**

- For your use case, write one paragraph per rung you reject. Include a number for the rung you keep and the rung below it.
- Invent a second task, "route invoices to three folders by vendor name printed in a fixed box." Decide the rung in writing before you think about a model.
- Say the five-minute defense aloud once. If you say "AI" before you say the failure that matters, start over.

**Practice questions**

1. The format never changes and you have 40 labeled rows. Why is a fine-tune a suspicious first choice?
2. The policy PDF changes every Monday. Why is a fine-tune the wrong place to store it?
3. A keyword rule scores 0.92 and the model scores 0.93, and a mistake emails the wrong customer. Which do you ship, and what else must be true?
4. What new failure appears when you go from a classifier to an agent with a `send_email` tool?
5. Write the metric and the minimum score for your chosen use case in one sentence a non-specialist can check.

## You are done when

You can defend the choice in five minutes without saying "AI" until you have said what the system must not get wrong.
