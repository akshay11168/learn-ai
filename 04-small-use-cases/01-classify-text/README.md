# 01 — Classify text

Prerequisite: the training loop in [path 01 lab 04](../../01-train-from-scratch/04-autograd-and-the-training-loop/README.md). You may redo this after [path 02 lab 03](../../02-train-from-a-base-model/03-fine-tune-a-small-text-model/README.md) with a pretrained encoder and compare.

## Purpose

Ship a classifier for a labeling task you actually have. The labels should be ones you can define in a sentence, and ones a stranger would mostly agree on.

## Task ideas

Pick one and stick to it.

- Personal notes tagged `todo`, `reference`, or `log`.
- Commit messages or one-line bug titles tagged `bug` versus `feature`, using text you write if you do not have a corpus.
- Questions tagged with the path they belong to (`foundations`, `from-scratch`, `agents`, …), using sentences from this course as a start and your own paraphrases as the real test.

A few hundred labeled rows is a serious small project. Fifty is enough to learn the pipeline and too few to trust the score. Write the count in the notes.

## Build

1. A labeling guide in `notes.md`: what each class means, and two borderline examples with the decision you made.
2. Collect the texts on D:. Split by document, not by sentence from the same document, if sentences in one document leak the label.
3. A baseline: most-common-class, and a keyword rule you write by hand (if the text contains "fix" then bug). Record both on validation.
4. A model. First version: bag of word counts, a linear layer, cross-entropy, the training loop you wrote. Second version, optional: the pretrained encoder from path 02.
5. Macro F1 if the classes are uneven, accuracy if they are balanced. Report both if you are unsure.
6. `python classify.py "some new sentence"` prints the label and the probabilities.
7. Twenty validation errors, tagged: ambiguous label, missing vocabulary, too short, or model systematically confuses two classes.

## You are done when

The script works in a new terminal with the venv active, the validation score beats both baselines, and the error tags tell you the next data you would collect.
