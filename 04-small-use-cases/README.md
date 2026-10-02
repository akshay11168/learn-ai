# 04 — Small use cases

These labs turn a technique into something that runs on your data and answers a new input next week. They are short on purpose. Depth comes from the split, the baseline, and the error analysis, which you already practice in path 06.

Start a use case only after its prerequisite lab. Build the model in the technique lab if that lab says so, and use this folder for the data, the script a person would run, and the write-up of how it fails.

| Use case | Prerequisite | What "finished" means |
|---|---|---|
| [01 Classify text](01-classify-text/README.md) | Path 01 lab 04, or path 02 lab 03 | A command prints a label for a new sentence, with a validation score behind it |
| [02 Classify your photos](02-classify-your-own-photos/README.md) | Path 02 lab 02 | A command prints a class for a new image path |
| [03 Ask questions over notes](03-ask-questions-over-notes/README.md) | Path 02 lab 01 | An answer that cites a passage, measurable against a handful of questions |
| [04 Recognize short sounds](04-recognize-short-sounds/README.md) | Path 01 lab 04 | A command classifies a short wav you just recorded |
| [05 Ship one small tool](05-ship-one-small-tool/README.md) | One of the four above | Someone else can run it from a README you wrote |

## Habits that apply to all five

- Data lives on `D:\learn-ai-data\<project>`.
- The test split is used once.
- The baseline is in the write-up, next to the model score.
- Twenty mistakes, tagged, are part of finishing.
- The interface is a script with arguments. A web UI is optional and comes after the script is correct.

## Done with this path when

At least three of the four data labs are finished, and lab 05 has packaged one of them so you can run it without rereading the training code. That packaged tool is the input to [production practice](../07-production-practice/README.md) and, later, the case studies in [consulting](../08-consulting/README.md).
