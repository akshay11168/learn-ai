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

## You are done when

The cold-terminal drill works, and each document names a limit you measured.
