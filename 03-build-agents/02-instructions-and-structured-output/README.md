# 02 — Instructions and structured output

Prerequisite: [01 The agent loop](../01-the-agent-loop/README.md).

## Purpose

Make the model's action a JSON object that either parses or fails. The system prompt is a specification, and the parser is the enforcer. Prose around the JSON is a bug.

## Explain before you code

1. Why is "be helpful and use tools when appropriate" a weak specification?
2. What fields does a tool call need so that your code can run it without guessing?
3. If parsing fails, what message do you append so the model can correct itself, and what stops that correction from looping forever?

## Build

Define a schema, written in the system prompt and checked in Python:

```json
{"type": "tool", "name": "add", "arguments": {"a": 1, "b": 2}}
```

or

```json
{"type": "final", "answer": "..."}
```

`parse_action(text) -> dict` raises a typed error on invalid JSON, missing fields, the wrong `name`, or arguments that are not numbers. The loop catches that error, appends a short repair message that quotes the error, and continues. After two repair failures, stop with `stopped: parse`.

Add a second dummy tool, `now()`, with no arguments, so the model must pick a name. Tasks:

- Addition, as before.
- "What time is it?" which should call `now`. Your `now` returns a fixed string in tests and the real time in a manual run. Fixed time keeps the test deterministic.
- A request that should be final, with no tool.
- A unit test that feeds malformed model output directly to the loop (you do not need the model for this) and asserts the run stops or repairs.

Save one real-model transcript where the first emission failed to parse, if you can provoke one by asking for a slightly unusual format. If the model is perfectly formatted on your first try, temporarily add a contradictory line to the system prompt to force a bad sample, observe the repair, then remove the contradiction. Record that you did so.

## Verify

Unit tests cover invalid JSON, unknown tool name, and wrong argument type, without calling the model. At least one end-to-end transcript shows a repair or a clean first parse. You can explain why the schema lives in both the prompt and the parser.

## Stretch

Ask the model to emit the action and nothing else. Measure how often a sample still wraps the JSON in markdown fences. Strip a single fence in the parser if you need to, and treat that as a compatibility hack you documented.

## You are done when

A bad action cannot crash the process and cannot run a tool, and the transcript shows the rejection.
