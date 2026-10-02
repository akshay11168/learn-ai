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

## Study this step

**Concepts to master**

- The agent is a control loop in your process. The model is one function call inside it. Authority to act sits in the branches you wrote.
- State is the message list. If a fact is not in that list or in a tool result you append, the model cannot use it on the next turn except by guessing.
- Termination conditions are part of the design: final answer, step limit, and later parse failure. A loop without a limit is not an agent. It is a hang.
- A tool result is data, including error strings. The model does not get a hidden exception. It gets text you chose to show it.
- Transcripts are the debugging trace. A summary of "it worked" deletes the only evidence.

**Study**

- Lilian Weng, "LLM Powered Autonomous Agents", the section that defines the loop (planning, memory, tools), the opening only: https://lilianweng.github.io/posts/2023-06-23-agent/ — redraw her diagram as the six-line loop in this path's README. Note what you are not building yet.
- Anthropic, "Building effective agents": https://www.anthropic.com/engineering/building-effective-agents — read the distinction between a workflow (you wrote the branches) and an agent (the model chooses the branch). Your lab is the smallest agent. Be able to say which one a given demo is.
- Simon Willison, "Agent" is a vague word, the post where he argues for a definition: search his blog for the 2025 piece titled along the lines of "I still don't like agents" or read https://simonwillison.net/2025/May/22/tools-in-a-loop/ if that is the current canonical post. The idea to take: an agent is tools in a loop, and everything else is marketing. If the URL has moved, find it from his site search rather than a repost.

**Practice**

- Desk-check the loop on paper for task 1: write every message after each step before you run the model.
- Force `max_steps = 1` and confirm the adder task stops with an explicit stop reason if the model only called the tool.
- Change `add` to return `a + b + 1` and save the transcript where the model trusts the lie.

**Practice questions**

1. The model says "I called add and the result is 42" without a tool message in the transcript. Did it call the tool?
2. Why must the tool result be appended as its own message?
3. `max_steps` is 4 and the model emits a tool call every time. What does the user receive, and which line of code decided that?
4. A framework hides the message list. What bug becomes harder to see?
5. Task 2 should not call `add`. If it does, is that a loop bug or a model bug, and how do you know from the transcript?
6. Write the loop's state in one sentence after step 0 and after a successful tool call.

## You are done when

You can reimplement the loop from the pseudocode without looking at `agent.py`, and the termination condition is obvious in the code.
