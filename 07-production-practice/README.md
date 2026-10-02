# 07 — Production practice

A model that scores well in a lab is not yet a system a person can depend on. Production work adds a decision, a service boundary, a cost, a way to refuse unsafe inputs, a test set that can stop a release, and a document someone else can use to run it. This is the technical standard behind the consulting path. Buyers who have been burned by a demo ask for these pieces by other names: "How do we know it works?", "What does it cost per day?", "What happens when it is wrong?", "Who owns the data?"

Do this path on one use case you have already finished in path 04. The photo classifier, the text classifier, and the notes question-answerer are all valid. An agent from path 03 is the right target if you want the security lab to be sharp. Do not start a new product idea here.

## Labs

| Lab | Artifact |
|---|---|
| [01 The smallest system](01-the-smallest-system/README.md) | A written decision: rules, classic model, prompt, retrieval, fine-tune, or agent, and what you rejected |
| [02 Data you can defend](02-data-you-can-defend/README.md) | A labeling guide, a split rule, and a data inventory |
| [03 Serve it](03-serve-it/README.md) | A local HTTP service with a health check and a version |
| [04 Cost, latency, and failure](04-cost-latency-and-failure/README.md) | A measured budget per request and a timeout policy |
| [05 Security and privacy](05-security-and-privacy/README.md) | A threat list you tried to break, and a logging rule |
| [06 An eval that can block a release](06-an-eval-that-can-block-a-release/README.md) | A golden set and a pass/fail gate |
| [07 Handover](07-handover/README.md) | A model card, a runbook, and a human-review rule |

## Industry habits this path drills

- Version the data, the code, and the weights together. A metric without those three cannot be reproduced. Your experiment log from path 06 is the start of this.
- Prefer a deterministic baseline in the request path when the cost of a mistake is high. The model handles the residue.
- Measure on a frozen set before you change a prompt, a threshold, or a checkpoint. A change that is not gated will be "improved" by memory.
- Log enough to debug a bad answer, and do not log secrets or raw personal data you do not need.
- Write the failure behavior first: timeout, low confidence, and "I do not know" are product features.

## Done with this path when

One of your use cases has a service, a golden-set score, a cost note, a short threat note, and a handover folder a stranger could operate. That bundle is the technical half of a consulting delivery.
