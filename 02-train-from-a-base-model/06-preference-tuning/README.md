# 06 — Preference tuning

Prerequisite: [05 Capstone](../05-capstone-an-algorithm-coder/README.md). This lab is conceptual plus a small experiment. It is here because modern chat and code models go through a stage after next-token training, and skipping it leaves a hole in the map.

## Purpose

Understand what a preference stage changes, and run a tiny version on pairs you labeled yourself.

## Explain before you code

1. Next-token training rewards imitating the dataset, including its bad solutions. What signal does a pair "(this answer is better than that one)" add?
2. Why is a reward model a learned function, and what is DPO doing so that the same preference can train the policy without a separate reward model in the loop?
3. What goes wrong if the "preferred" answers were written by a stronger model and your small model can imitate their style without becoming more correct?

## Concepts

Instruction tuning (which you already did, in a narrow way, by training on problem/solution pairs) teaches the format of following a request. Preference tuning teaches a ranking between two completed answers. The usual pipeline is: sample two answers, have a person or a test suite mark the better one, then update the model toward the winner and away from the loser, while a penalty keeps it from drifting arbitrarily far from the model you started the stage with.

RLHF uses a learned reward model plus a reinforcement-learning loop. DPO writes a classification-style loss directly on the pair, so the implementation is closer to the supervised losses you already know. You do not need to re-derive the DPO paper's equation from scratch on the first read. You do need to implement the shape of the update on a toy: preferred completion gets a higher relative probability than the rejected one, and a copy of the reference model anchors the change.

On code, a test suite is a preference oracle that does not require a human: a completion that passes tests is preferred to one that fails. That is the cleanest small experiment available to you.

## Build

1. Read the DPO paper's abstract, figure 1, and the loss section, slowly. In `notes.md`, restate the loss in your own words and name the two models it references (the one being trained, and the frozen reference).
2. From the capstone model, generate two completions for 20 validation problems. Mark the one that passes tests as preferred. If both pass or both fail, discard the pair. If you have fewer than 10 pairs, generate more.
3. Run a short DPO or preference update with `trl` (`pip install trl`) on those pairs, with a small learning rate and a short step budget. If the stack is too heavy for 6 GB, implement a simpler toy on your path 01 transformer: two fixed target strings for one prompt, raise the probability of the preferred string relative to the other for a few steps, and show both probabilities before and after.
4. Re-run the 20 problems. Report pass rate before and after, and one example where the preferred style was copied while the tests still failed.

## Verify

You can draw the pipeline "sample, rank, update, stay close to the reference" and point at which box your experiment filled. The before/after numbers are in the experiment log.

## You are done when

You can say what preference data measures in the code setting (tests) and what it would measure in a chat setting (a person's ranking), and you have one run that moved probabilities in the direction the pair specified.
