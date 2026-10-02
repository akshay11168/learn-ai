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

## Study this step

**Concepts to master**

- Scope is a list of artifacts and activities. "Improve the process with AI" is not scope.
- The metric names the set, the baseline, and the fact that it is not a warranty.
- Exclusions are where fixed-price work survives. Anything omitted will be assumed included.
- Assumptions are the conditions that pause the project: access, labels, a named reviewer, data permission.
- Change control means a new workflow is a new estimate, written down before you build it.

**Study**

- A university procurement office's public "how to write a statement of work" guide. Search for "statement of work" on a .edu site and read one short guide end to end, for the shape only: objectives, deliverables, timeline, assumptions, and what is outside the work. It will be written for buyers. Read it as the document your client wishes you had given them. Your lawyer's template overrides it when you take a real client.
- Your path 07 decision record. The SOW's method must match the rung you can defend.
- McKenzie on writing concrete proposals, in "Don't Call Yourself A Programmer", the bits about promises you can keep.

**Practice**

- Underline every adjective in the draft and replace it with a noun or a number, or delete it.
- Circle one capability you have (a chatbot, a second department, hosting) and put it in exclusions on purpose.
- Read the SOW as the client and list every sentence that sounds like guaranteed savings. Rewrite them.

**Practice questions**

1. Why does a fixed price without exclusions transfer unbounded work to you?
2. "The system will be accurate" belongs in which section, if any, and what replaces it?
3. The client adds a second document type in week two. What does the change rule require you to do before you touch it?
4. Their duty is "support as needed." Why is that not a duty, and what do you write instead?
5. Which artifact from path 07 must the SOW promise, and which number must it refuse to invent?

## You are done when

The scope lists artifacts, the metric names a baseline, and the exclusions include at least one thing you are technically able to do and still refuse to hide inside this pilot.
