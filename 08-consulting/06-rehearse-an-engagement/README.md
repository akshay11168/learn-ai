# 06 — Rehearse an engagement

Prerequisite: the rest of path 08, and [path 07 handover](../../07-production-practice/07-handover/README.md) for the same use case.

## Purpose

Assemble one engagement from discovery through handover, as if a client had commissioned your existing project. This is the dress rehearsal before you take money. The artifacts should agree with each other. A proposal that promises a different metric from the model card is the kind of inconsistency that ends a real engagement.

## Build

Create `packet/` in this folder, or a single `PACKET.md` with links. It contains:

1. Wedge sentence, and why this problem fits.
2. Discovery notes and the problem statement.
3. The SOW and the price, including exclusions.
4. The decision record (which rung, which baseline).
5. The weekly update and the readout.
6. Links to the model card, runbook, privacy note, and golden-set result.
7. A one-page "if this were a real client" gap list: what you would still need from them (data permission, a reviewer, hosting, a contract reviewed by your lawyer).

Then do a tabletop review. Read the packet in one sitting and check:

- The metric is the same number in the SOW, the readout, and the model card.
- The exclusions match the risks.
- The price matches the hours.
- The case-study tone does not claim a client you did not have.
- The security and privacy notes are mentioned in the SOW assumptions, at least by pointing at whose data stays where.

Fix contradictions. Do not add new scope during the review.

## Optional and valuable

Ask one person who is not an ML specialist to read the problem statement and the readout outline. Ask them what they think you are delivering and what it costs. Where their answer differs from the SOW, the writing is wrong. Adjust the writing.

## Study this step

**Concepts to master**

- One engagement, one metric, one price, one data story. Contradictions between the SOW and the model card are defects.
- The gap list is what a real client would still have to provide. Hiding it is how rehearsals lie.
- A tabletop read is you pretending to be the buyer for one sitting.
- This packet is not permission to take data or money. The lawyer and the data agreement are still outside the rehearsal.

**Study**

- Your own labs 01–05 of this path, in order, with a pen, looking only for mismatched numbers.
- Path 07 handover documents for the same use case.
- Hamel Husain's eval post one more time, as the standard the readout has to live up to: https://hamel.dev/blog/posts/evals/

**Practice**

- Build the packet as links, not as pasted numbers that can drift. If you must paste a number, paste it once and point elsewhere.
- Read as the buyer. Write five questions you would ask. Answer them by editing the packet or by adding them to the gap list.
- If a non-specialist will read it, ask only what they think is being delivered and what it costs.

**Practice questions**

1. The SOW says macro F1 and the model card says accuracy. What do you fix before anyone sees the packet?
2. Why does the gap list include data permission and hosting instead of pretending you already have them?
3. The case study implies a paying client. What sentence must you add or remove?
4. Name two conditions under which the packet still must not be sent.
5. What did the rehearsal change in the SOW?

## You are done when

The packet is consistent, the gap list is honest, and you would send it to a real buyer after a lawyer had seen the contract and the client had agreed to the data use. Until those two are true, it stays a rehearsal.
