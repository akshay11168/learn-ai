# 04 — How to follow the field

## Purpose

Build a small, repeatable way to learn what changed, without turning the course into a news feed. Do this once now, on paper, and again at the end of any month in which you are still studying.

## What to read, and what to ignore while you are a beginner

Read, in this order, when something claims to be new:

1. The model card or the system card: size, data, license, hardware they used, the metric they lead with.
2. The dataset card: what the rows are, what the split is, what you are not allowed to do.
3. One section of the paper or the technical report: the method figure and the comparison table. Skip the related-work tour until the method is clear.
4. A second source that measures the same claim, if one exists. A single leaderboard number is a hint, not a result.

Ignore, until the above is done: launch threads, screenshots of chats, and parameter-count boasts with no eval protocol.

## The five-sentence note

Each time you do this, add a dated section to `notes.md` in this folder:

1. What the thing is (model, dataset, or training method), with a link and a date.
2. Where it sits on the language, image, or sound map.
3. What it would take to run it on the RTX 3060, in VRAM terms you compute or quote from the card.
4. What evidence the authors show, and what a skeptic would still ask (split, baseline, data leakage, license).
5. Whether this course's map needs a change. Most months the answer is no. Write no when it is no.

## First assignment

Write that note for one of these, whichever you can open and read carefully:

- The model card of the small code model you chose in path 02.
- The TACO dataset card.
- One recent model you have heard named and have not yet placed. If the card does not state hardware or data, sentence 4 is about that gap.

## A longer practice, later

Path 06 lab 04 asks you to reproduce one idea from a paper at a small scale. That is the advanced form of this lab. This lab is the monthly version: understand the claim well enough to decide whether it belongs in your work.

## Study this step

**Concepts to master**

- A claim has a subject (model, data, or method), a comparison, and a limitation. A launch post often has only a name.
- The model card and the dataset card are the primary sources. Commentary is secondary.
- VRAM arithmetic from path 01's capstone is how you decide whether "new" matters on your machine.
- Most months the map does not change. Writing "no change" is a successful review.
- You cannot follow everything. You follow the wedge you will consult on, plus one adjacent area.

**Study**

- Hugging Face model-card guidebook, the sections a card should contain: https://huggingface.co/docs/hub/model-cards — use it as the checklist while you read a real card.
- "Troubleshooting public ML reports" habits from Karpathy's recipe, the evaluation warnings: https://karpathy.github.io/2019/04/25/recipe/
- One dataset card end to end, TACO or the set you actually used. Cards are the genre. A blog will not substitute.

**Practice**

- Fill the five-sentence note from the lab instructions for a real card, with a link and a date.
- Compute whether the thing fits in 6 GB for inference and for training. Show the arithmetic.
- Find one sentence in a secondary blog that is stronger than the card. Strike it from your note.

**Practice questions**

1. A post says a model "beats GPT-4." What four facts do you need before that sentence enters your notes?
2. The card omits training data and eval contamination. What do you write in sentence 4?
3. Why is a month with "no map change" not a failed study session?
4. Which source wins when a Twitter thread and a model card disagree, and why?
5. Name the one area you will not track this year, on purpose.

## You are done when

One five-sentence note exists, and you know the day of the month you will write the next one.
