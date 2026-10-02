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

## Study this step

**Concepts to master**

- A label definition includes the borderline case. If two careful readers disagree, the ceiling on accuracy is their agreement, not 100%.
- Bag-of-words plus a linear layer is a real model. It is the baseline a transformer must beat on a small text task.
- Macro F1 averages per-class F1 so a rare class is not invisible. Accuracy hides that class.
- The split unit is the document or the author, not the sentence, when sentences from one document share a label for free.
- Error tags decide the next data collection action. A tag of "ambiguous label" means fix the guide. A tag of "rare wording" means collect those rows. Another epoch is not a tag.

**Study**

- Scikit-learn, "Working with text data", the bag-of-words section: https://scikit-learn.org/stable/tutorial/text_analytics/working_with_text_data.html — read `CountVectorizer` and the train/test discipline. You may still train the linear layer in PyTorch.
- Google's Rules of ML, the rules about features and about not ignoring non-ML baselines: https://developers.google.com/machine-learning/guides/rules-of-ml
- Path 06 lab 01 if you have not finished it. The metric definitions there are assumed.

**Practice**

- Write the keyword baseline in ten lines and score it before you train anything.
- Hand-label 20 items a second time a day later. Count flips.
- Run `classify.py` on five sentences that use none of your training vocabulary and record whether the model abstains or guesses at high confidence. If it cannot abstain, note that as a missing feature.

**Practice questions**

1. 90 of 100 rows are `todo`. A model predicts `todo` always. Accuracy, and the F1 of the rare class?
2. Why can a random sentence split of one diary inflate accuracy?
3. Your keyword rule scores 0.70 macro F1 and the neural net scores 0.72. Is the net the product, or is the rule still in the running? What else would you measure (failure types, speed)?
4. The guide does not mention questions that are both a todo and a reference. What will the model learn on those rows?
5. Name the next 20 rows you would label, based on the error tags, not on convenience.

## You are done when

The script works in a new terminal with the venv active, the validation score beats both baselines, and the error tags tell you the next data you would collect.
