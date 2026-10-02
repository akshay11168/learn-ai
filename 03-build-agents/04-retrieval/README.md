# 04 — Retrieval

Prerequisite: [03 Tools](../03-tools/README.md) and [the notes use case](../../04-small-use-cases/03-ask-questions-over-notes/README.md). Build the retriever there first if you have not, and call it from here.

## Purpose

Give the agent a tool that searches your notes and returns small passages. The model answers from those passages. This is retrieval-augmented generation, done as a tool you can test, not as a product feature.

## Explain before you code

1. Why retrieve a short passage instead of pasting every note into the prompt?
2. What is a failure mode where the retrieved passage is on-topic and the answer is still wrong?
3. How will you notice if the model ignored the passage and answered from memory?

## Build

The tool `search_notes(query)` returns the top 3 passages and their source filenames, using the retriever from the use-case lab (keyword or embedding; use the one you measured). If that lab is not done, a minimal version for now is: split markdown files under `learn-ai` into paragraphs, score them by overlap with the query words, return the best three. Replace it with the measured retriever before you call this lab finished.

Rules you enforce in code and ask for in the prompt:

- The final answer cites the filename for any claim taken from notes.
- If `search_notes` returns nothing useful, the final answer says so.

Tasks:

1. A question whose answer is in one lab README (for example, what validation split is for). Check that the citation matches the file that contains the answer.
2. A question about something absent from the notes ("what is my birthday" unless you wrote it). The correct behavior is to say the notes do not contain it.
3. A question that needs two passages. Record whether one search was enough or the model searched again.

Log the retrieved passages in the transcript so you can see them separately from the answer.

## Verify

Task 1's citation is real. Task 2 does not invent a birthday from outside the notes. You can point at one transcript where the retrieved text and the answer disagree, or you can say you tried and the model stayed faithful. Both outcomes get written down.

## Stretch

Deliberately return a wrong passage as the tool result (a test double) and see whether the answer follows the tool or the model's prior belief. This is the same trust question as the buggy `add` tool, now with text.

## You are done when

A question about this course can be answered with a filename you can open and check.
