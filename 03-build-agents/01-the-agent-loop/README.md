# 01 — The agent loop

Prerequisite: [path 02 lab 01](../../02-train-from-a-base-model/01-load-a-model-and-run-it/README.md).

## Purpose

Write the loop in the path README as executable code, with one fake tool, and watch a full transcript. No framework.

## Explain before you code

1. Where in the loop does your process have authority the model does not?
2. What should happen on step `max_steps`, even if the model wants to continue?
3. Why append the tool result as its own message, instead of hiding it inside a Python variable the model never sees?

## Build

`agent.py` in this folder.

- Messages are a list of dicts: `role`, `content`, and when needed `tool_name`.
- The system message tells the model it may either answer or call the single tool `add(a, b)` by emitting a line you define, for example `TOOL add {"a": 2, "b": 2}`.
- Your parser looks for that line. Anything else is treated as a final answer.
- `add` is implemented by you and returns a string. Include a case that returns an error string when the arguments are missing.
- `max_steps` is 4.
- After each step, print the transcript.

Tasks to run and save under `transcripts/`:

1. "What is 17 + 25?" The useful behavior is one tool call and then the number.
2. "What is the capital of France?" The useful behavior is an answer with no tool call.
3. A prompt that mentions addition but also asks for a capital. Record which tool choice the model made.

If you have no GPU session handy, a stub model that returns a scripted reply is allowed for the parser tests only. The three tasks above must use the real model once, because the point is to see a real transcript, including a format slip if one happens.

## Verify

You can point at a transcript and name the step where control returned from the model to your function. Task 1 ends with the correct sum or with a tagged failure you understand. The loop always terminates.

## Stretch

Insert a bug in `add` so it returns `a + b + 1`. See whether the model's final answer trusts the tool. Write down what that implies for tools that hit a database or a test runner.

## You are done when

You can reimplement the loop from the pseudocode without looking at `agent.py`, and the termination condition is obvious in the code.
