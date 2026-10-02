# 03 — Build agents

An agent is a loop. The model reads the situation and proposes the next action. Your program performs that action, if it is allowed, and puts the result back into the situation. The model is not the agent. The loop is.

You will write the loop in plain Python before you adopt a framework. Frameworks hide the state you need to see while you are learning: the message list, the tool schema, the raw model output, and the branch you took when the output was nonsense.

This path comes after you can run a model (path 02 lab 01) and after you have a retriever (path 04 lab 03). The capstone can call the algorithm coder from path 02 if that adapter exists. If it does not yet, the capstone still works with the untouched base model and your tools.

## The loop, which every lab extends

```text
messages = [system instructions, user request]
for step in range(max_steps):
    reply = model(messages)
    if reply is a final answer:
        return reply
    if reply is a tool call:
        result = run_tool(reply.tool, reply.arguments)   # your code, not the model's
        messages.append(reply)
        messages.append(tool_result(result))
return "stopped: step limit"
```

The model never executes a tool by wishing it. It emits a description of a call. You parse it, you run it, you report what happened, including crashes.

## Labs

| Lab | You add to the loop |
|---|---|
| [01 The agent loop](01-the-agent-loop/README.md) | A visible state machine, one dummy tool |
| [02 Instructions and structured output](02-instructions-and-structured-output/README.md) | A schema the model must fill, and a parser that can fail |
| [03 Tools](03-tools/README.md) | Real functions with checked arguments |
| [04 Retrieval](04-retrieval/README.md) | Your notes as a tool |
| [05 Memory, planning, and failure](05-memory-planning-and-failure/README.md) | What to keep between steps, and a test suite for bad behavior |
| [06 Capstone](06-capstone-a-local-study-agent/README.md) | A study agent over this course |

## How deep to go

- Log every message on every step for every experiment. When something fails you need the transcript, not a summary.
- Set `max_steps` and a timeout on every tool. An unbounded loop is not a patient agent. It is a stuck program.
- Prefer tools that read and compute. A tool that deletes files, sends mail, or spends money is out of scope for the course.
- Generated code, if you allow it, runs in a subprocess with a timeout and a working directory you created for that run. Do not run it in the repo root.
- Judge the agent by task success on a fixed set of tasks. A fluent final paragraph can coexist with a wrong tool result. Path 06 applies here too.

## Done with this path when

- You can draw the loop and say which box is the model and which box is your process.
- A parser rejects malformed tool calls and the loop asks the model to repair them, up to the step limit.
- A fixed suite of tasks reports a success count, and you have at least five transcripts of failures tagged by cause: bad tool choice, bad arguments, ignored tool result, looped, or hit the step limit.
- The study-agent capstone answers a question about your notes by quoting a file it actually retrieved.
