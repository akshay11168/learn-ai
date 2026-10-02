# 02 — Data you can defend

Prerequisite: [01 The smallest system](../01-the-smallest-system/README.md).

## Purpose

Most production failures are data failures. This lab makes the dataset for your chosen use case something you can describe to a client: where it came from, who labeled it, what the classes mean, what must never be in it, and how a new batch will be split.

## Explain before you code

1. Two people label the same row differently. Whose label does the model learn, and how would you notice?
2. Why is a spreadsheet of customer messages on your laptop a different risk from CIFAR-10?
3. What does "the validation score used this week's definition of the label" mean for a number you quoted last month?

## Concepts

**A labeling guide** is a short document: each label, positive examples, negative examples, and the borderline case with the decision. If you cannot write the borderline case, the class is not ready to train on.

**Agreement.** Label 30 rows twice, on different days, or have a friend label them from the guide alone. Count disagreements. A guide that produces constant disagreement will cap the model's score. Fix the guide before you collect more data. You do not need a formal kappa statistic on the first pass. You need the disagreements in a list.

**Inventory.** For a client later, you will need to say: source, date range, number of rows, whether people are identifiable, license or permission, and where the files live. Write that inventory now for your own project. If the data is only yours, say so. If you would not be allowed to use a similar client file this way, mark the gap.

**Splits that survive contact with reality.** Group by person, document, session, or time. Do not shuffle rows that share an author or a burst of photos. New production data looks like a future time slice. A random row split estimates a kinder world.

**Version the set.** Copy the exact files you trained on to a folder named with the date, or record a hash of the file list. The golden set in lab 06 is a separate frozen file. Training data and the release test are not the same object.

**Personal data.** Names, phone numbers, addresses, government ids, health details, and private messages do not belong in a lab notebook, a prompt log, or a git repo. If a row contains them, redact before the row enters a training file. For this course, prefer data you can publish in a case study. That constraint is practice for client work.

## Build

In this folder, for the use case you chose:

1. `labeling-guide.md` with classes or output rules, including three borderline examples.
2. A second-pass label of 30 items and a disagreement list.
3. `inventory.md`: source, counts, split rule, permission, location on `D:`, and what you redacted.
4. A one-line change to the split if the disagreement review or the leakage rule says the old split was flattering. Retrain only if the decision lab's metric would move. Log it.

## You are done when

You can hand the labeling guide to another person and predict the rows they will argue about, because you already listed them.
