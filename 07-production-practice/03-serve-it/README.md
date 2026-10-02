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

## You are done when

You can restart the process and a single request still returns the same version and the same label on the example input.
