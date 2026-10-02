# 03 — Contracts, IP, and liability

Prerequisite: [the SOW lab](../../08-consulting/03-proposal-and-statement-of-work/README.md) and [02 Entity, tax, and the professionals](../02-entity-tax-and-professionals/README.md).

## Purpose

Learn the clauses that decide who owns the work, who is responsible when it fails, and what "done" means, so a lawyer's edits make sense to you. You will mark up your practice SOW. You will not invent a contract and use it unsigned by a lawyer on a real client.

## Explain before you code

1. If the contract is silent on who owns the fine-tuned weights and the labeled data, what fight are you postponing?
2. Why is an unlimited liability clause a different risk for a one-person firm than for a large vendor?
3. What is the difference between a statement of work and a master services agreement?

## The pieces

**Master services agreement (MSA).** The standing rules: payment terms, confidentiality, liability cap, who owns what, how you may use public experience, how either side ends the relationship. You sign it once.

**Statement of work.** The pilot you already drafted: scope, price, metric, exclusions. Several SOWs can sit under one MSA.

**Order of a fight.** If the SOW and the MSA disagree, the contract says which one wins. You want to know which.

Read your practice SOW and add a margin note, not legal language, for each item below. Your lawyer turns the notes into clauses.

| Topic | What you are trying to protect |
|---|---|
| Scope and change control | Work outside the SOW is estimated again before you do it |
| Acceptance | Delivering the named artifacts and reviewing the metric is acceptance. A hope about next quarter's revenue is not a hidden test |
| Client data | They own their data. You use it only to perform the SOW. You delete or return it when the SOW says |
| Your pre-existing tools | Code and methods you brought in (your training loop, your eval harness) stay yours. You license them as needed to run the pilot |
| Newly created artifacts | Agree explicitly: the report and the fine-tune might be theirs; your generic harness stays yours. Silence here is expensive |
| Third-party models | A base model has its own license. The client does not "own GPT" or the open weights. The contract should not promise that |
| Confidentiality | You do not put their data in case studies or public prompts. A public write-up needs their written yes |
| Liability cap | Your exposure should be limited to a defined amount, often the fees on that SOW, with the exceptions the lawyer says you cannot limit |
| No warranty of accuracy | The metric is a measurement on a named set. You do not warrant that every future input will be correct |
| Non-solicitation and publicity | Only if you mean them. Do not copy a harsh clause you do not understand |
| Termination | How either side stops, what is paid for work done, and what happens to the data |

**Insurance.** Ask the lawyer and an insurance broker, not a blog, whether professional indemnity (errors and omissions) insurance is appropriate before the first serious client. Note the question in `brief.md`. Do not buy a policy you have not read in order to feel finished.

**Claims.** Your website and proposals may not promise a result the contract disclaims. Make those documents match.

## Build

`clause-notes.md` with a row for each topic, written against your practice SOW: "today the draft says nothing; I need the lawyer to cover it this way." Add the third-party model license name for the model you actually use, and a sentence on what that license allows a client to do.

## You are done when

You can explain, without bluffing, who owns the data, who owns your harness, what the metric does not promise, and which of those sentences still need a lawyer's clause.
