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

## Study this step

**Concepts to master**

- A schema is a type for an action. The prompt asks for it. The parser enforces it. The model cannot be the enforcer.
- Failure modes of parsing: not JSON, extra prose, missing fields, wrong types, unknown tool names. Each one needs a test that does not call the model.
- A repair message quotes the error and consumes a step. Two repairs then stop. Otherwise a chatty model loops until the step limit on syntax.
- Structured output does not make the arguments true. It makes them well-typed. `add` with `a = "seventeen"` is a type failure. `add` with `a = 17` and a buggy implementation is a tool failure.
- Temperature 0 reduces format noise. It does not remove it. Tests that feed canned bad strings are the real parser tests.

**Study**

- JSON schema's core idea, the "type", "properties", and "required" keywords only: https://json-schema.org/understanding-json-schema/reference/object — you are not adopting the whole spec. You are stealing the idea that required fields are declared.
- Anthropic or OpenAI docs on tool use / structured outputs, one current page from the vendor of a model you might call later. Read the request shape. Then close it and keep your hand-rolled parser. The point is to see that production APIs are the same contract: a name, arguments, and a parser.
- The Python `json` module docs, `JSONDecodeError`: https://docs.python.org/3/library/json.html — catch that exception by name in your parser.

**Practice**

- Write five invalid payloads and assert the parser raises before any tool runs. Do this before the model is wired.
- Provoke a fenced ```json block and decide, in writing, whether stripping one fence is allowed. Test both the fenced and the unfenced input.
- Count steps used by a repair. Show that the third failure stops.

**Practice questions**

1. The model emits valid JSON with `"name": "drop_table"`. What happens, line by line?
2. Why is a unit test that mocks the model required, instead of only end-to-end tests?
3. The repair message includes the bad output. How could that message itself grow without bound, and what limit stops it?
4. Arguments `{"a": "1", "b": 2}` reach `add`. Should the parser coerce the string? Argue one side and implement that side only.
5. Why does "be helpful and use tools when needed" not specify the action type?
6. You change the schema and forget to change the prompt. Which test fails first, the parser test or the live model test?

## You are done when

A bad action cannot crash the process and cannot run a tool, and the transcript shows the rejection.
