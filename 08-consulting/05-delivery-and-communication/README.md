# 05 — Delivery and communication

Prerequisite: [03 Proposal and statement of work](../03-proposal-and-statement-of-work/README.md).

## Purpose

Practice the writing that makes a technical project feel reliable to someone who will not read your training log. The standard is specific, early, and dull. Surprises belong in the message the day you learn them, not in the final meeting.

## Explain before you code

1. A status that says "going well" contains no information. What three facts would you want if you were the client?
2. The golden-set score is below the gate a week before the end. What do you owe them that day?
3. Why is a chart of loss a poor slide for a buyer who asked for less manual tagging?

## Build

**`weekly-update.md`.** A template with: what finished this week (artifacts, not effort), the metric if you measured one, the decision you need from them, the risk, and what you will do next week. Fill it twice for your rehearsed project, as if week 1 and week 2 had happened. Use real numbers from your labs where you have them.

**`bad-news.md`.** A short note for this situation: the baseline is closer to the model than you hoped, or the labels are too inconsistent to hit the gate. The note says what you measured, what it means for the SOW, and the options (relabel a sample, narrow the workflow, stop, or change scope). It does not bury the number under a plan to try a larger model.

**`readout-outline.md`.** The final meeting, half an hour: the process you studied, the baseline, what you shipped, the golden-set result, three failure examples, what the human still does, and the decision you are asking them to make (use it on one queue, collect more labels, or do not proceed). Ten slides is too many. Headings in a document are enough.

**Language rules,** written at the top of the readout so you follow them:

- Say "the model" and the metric, not "the AI thinks."
- Show one mistake on purpose. A readout with no failure is not credible.
- Separate what you measured from what you expect next quarter.
- Name the fallback.

## Verify

Read the bad-news note aloud. If you wince and soften the number, put the number back. Then check that every metric in the readout appears in the experiment log or the golden-set output.

## You are done when

You have a filled weekly update, a bad-news note with a real limitation from your project, and a readout outline that ends in a decision.
