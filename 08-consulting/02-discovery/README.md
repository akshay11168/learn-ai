# 02 — Discovery

Prerequisite: [01 Wedge and portfolio](../01-wedge-and-portfolio/README.md).

## Purpose

Learn the client's workflow before you mention a model. Discovery is a set of questions, a summary they can correct, and a decision that this is a fit, a later fit, or a no.

## Explain before you code

1. Why is "walk me through the last ten times you did this task" a better question than "would AI help?"
2. What do you need to know about the cost of a wrong answer before you propose automation?
3. When is the honest outcome of a discovery a recommendation to keep the current process?

## The conversation

Write `discovery-questions.md` and then use it. Sections:

- **The work.** Who does it, how often, how long a single item takes, what the input looks like, what "done" looks like, where the output goes next.
- **The pain.** Which step is slow, which step is often wrong, what a mistake costs in time or money or risk. Ask for a recent example, not a mood.
- **The data.** Where the examples live, who may use them, whether people are identifiable, how labels are decided, how often the rules change.
- **The constraint.** Deadline, systems you must not touch, languages, review by a human, what must stay on their machines.
- **The baseline.** What they do today, and any number they already track.

Practice on yourself or a friend using one of your use cases as the "client." Take notes. Then write `problem-statement.md` in their vocabulary:

- Current step-by-step process.
- Volume and time.
- The mistake that matters.
- The data you would need, and whether it exists.
- Fit: yes for a pilot, not now, or no, with one paragraph why.

Send the statement back to the person you interviewed if there was one, and correct it where they disagree. If you interviewed yourself, reread it a day later and mark the sentences you cannot support with an example.

## A no is a result

Add a short list of automatic nos for your wedge. Examples you may adopt or rewrite: no data and no willingness to label any; a regulated decision (credit, hiring, medical, legal) you are not qualified to own; a request to copy a person or to deceive customers; a request for a guarantee of accuracy; a production system that must run unattended at a scale your handover cannot support. Declining these is part of being hireable.

## You are done when

You have one problem statement written from a real conversation or a strict self-interview, including a fit decision, and a list of nos you are willing to say out loud.
