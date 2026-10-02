# 03 — Ask questions over your notes

Prerequisite: [path 02 lab 01](../../02-train-from-a-base-model/01-load-a-model-and-run-it/README.md). The agent path reuses this retriever.

## Purpose

Build a question-answerer over a folder of markdown, grounded in retrieved passages. Start with search that is easy to debug. Add embeddings only after the keyword version has a score.

## Explain before you code

1. What is the difference between retrieving the right paragraph and generating the right answer?
2. Why should the generator see the passage in the prompt, and the user see the filename?
3. How can you score this with 15 hand-written questions without fooling yourself?

## Build

Index the `learn-ai` markdown files, or a notes folder of your own on D:.

1. Split into passages of about a paragraph, keeping the source path and a character offset.
2. Keyword retriever: lowercase, split on words, score a passage by overlap with the query, ignore a small set of stopwords you write down. Return the top 3.
3. Write 15 questions whose answers you can point to in a file, plus 5 questions the notes do not answer.
4. A script that retrieves, builds a prompt that says "answer only from these passages and cite the path," runs the small model from path 02 lab 01, and prints the answer and the paths.
5. Score each of the 20 by hand: right citation, wrong citation, answered when it should have abstained, abstained correctly. This is tedious and it is the lab.
6. Optional: an embedding retriever. Embed passages with a small local embedding model that fits on the GPU, or even with a bag-of-words vector if downloading another model is a distraction. Compare top-3 overlap with the keyword retriever on the same 15 questions. Keep the one that gets more right citations. Write the count.

Do not fine-tune the generator in this lab. The thing you are measuring is retrieval plus prompting. Fine-tuning here would mix two changes.

## You are done when

You have a score out of 20, you know whether failures were retrieval or generation, and path 03 can import `search(query)`.
