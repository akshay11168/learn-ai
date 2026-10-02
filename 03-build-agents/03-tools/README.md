# 03 — Tools

Prerequisite: [02 Structured output](../02-instructions-and-structured-output/README.md).

## Purpose

Replace dummy tools with a small set of real functions, each with a name, a docstring the model sees, an argument checker, and a timeout. The model selects. Your registry executes.

## Explain before you code

1. Why does the tool's description belong in the prompt, in your words, rather than only in a Python docstring the model never receives?
2. What is the blast radius of a tool that takes a filesystem path?
3. Why return tool errors as data inside the loop, instead of raising them out of the agent?

## Build

A registry: a dict from name to a function, plus a schema. Implement four tools.

| Tool | Behavior | Limit |
|---|---|---|
| `calculate` | Evaluate an arithmetic expression you parse yourself (numbers and `+ - * / ()`). Do not pass the string to Python `eval` | Reject anything else |
| `read_note` | Read a file from a single directory `D:\learn-ai-data\agent-notes` | Refuse paths that escape the directory after resolving `..` |
| `list_notes` | List filenames in that directory | No arguments |
| `word_count` | Count words in a string argument | Cap the string length |

Write three notes in that directory so `read_note` has something true to return.

For each tool, tests that do not involve the model: good arguments, bad arguments, and for `read_note` a path outside the directory.

Wire the registry into the loop. The system prompt lists each tool's name, arguments, and one sentence on when to use it. You will edit those sentences after you see mistakes. That edit is prompt engineering grounded in a transcript, which is the version worth learning.

Tasks, saved as transcripts:

1. Compute `(17 + 25) * 2` via the tool.
2. List the notes and read one of them. The final answer must contain a sentence that is actually in the file. Your test can check that.
3. A request that needs two tools in sequence (list, then read, or compute then include the number in a summary of a note).
4. A request to read `C:\Windows\win.ini` or a path with `..`. The tool must refuse, and the final answer must not contain the file.

## Verify

The escape-path test passes. Task 4's transcript shows a refusal from your code. Task 2's answer is grounded in the file, checked by a string that you know is in the note. No tool uses `eval` or a shell.

## Stretch

Add a timeout wrapper that kills a tool after one second. Give it a dummy tool that sleeps two seconds and confirm the loop records a timeout result and continues or stops by your rule.

## Study this step

**Concepts to master**

- A tool is a function plus a schema plus a description the model sees plus an authorization check the model does not control.
- The registry is a dispatch table. The loop should not grow a new `if` per tool.
- Path traversal is joining a user-controlled string to a directory and escaping it with `..` or an absolute path. The fix is to resolve the path and check that it stays inside the root.
- `eval` and a shell are not calculators. They are code execution. An arithmetic parser accepts a grammar you wrote.
- Errors return as tool results. They do not escape the loop, or one bad file read kills a long task with a traceback the model never sees.
- Least privilege: the tool can do the smallest thing the task needs. A reader is not a writer.

**Study**

- OWASP, "Path Traversal": https://owasp.org/www-community/attacks/Path_Traversal — read the attack description and the "how to prevent" section. Map each prevention onto `read_note`.
- Python docs for `pathlib.Path.resolve` and the warning about symlinks: https://docs.python.org/3/library/pathlib.html — write the check `root in path.parents or path == root` after resolve, and test it.
- Lilian Weng's agent post, the "Tool use" section only: https://lilianweng.github.io/posts/2023-06-23-agent/ — compare her open-ended tool list with your four tools and say which of hers you are refusing to add yet.

**Practice**

- Tests that never call the model: happy path, missing file, `..\\..\\Windows`, an absolute path, a non-numeric `calculate` string, a too-long `word_count`.
- Log the resolved path in the test failure message so a broken check is obvious.
- Add the timeout wrapper from the stretch on a sleeping fake tool and assert the loop records timeout rather than hanging.

**Practice questions**

1. The model asks `read_note` for `notes/../../secret.txt`. What does your function return, and what does the model never receive?
2. Why is `eval(expr)` wrong even if you "only expect arithmetic"?
3. `calculate("(2+3)*4")` should return what, and which character class must the parser reject?
4. A tool description says "reads any file the user names." The code limits the directory. Which one is the security boundary, and why must the description still not promise the wider power?
5. The tool raises `FileNotFoundError` and you do not catch it. What does the user see, and what should they see instead?
6. Why does each new tool need a test that does not involve the model?

## You are done when

Adding a fifth tool means one registry entry, one paragraph in the prompt, and one test, with no change to the loop's control flow.
