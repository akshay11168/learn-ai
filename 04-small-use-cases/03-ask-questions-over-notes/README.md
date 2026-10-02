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

## Study this step

**Concepts to master**

- A question set written before you tune the retriever is an eval. A question set written after you saw the failures and then "fixed" until they passed is a memory test of those fixes.
- Separate retrieval failure (the passage was not in the top 3) from generation failure (the passage was present and the answer still wrong).
- Keyword scoring is explainable: you can see the overlapping words. Embedding scoring needs a nearest-neighbor inspection or it is a black box you cannot debug on 15 questions.
- The prompt constraint "answer only from these passages" is a request. The citation check is the enforcement.

**Study**

- Stanford CS224n notes or slides on lexical retrieval versus dense retrieval, any one lecture section you can find in the public CS224n materials on word vectors and similarity: https://web.stanford.edu/class/cs224n/ — read the cosine-similarity definition. Your optional embedding retriever is cosine similarity over passage vectors.
- Lewis et al., RAG, Figure 1 again: https://arxiv.org/abs/2005.11401
- A short note on BM25 from the Wikipedia page or from the Elasticsearch guide's BM25 section: https://www.elastic.co/blog/practical-bm25-part-1 — you implement overlap, not BM25. You should know overlap is the crude cousin of BM25 so you do not think keyword search is a toy only hobbyists use.

**Practice**

- Build the 20 questions first, with the file and a supporting quote written down, before changing the ranker.
- For each of the 15 answerable questions, record rank of the correct passage. A histogram of ranks tells you whether top-3 is the right cutoff.
- Change one stopword list and rescore. If you cannot explain the delta, revert.

**Practice questions**

1. Correct passage is rank 1 and the answer contradicts it. Which component do you edit?
2. Correct passage is rank 6. Which component do you edit, and why is a longer prompt to "think harder" the wrong edit?
3. You add the 15 questions into the index as notes, and the score jumps. What did you leak?
4. Why can a high embedding similarity still retrieve the wrong section of a lab that shares many words with the question?
5. Define a pass for an unanswerable question in one sentence a script could check.

## You are done when

You have a score out of 20, you know whether failures were retrieval or generation, and path 03 can import `search(query)`.
