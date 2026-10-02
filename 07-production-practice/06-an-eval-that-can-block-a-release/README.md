# 06 — An eval that can block a release

Prerequisite: [02 Data you can defend](../02-data-you-can-defend/README.md) and a running service. Path 06's metric lab is assumed.

## Purpose

Build a frozen set of examples and a command that exits with a failure code when the system drops below a bar you set. This is the industry habit that replaces "I tried a few prompts and it seemed better." Consulting work uses the same gate before you tell a client a change is an improvement.

## Explain before you code

1. Why must the golden set stay out of training and out of prompt tinkering?
2. A change raises the average score and breaks the three examples the client cares about most. Did you improve the system?
3. What is a fair minimum bar on the first release: the baseline, the current model, or a number you wish were true?

## Build

1. Freeze 40 to 100 labeled examples in `golden.jsonl` (or a folder of files plus a label file). Include the boring typical case, the borderline cases from the labeling guide, the hostile inputs from the security lab, and the cases that must abstain or refuse. Store this on `D:` if it contains anything private, and keep a pointer in the lab notes. Commit the file only if it is safe to publish.
2. `eval_release.py` calls the same code path as `/predict` (import the function; do not copy it). It prints the metric from path 06, the baseline on this same set, and a slice score for the "must not fail" tag.
3. Pick a gate before you look at today's score if you can. A practical first gate: beat the baseline by a margin you write down, and zero failures on the refuse/abstain slice if that slice exists. If today's system misses the gate, the result is "not ready," not "lower the gate until it passes." You may lower a gate only by writing why the old one was measuring the wrong thing.
4. Change one thing (a threshold, a prompt line, or a checkpoint). Run the eval again. Record both rows in the experiment log. If the headline rose and a critical slice fell, the change does not ship.
5. The script's process exit code is non-zero when the gate fails, so you can run it before you replace a checkpoint.

## Verify

Two runs on an unchanged system produce the same score, or you document the sampling setting that makes them differ and you switch the release path to greedy or temperature 0. The golden file's date and count are in the eval output.

## You are done when

You can point at a command that would have stopped you from shipping a bad change, and you have one example of a change it rejected or accepted with the numbers attached.
