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

## Study this step

**Concepts to master**

- Instruction tuning teaches a format by imitating prompt-response pairs. Preference tuning teaches a ranking between two finished responses.
- A reward model is a learned scalar judge. RLHF optimizes the policy against that judge while a penalty keeps the policy near the reference model.
- DPO writes a classification loss on the pair so the policy's relative probability of the preferred answer rises, with the frozen reference in the formula. You should be able to say what each of the two models is doing even if you have not derived the loss.
- On code, tests are a preference oracle: pass is preferred to fail. Style is not, unless you decided it was and wrote that down.
- A small model can copy the length or the tone of a preferred answer without becoming more correct. Your before/after pass rate is what catches that.

**Study**

- Lilian Weng, "Learning from human feedback", the RLHF overview: https://lilianweng.github.io/posts/2022-02-20-rlhf/ — read the pipeline diagram until you can redraw it from memory: SFT, reward model, policy optimization.
- The DPO paper, abstract and Figure 1: https://arxiv.org/abs/2305.18290 — write one sentence on what DPO removes (the explicit reward model and the RL loop) and one sentence on what it keeps (a reference policy).
- Hugging Face course chapter or blog on RLHF if the course currently includes it, plus the TRL DPO trainer doc you would call: https://huggingface.co/docs/trl/dpo_trainer — read the arguments `beta`, `max_length`, and the expected dataset columns. You are mapping concepts to fields, not memorizing a framework.

**Practice**

- On one prompt, compute the length in tokens of the preferred and rejected completions. If the preferred ones are systematically longer, write that confound down before you train.
- After the update, print the log probability of one preferred string and one rejected string if you can do so cheaply, or rerun tests if you cannot. The tests are the result that matters.
- One pair where both solutions fail tests must be excluded. Count exclusions.

**Practice questions**

1. Why is "always prefer the longer answer" a reward hack a human labeler can teach by accident?
2. In DPO, what is the reference model a brake on?
3. Tests mark A better than B, and after training the model emits answers that look like A and still fail. Which objective did you increase, and which did you not measure until eval?
4. You use the same 20 problems to build preference pairs and to report the final pass rate. What is wrong with the report?
5. Instruction tuning data is `(problem, solution)`. Preference data is `(problem, better, worse)`. Which one can punish a fluent wrong answer that SFT would have imitated?
6. Beta in the DPO loss is described as controlling how far the policy may move from the reference. If your tiny run's outputs become gibberish, which way do you move beta, and what do you reread before you touch it?

## You are done when

You can say what preference data measures in the code setting (tests) and what it would measure in a chat setting (a person's ranking), and you have one run that moved probabilities in the direction the pair specified.
