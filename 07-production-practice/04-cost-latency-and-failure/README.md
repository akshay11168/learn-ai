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

## You are done when

You can quote one latency number, one failure behavior, and one sentence on cost at 100× volume, from measurements you ran.
