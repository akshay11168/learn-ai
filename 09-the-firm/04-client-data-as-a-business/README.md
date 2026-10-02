# 04 — Client data as a business

Prerequisite: [path 07 lab 05](../../07-production-practice/05-security-and-privacy/README.md) and [03 Contracts, IP, and liability](../03-contracts-ip-and-liability/README.md).

## Purpose

Write the operating rules for client data before the first file arrives. A privacy paragraph in a proposal is not a habit. The habit is where files live, who can open them, what never enters a public model API, and how you delete them.

## Explain before you code

1. Why is a copy of a client's spreadsheet in your personal Downloads folder, and another in a chat tool, already a policy failure?
2. When is sending their text to a hosted model API a decision the contract must mention first?
3. What do you lose if you cannot delete their data at the end of the SOW because it is mixed into a training run you also used for someone else?

## Policy

Write `data-policy.md` in rules you can follow on this PC.

- **One folder per client** on a drive location you name, separate from `learn-ai` and from your personal documents. No client data in the git repo, in screenshots posted online, or in a chat with a consumer AI tool.
- **Inventory** the moment data arrives: what it is, date, whether it contains personal data, where it is allowed to be processed (local GPU only, or a named provider).
- **Hosted models.** Default is off for client text. If a project needs a provider, the SOW names the provider, and you have read the current terms on that date. Record the date.
- **Training.** Do not train one client's data into a shared base you will reuse on another client. Adapters and datasets stay in the client folder. Your generic code can be reused. Their examples cannot, unless the contract says you may keep anonymized aggregates and you actually anonymized them.
- **Logs.** The production logging rule applies: no raw personal data in logs.
- **People.** If a subcontractor appears later, they get the minimum folder, not your whole disk, and a written confidentiality promise your lawyer has approved.
- **End of job.** A deletion or return step, with a date, checked off. Secrets and tokens they gave you (API keys, VPN) are revoked on their side and removed on yours.
- **Breach.** If a file goes somewhere it should not, you tell the client what happened, what was in it, and what you did. Write the sentence you would send, as a template, before you need it. Ask the lawyer later whether your country requires a specific notice. The template does not replace that duty.

Practice the mechanics once with a fake client folder and a dummy file: create it, inventory it, run a pretend job, delete it, and note the deletion. Use no real person's data.

## You are done when

The policy fits on two pages, you have rehearsed the folder lifecycle with dummy data, and the SOW assumptions point at "client data stays in the project folder and is not sent to a hosted model unless the SOW names one."
