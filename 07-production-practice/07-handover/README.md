# 07 — Handover

Prerequisite: labs 01 through 06 of this path.

## Purpose

Write the packet a colleague, a future you, or a client operator needs on the day you are not in the room. This is the technical deliverable. Path 08 will wrap a commercial document around it. The packet has to match the system you actually run.

## Explain before you code

1. A model card that quotes a test score you tuned on is a false document. Which number are you allowed to print?
2. What are the three failures an operator should expect in the first week?
3. When the model is unsure, what does the human do, and where do those corrections go?

## The packet

Create these files in this folder, concrete to your use case:

**`model-card.md`.** What the system is for, what it is not for, inputs, outputs, model or rules version, training data inventory pointer, metric, baseline, golden-set result, known failure tags, and the date. "Not for" matters as much as the description. A notes answerer is not a source of medical or legal decisions. Write the limit that matches your project.

**`runbook.md`.** How to start the service, how to call it, how to tell that it is unhealthy, where the checkpoint lives, how to roll back to the previous checkpoint, and who to ask when a request fails. Include the timeout and the abstain behavior.

**`human-review.md`.** Which outputs a person must check before they count (low confidence, a named class, any agent action that writes or sends). Where corrections are stored so they can become future labeled rows. A loop that throws away corrections will not improve.

**`risks.md`.** Ten lines: the worst mistake, who it harms, how you would notice, what you do that week. This is a small version of a risk review. Formal frameworks such as the NIST AI Risk Management Framework exist for larger programs. You are not certifying anything. You are practicing the questions those frameworks ask, at the size of one tool.

## Build

Fill the four documents from the earlier labs, not from imagination. If a number is missing, go back and measure it. Then do a handover drill: follow `runbook.md` on a cold terminal, from process start to one prediction, without using any other note. Fix the runbook where you got stuck.

## Study this step

**Concepts to master**

- A model card says what the system is for, what it is not for, and the measurement. Marketing adjectives do not belong.
- A runbook is executable prose. If you cannot follow it on a cold terminal, it is a memoir.
- Human review is a queue with a rule for what enters it and a place corrections are stored.
- A risk note names the harm, the detection, and the response. It is short on purpose.
- NIST's AI Risk Management Framework is a map of questions (govern, map, measure, manage). You are answering a tiny subset, not claiming compliance. The public document: https://www.nist.gov/itl/ai-risk-management-framework

**Study**

- Hugging Face model card guide: https://huggingface.co/docs/hub/model-cards — fill their headings that apply, and skip the ones that do not, explicitly.
- NIST AI RMF playbook, one function only, "Measure": skim the headings so you see what a full program asks. Write the three questions that apply to your tool.
- Your own golden-set output. Every number in the model card must match it.

**Practice**

- Cold-terminal drill. Fix the runbook at each stumble.
- Have someone else start the service from the runbook if you can. Where they stop, the runbook is wrong.
- Delete any sentence in the model card that you cannot point to a measurement or a limitation for.

**Practice questions**

1. The card says "highly accurate" and the golden set says 0.81 against a 0.78 baseline. What do you replace the adjective with?
2. Why does "not for medical decisions" belong in the card even if you think it is obvious?
3. Corrections are stored nowhere. What happens to the system in three months?
4. Rollback is "retrain if needed." Why is that not a rollback?
5. Which NIST-style question did you actually answer in `risks.md`, in one sentence?

## You are done when

The cold-terminal drill works, and each document names a limit you measured.
