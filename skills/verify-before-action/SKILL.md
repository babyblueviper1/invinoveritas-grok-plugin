---
name: verify-before-action
description: >-
  Get an independent, recomputable verdict BEFORE an agent commits an
  irreversible action — posting to X, signing or sending an on-chain
  transaction, shipping a code change, or any high-impact tool call. Triggers:
  "should I post this", "is this safe to sign", "verify before posting",
  "review this transaction", "pre-trade check", "is this action sound",
  "get a second opinion", or any time the agent is about to use a write/post/
  send/sign tool (e.g. the X MCP createPosts/likePost, a wallet signTransaction).
  Also: "verify this proof" / "is this verdict real" → `verify_proof`.
---

# Verify before action

invinoveritas is a verification layer for autonomous agents: a neutral second
opinion *before* an irreversible action, and a signed, independently-checkable
proof *after*. The verdict is **recomputable** — a downstream party confirms it
against a published key without trusting the agent or invinoveritas.

Use this whenever an agent is about to do something it can't take back.

## When to call it

- **Before posting to X** (the hosted X MCP `createPosts` / `likePost`): the X
  API authenticates *who* may post; it does not judge *whether the post should
  go out*. Call `review` on the draft first.
- **Before signing/sending on-chain** (a wallet `signTransaction`): catches
  drainer approvals, honeypot/scam tokens, address poisoning, wrong-chain
  recipients, slippage/MEV — deterministic and recomputable.
- **Before shipping** a diff, command, config, or plan that's hard to reverse.

## How to use it (MCP tools)

This plugin adds the `invinoveritas` MCP server. Two tools matter:

- **`review`** — submit the artifact (the draft post, the decoded transaction,
  the diff/plan). Returns a structured verdict: `approve` / `approve_with_concerns`
  / `reject`, with ranked issues and suggested fixes. Set `sign: true` to also get
  a portable signed proof. Use the verdict as a gate: on `reject`, do not commit
  the action — surface the issues and adapt.
- **`verify_proof`** — free, no auth. Hand it a signed verdict proof you received
  from another agent and it confirms the proof recomputes against the published
  key — so you trust the *proof*, not the presenter.

Pattern for a gated X post:

```
draft = agent writes the post
result  = review(artifact=draft, artifact_type="agent_output", sign=true)
if result.verdict == "reject": stop, show result.issues
else: X-MCP createPosts(draft)   # now carrying a recomputable proof
```

## Auth

`verify_proof` is free and needs no key. `review` is a paid call — register free
at https://api.babyblueviper.com/register, fund with Lightning sats or USDC
(x402 on Base), and set your Bearer token. The verdict itself is recomputable by
anyone; you pay for the judgment, not for the right to check it.

## Why recomputable matters

A guardrail that only an enforcer can attest to is "trust the guardrail." An
invinoveritas verdict re-derives from the same inputs by a party that isn't the
acting agent, and a ``verify_proof`` check confirms it independently. That's the
difference between an audit log you keep and evidence a counterparty can check.
