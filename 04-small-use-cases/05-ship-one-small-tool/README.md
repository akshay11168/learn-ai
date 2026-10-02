# 05 — Ship one small tool

Prerequisite: any one of labs 01–04 whose script already works.

## Purpose

Leave the lab folder and make one tool that you can run in a month. The work is packaging, a frozen decision about the checkpoint, and instructions that do not depend on your memory.

## Build

Create `D:\learn-ai-tools\<name>\` outside the git history of half-finished experiments, or create it here if you prefer it in the course repo. Include:

1. `README.md` with the task, the metric and the score, the known failures, and the exact command.
2. `requirements.txt` pinned to the versions you actually used (`pip freeze` is too wide; pin the few libraries the script imports).
3. The prediction script, not the training script. Training code stays in the lab.
4. A checkpoint path on D:, and a sentence about what to do if the file is missing (retrain from the lab, do not invent a new training recipe in the tool folder).
5. One example input and the output you expect, so a later you can smoke-test in thirty seconds.

Ask yourself the only release question that matters here: if you follow the README on a fresh terminal, do you get that example output?

## You are done when

You have done that smoke test, and the README lists the failures you already know about so you are not surprised by them later.
