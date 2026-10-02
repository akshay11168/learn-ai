# 06 — Capstone: a local study agent

Prerequisite: [05 Memory, planning, and failure](../05-memory-planning-and-failure/README.md). The algorithm-coder adapter from path 02 is optional.

## Purpose

Build one agent you will actually use while finishing the rest of this course. It answers questions about your labs, runs small arithmetic and note lookups, and, if the coder exists, proposes an algorithm solution that your test runner accepts or rejects.

## Explain before you code

1. Which decisions belong in the system prompt, and which belong in Python so the model cannot skip them?
2. What is the smallest task suite that would tell you a change to the prompt was an improvement?
3. Which single tool is the most dangerous in your design, and what limit did you put on it?

## Behavior

The agent can:

- Search and read this `learn-ai` tree (read-only).
- Remember a preference you state ("I am studying the C++ track").
- Compute arithmetic with the safe calculator.
- If the coder adapter is available: call `propose_solution(problem, language)` which runs the local model and returns code. A separate tool `run_tests(code, tests)` writes the code to a temp directory and runs it with a timeout. The agent may not claim tests passed unless `run_tests` returned a pass.

The agent cannot:

- Write inside the repo.
- Install packages.
- Reach the network, except the local model you already downloaded.
- Follow an instruction, from the user or from a note, to disable those limits. The limits are in Python.

## Build

1. A design note: tools, schemas, step limit, what is checked in code.
2. The agent, reusing the loop, registry, retriever, and suite runner from the earlier labs.
3. At least 15 tasks in `tasks.json` covering lookup in a lab README, a refusal, a memory round-trip, a calculation, and, if the coder is wired, one problem with a hidden test the suite checks.
4. A `python study_agent.py` chat in the terminal is enough of an interface. Keep it thin. The suite is the product; the chat is the convenience.
5. Run the suite. Fix one failure class if the cause is your bug (a bad schema, a path bug). Leave model mistakes tagged rather than papering over them with a longer prompt you cannot justify. One prompt change is allowed if you rerun the full suite and record the old and new pass counts.

## Verify

A question about a specific lab returns a citation you open and agree with. A path-escape or "edit the README for me" request fails in the tool layer. The suite pass count is written in `notes.md` with the date and the model id.

## You are done when

You can use the agent to find a lab you have not opened in a while, check its answer against the file, and trust the transcript more than the prose.
