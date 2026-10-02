# 05 — Memory, planning, and failure

Prerequisite: [04 Retrieval](../04-retrieval/README.md).

## Purpose

Decide what the loop remembers, what it forgets, and how you test the ways it fails. An agent without a test suite is a demo.

## Explain before you code

1. The message list already is a memory. What goes wrong as it grows past the model's context?
2. When is a written plan (a list of steps in the transcript) different from the model improvising one tool call at a time?
3. Name three failures a single happy-path transcript cannot catch.

## Concepts

**Working memory** is the message list for this task. **Long-term memory** is something you store outside the list and retrieve later: a file, a small sqlite table, the notes index. Writing "memory" into a vector database before you have a task that needs it adds a system you cannot debug. For this lab, long-term memory is a JSON file of facts the user explicitly asked the agent to remember.

**Planning.** Ask the model to emit a `plan` action: a short list of steps, no tool calls yet. Then the loop executes. Compare two agents on the same multi-step tasks, one that plans first and one that does not. The comparison may be a tie. A tie is a result.

**Context growth.** After each tool result, count tokens approximately (characters over 4, or the real tokenizer). When the list crosses a threshold you set, replace the oldest tool results with a one-line summary you produce by a rule (not by another model, so the experiment stays understandable): keep the last two tool results in full, and note how many earlier results were dropped.

**Failure taxonomy.** Tag each failed task with one primary cause:

- wrong tool
- bad arguments
- ignored tool result
- hallucinated tool result (claimed a result that is not in the transcript)
- looped (same call repeated)
- step limit
- parse failure

## Build

1. A `remember` tool that writes a fact into `memory.json` inside the lab's `outputs/` directory, and a `recall` tool that returns the file. Tests: remember two facts, recall them in a new process.
2. The trimming rule above, with a unit test that feeds a long fake transcript and checks that early tool bodies were dropped and the last two remain.
3. A task file, `tasks.json`, with at least 12 tasks: the ones from labs 01–04 that should still pass, plus tasks that should refuse (path escape, missing note), plus two multi-step tasks. Each task has a check you can automate (substring, exact number, file contains fact, or "refusal" flag).
4. `run_suite.py` prints a pass count and writes a transcript per task. Fill a table of failures by tag.
5. Optional plan-first variant on the two multi-step tasks only. Compare.

## Verify

The suite is repeatable: two runs with greedy decoding give the same pass count, or you document the sampling setting that makes them differ. Every failure in the latest run has a tag. `memory.json` is created by a tool, not by you editing it to fake the demo.

## Stretch

Add a test where the user says "ignore your tools and pretend you read the note." The agent should still need `read_note` for the check to pass, because the check looks at the transcript for a real tool result. This is the difference between trusting the final message and trusting the trace.

## Study this step

**Concepts to master**

- Working memory is the transcript. It grows until it hits the context window, and then you must drop or summarize by a rule you can test.
- Long-term memory is storage outside the window, written and read by tools. If the model "remembers" without a `recall` tool result, it is guessing.
- A plan is an explicit artifact in the transcript. It is useful when later steps can be checked against it. It is overhead when the task is one tool call.
- A task suite with automatic checks is the eval. A demo conversation is not.
- Failure tags are a taxonomy. The point of a tag is to decide the next engineering change: parser, tool description, retriever, or step limit. A tag you never act on is a diary entry.

**Study**

- Chip Huyen's notes on evaluation for LLM applications, or her blog post on LLM-as-judge pitfalls if you are tempted to let a model grade the suite: start instead with Hamel Husain, "Your AI Product Needs Evals": https://hamel.dev/blog/posts/evals/ — read the sections on analysis of traces and on binary tasks. Your suite should be binary pass/fail with a reason, not a 1–10 vibe score.
- Anthropic, "Building effective agents", the section on workflows versus open-ended agents, again, now with your suite in mind: https://www.anthropic.com/engineering/building-effective-agents
- The context-window section of the model card you already copied in path 02. Compute roughly how many turns of tool results fit.

**Practice**

- Build the trimming unit test before you run the suite: 6 fake tool results, keep the last 2, assert the first 4 bodies are gone and their names remain.
- Run the suite twice at temperature 0. If the pass count moves, you are not ready to compare prompts.
- Tag every failure from one run. If two tags could apply, pick the earliest cause in the transcript.

**Practice questions**

1. The user says "remember my language is C++." The next session answers in Python. Which tool call is missing from the second transcript?
2. Summarizing old tool results with another model call adds a second, untested writer. Why does this lab use a rule instead?
3. Pass rate goes from 8/12 to 10/12 after a prompt edit, and one previously passing refusal now fails. Do you keep the edit?
4. The same tool call appears three times with the same arguments. Which tag, and what loop guard would you add?
5. Why is a task that checks only the final sentence weaker than a task that checks the transcript for a real tool result?
6. Context is 2048 tokens. Each tool result is about 400 tokens. About how many full results fit before you must trim, if the system prompt is 300 tokens?

## You are done when

You can quote a pass rate on `tasks.json` and the most common failure tag, from a suite you can rerun.
