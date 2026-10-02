# 04 — Cost, latency, and failure

Prerequisite: [03 Serve it](../03-serve-it/README.md).

## Purpose

A buyer asks what a day of use costs and how slow it is on a bad input. You will measure your service and write a one-page budget. You will also define what the service does when it is slow, unsure, or wrong.

## Explain before you code

1. A local GPU you already own has a different cost shape from a paid API billed per token. What do you still need to account for on the local machine (electricity, your time, the laptop being busy)?
2. Why is the slowest 5% of requests more important than the average, for a person waiting on one document?
3. If the model abstains, who does the work, and is that an acceptable product?

## What to measure

Run 50 requests through `/predict` that look like real use: typical inputs, one very long input, one empty input, and a few from your error pile.

Record:

- Median time and the time that 95% of requests finish under.
- Whether the GPU or the CPU did the work (`nvidia-smi` during the run).
- Peak memory if the service loads a model.
- For a generative model: input tokens, output tokens, and a rough cost if you also time the same prompt on a paid API you choose not to call. You may estimate API cost from the provider's published price and your token counts. Label the estimate. Do not create an account just to finish the lab.
- Failure count: timeouts, exceptions, empty answers, abstentions.

Write the policy next to the numbers:

- Timeout at a specific number of seconds. What the caller receives.
- Abstain when confidence is below the threshold from the use case, if you have one.
- If the service is down, the human path is the documented fallback (do the task by hand, or use the keyword baseline). A system without a fallback is only safe for hobbies.

## Capacity on this laptop

One RTX 3060 serving one model interactively is a demo and a single-user tool. It is not a multi-tenant production GPU. Say that in the budget note in a sentence a client would understand: this hardware supports development and a light internal tool; a shared office workload needs a separate machine or a hosted model, sized with these latency numbers as the starting point.

## Build

`budget.md` with the table of measurements, the timeout, the abstain rule, the fallback, and a sentence on what would dominate cost if volume went up by 100× (your time, GPU hours, or API tokens). Update the service if the timeout exists only in the notes.

## Study this step

**Concepts to master**

- Median latency hides the tail. The slow requests are the ones a human remembers.
- Timeout, abstain, and fallback are product decisions with numbers attached.
- Local GPU cost is opportunity and power, not a token invoice. API cost is tokens times a published price. Label which one you measured and which you estimated.
- A 100× volume thought experiment tells you whether the design dies from your time, from GPU hours, or from API tokens.
- This laptop is a single-user development machine. Multi-tenant serving is a different computer.

**Study**

- Google SRE book, the chapter on service level objectives, the part that distinguishes a target from a wish: https://sre.google/sre-book/service-level-objectives/ — read the vocabulary of SLI/SLO. You will write one latency SLI, not a Google-scale system.
- The pricing page of one model API, read on the day you estimate, and dated in `budget.md`. Do not hard-code a price from memory into the course.
- Your `nvidia-smi` memory and utilization during the 50 requests.

**Practice**

- Measure 50 requests. Compute median and the 95th percentile by hand from a sorted list, then with code.
- Add a timeout that fires on the long input. Save the body the client receives.
- Write the 100× sentence: what dominates, and what you would move off the laptop.

**Practice questions**

1. Mean latency is 200 ms and the 95th percentile is 4 s. Which number do you tell a user who submits one document, and why?
2. The model abstains on 30% of validation items. Is that a failure or a cost you shift onto a human? What must you know to decide?
3. Why is "the GPU was free because I own it" incomplete accounting?
4. A timeout that returns a stack trace is which lab's bug, and what should it return instead?
5. At 100× volume the laptop's fan is not the plan. Name the two real options the budget note should mention.

## You are done when

You can quote one latency number, one failure behavior, and one sentence on cost at 100× volume, from measurements you ran.
