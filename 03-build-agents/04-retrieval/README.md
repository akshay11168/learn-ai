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

## Study this step

**Concepts to master**

- Retrieval augments the prompt with passages. It does not by itself make the answer true. The generator can ignore the passages.
- The unit of retrieval is a passage with a source, not a whole file and not a vague "context."
- Keyword overlap is a baseline retriever. It fails on synonyms and wins on exact terms, which makes errors easy to see. An embedding retriever is a second system you adopt only after the baseline has a score.
- Citation is a checkable claim: the filename contains the supporting sentence. A citation the file does not support is a failure even if the prose sounds right.
- Abstention when nothing relevant was retrieved is a correct answer. Answering from parametric memory is a different system, and you are not building that one here.

**Study**

- The original RAG paper, abstract and Figure 1 only (Lewis et al.): https://arxiv.org/abs/2005.11401 — identify the retriever and the generator in their diagram and in your tool.
- Eugene Yan, "Patterns for Building LLM-based Systems & Products", the retrieval and grounding patterns: https://eugeneyan.com/writing/llm-patterns/ — pick the two patterns that match this lab and ignore the rest until path 07.
- Your own notes-QA lab write-up in path 04. If that score does not exist yet, the keyword baseline section of that lab is the reading, and this lab waits.

**Practice**

- For three questions, print the top 3 passages before generation. Mark whether the answer sentence is actually in one of them.
- Insert a note that says the validation split is "the data you train on" and ask what the validation split is for. Record whether the model repeats the lie.
- Ask something absent from the notes and check that the answer refuses rather than inventing a page number.

**Practice questions**

1. The right paragraph is ranked 4th and you only return 3 passages. Where did the failure happen, retrieval or generation?
2. The right paragraph is in the prompt and the answer contradicts it. Where did the failure happen?
3. Why is pasting the entire course into the prompt a worse plan than retrieval, besides cost?
4. A citation names `01-fit-a-line/README.md` and the sentence is not in that file. Is the task a pass?
5. Why must the transcript store the retrieved text separately from the final answer?
6. You fine-tune the generator in the same week you change the retriever and the score rises. What do you know about the cause?

## You are done when

A question about this course can be answered with a filename you can open and check.
