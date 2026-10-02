# 03 — Serve it

Prerequisite: [02 Data you can defend](../02-data-you-can-defend/README.md), and a checkpoint or prompt setup that already runs as a script.

## Purpose

Put the system behind a local HTTP service with a version, a health check, and a request shape that does not require the caller to know PyTorch. Consulting deliverables almost always end at an interface someone else's software can call. A notebook is not that interface.

## Explain before you code

1. Why does the caller send raw input (text, a file path, or bytes) instead of a tensor?
2. What should the health check prove: that the process is up, or that the model actually loaded?
3. Why pin the response to a model version string that changes when the checkpoint or the prompt changes?

## Build

Use the course venv. `pip install fastapi uvicorn` is enough. Bind to `127.0.0.1` only. This service is for your machine, not a public deployment.

Implement:

- `GET /health` returns `{"status": "ok", "model_version": "..."}` after the weights or the prompt template have loaded. If loading failed, the process should not pretend to be healthy.
- `POST /predict` accepts the real input of your use case. It returns the label or answer, a confidence or an abstention flag if you have one, and `model_version`.
- The same preprocessing as training. Put preprocess in one function imported by both training evaluation and the service. A second, slightly different resize or tokenizer is a classic way to ship a model that scored well and behaves worse.
- A timeout inside the handler if generation or inference can hang. Return a clear error the caller can handle.
- `serve.md` with the command, an example request using `curl` or PowerShell `Invoke-RestMethod`, and the example response.

Do not add authentication yet unless you want the stretch. Do not open the port on the LAN. Write in `serve.md` that this bind address is a local demo, and a client deployment needs a private network, authentication, and someone else's operations choices.

Load the model once at startup, not on every request.

## Verify

Call `/health`, then `/predict`, with the example from your path 04 script. The label matches that script on the same input. Call `/predict` with a bad input (empty string, missing file) and confirm you get a controlled error, not a stack trace pasted into a client's log. A stack trace is fine in your console while you develop. The HTTP body should be a short message you wrote.

## Stretch

Add a `GET /version` that reports the git commit, the data-folder date from lab 02, and the checkpoint filename. Those three are the start of a reproducible release.

## Study this step

**Concepts to master**

- A service is a process with a contract: request shape, response shape, version, and error body. A script is not a contract until those are stable.
- Load the model at startup. Per-request loading hides a latency bug and a failure mode.
- Health means the model is loaded, not merely that the port is open.
- Preprocessing is shared code. Two copies will drift.
- Bind to localhost until you have an explicit deployment threat model. A demo port on the LAN is an incident.

**Study**

- FastAPI tutorial, "First steps" and request body: https://fastapi.tiangolo.com/tutorial/first-steps/ and https://fastapi.tiangolo.com/tutorial/body/ — stop before databases.
- HTTP status codes you will actually return: 200, 400, 422, 500. MDN's status overview: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status
- Twelve-Factor, "port binding": https://12factor.net/port-binding — the idea that the service is a process you can start and hit. Ignore the rest of the site for now.

**Practice**

- Call the example with `Invoke-RestMethod` or `curl`. Save the response next to the CLI script's output and diff them.
- Kill the process mid-request and see what the client observes. Write it down.
- Return a controlled 400 for an empty input. Confirm the body has no traceback.

**Practice questions**

1. Why must `/health` fail if the checkpoint path is wrong, instead of returning ok and erroring on the first real user?
2. Two preprocessing functions differ by a resize. The notebook score and the service disagree. Which one is the bug?
3. Why is binding `0.0.0.0` a different decision from binding `127.0.0.1`?
4. What does `model_version` need to include so two releases are distinguishable?
5. A stack trace in the JSON body teaches the caller what about your disk layout, and why is that undesirable?

## You are done when

You can restart the process and a single request still returns the same version and the same label on the example input.
