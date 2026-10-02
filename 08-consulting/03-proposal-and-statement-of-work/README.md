# 03 — Proposal and statement of work

Prerequisite: [02 Discovery](../02-discovery/README.md) and the decision record in [path 07 lab 01](../../07-production-practice/01-the-smallest-system/README.md).

## Purpose

Turn a problem statement into a document you could attach to an email. A proposal persuades. A statement of work defines the job so both sides can tell when it is finished. Write them as one short packet so you do not promise in the first half what the second half excludes.

This lab is practice. A real client's contract should be reviewed by a lawyer before you sign. Path 09 lists what that review is for.

## Explain before you code

1. Why does "build an AI solution" fail as a scope statement?
2. What is the difference between a pilot's success metric and a promise that the business result will appear?
3. Which assumptions, if false, should pause the work (no labeled data, no access, the process is different from the one described)?

## Build

`sow.md` for the problem statement you already wrote. Keep it under four pages. Include:

- **Context.** Their process, in the words from discovery. Two or three sentences.
- **Outcome of this engagement.** A pilot on one workflow. Name the workflow.
- **In scope.** The activities: data review, baseline, the system at the rung you expect, eval on a set you will freeze with them, a handover of the kind in path 07. Name artifacts (a report, a service or script, a runbook, a review meeting).
- **Out of scope.** Hosting for the whole company, mobile apps, unlabeled data collection beyond an agreed sample, other departments, guaranteed savings, legal or medical sign-off, ongoing on-call unless you add it as a separate line later.
- **Their duties.** A named person who can label or review, access to the sample data, a decision within a set number of days when you ask a question.
- **Metric.** What you will measure, on which set, against which baseline. State that the number is a measurement of the pilot, not a warranty of future accuracy.
- **Schedule.** A few weeks, with two checkpoints. Discovery correction at the start, eval review before the end.
- **Assumptions.** Volume, language, data permission, and the fallback when the model abstains.
- **Change rule.** New workflows are a new scope, not an implied extra.

Avoid adjectives that mean a score you do not have: accurate, intelligent, human-level, automated end to end. Use the metric instead.

## Verify

Read the SOW as the client. Underline every sentence that could be heard as a guarantee. Rewrite those sentences until a skeptical reader can see the limit. Then read it as yourself six weeks later: could you tell whether you had finished?

## You are done when

The scope lists artifacts, the metric names a baseline, and the exclusions include at least one thing you are technically able to do and still refuse to hide inside this pilot.
