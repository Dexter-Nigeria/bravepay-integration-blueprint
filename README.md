# BravePay · BankOne · NIBSS — Integration Blueprint

Technical integration blueprint for running BravePay on **BankOne (Qore)** as the core
banking engine, with **NIBSS** providing the payment rails.

**Read it here:** https://claude.ai/code/artifact/45807891-1789-400b-98ad-7c31c00897b0
(private — visible to the owner and anyone the link is deliberately shared with)

Or open `index.html` locally; it is a single self-contained file.

## Two documents

| Document | Audience | File |
|---|---|---|
| **[How BravePay Moves Money](https://claude.ai/code/artifact/bea92fd6-cd1d-44da-a5f3-9599f116b599)** | Non-technical — board, ops, commercial, support | `explainer.html` |
| **[Integration Blueprint](https://claude.ai/code/artifact/45807891-1789-400b-98ad-7c31c00897b0)** | Engineering | `index.html` |

The explainer covers the same arrangement in ordinary language: who does what, how
BankOne reaches NIBSS, how BravePay reaches BankOne, and why the HTTP/HTTPS question
matters. No technical background needed.

## What the blueprint is

A build-ready document for the engineering team, not an overview. It covers:

| § | |
|---|---|
| 01–04 | Evidence tiers, the ownership answer, BankOne's real API surface, and the three API facts that shape the design |
| 05–08 | BravePay ↔ BankOne architecture, BankOne ↔ NIBSS, transaction lifecycles, the nine-state machine |
| 09–15 | Module-by-module disposition, data ownership, security, failure/recovery, ledger & reconciliation, internal services, target architecture |
| 16–20 | Ten delivery phases, stack notes, regulatory obligations, gap analysis, and the open questions for Qore and NIBSS |

## Evidence tiers

Every non-obvious claim is tagged:

- **documented** — from BankOne's published API docs (all 78 pages retrieved and read)
- **bravepay code** — read from this organisation's repositories, file and line given
- **inferred** — a design conclusion drawn from the two above
- **ask qore** / **ask nibss** — not answerable publicly; must be confirmed before building against it

## Three findings that drive the design

1. **No webhooks.** Nothing in 78 pages pushes an event. Inbound credits are discoverable only by polling, which changes the cost and shape of the whole integration.
2. **94 production endpoints are published as `http://`**, with the auth token in the query string on the Corebanking half. Blocking pre-production question.
3. **Transaction references cap at 12 characters.** BravePay currently emits 16. The reference is also the only idempotency key available.

## Status

Draft v1.0, 3 September 2026. Sections tagged *ask qore* / *ask nibss* are unresolved —
see §20 for the exact list to send.
