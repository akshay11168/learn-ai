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

## Study this step

**Concepts to master**

- A shippable tool has a frozen checkpoint, a single entry command, pinned dependencies, and an example with expected output.
- Training code is not part of the tool. Leaving it in the folder invites a later you to retrain casually and lose the measured result.
- Known failures belong in the README. A tool that hides them will be "mysteriously wrong" in a month.
- The smoke test is running the documented command on a clean terminal, not recognizing the command you remember.

**Study**

- The Twelve-Factor App, sections on dependencies and config only: https://12factor.net/dependencies and https://12factor.net/config — translate "declare dependencies" into your `requirements.txt`, and "config in the environment" into the checkpoint path and `HF_HOME`.
- Packaging a small CLI you already understand: Python's `argparse` tutorial, the first example: https://docs.python.org/3/library/argparse.html

**Practice**

- Rename the checkpoint and confirm the documented error tells you how to restore it.
- Follow your README in a new terminal with the venv deactivated, then activated, and note which step you had assumed.
- Delete one import from a scratch copy of the script and confirm the README's expected output no longer matches. Restore it. That is you feeling the smoke test work.

**Practice questions**

1. Why is `pip freeze` of the whole machine a worse `requirements.txt` than the libraries the script imports?
2. The README says "run the notebook cells in order." What did you fail to ship?
3. A known failure is "dark photos abstain." Why does omitting it from the README cost you later?
4. The checkpoint path is absolute on `D:`. What must the README say for a different machine?
5. You "improve" the model the night before a demo and skip the smoke test. Which lab's gate did you bypass?

## You are done when

You have done that smoke test, and the README lists the failures you already know about so you are not surprised by them later.
