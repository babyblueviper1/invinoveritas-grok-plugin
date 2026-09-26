# invinoveritas — Grok Build plugin

**Verify before your agent acts.** A neutral, *recomputable* pre-action verdict and a portable signed proof for Grok Build agents — gate irreversible actions (posting to X, signing an on-chain transaction, shipping a change) before they fire.

invinoveritas is a verification layer for autonomous agents: a neutral second opinion *before* an irreversible action (`review`), and a signed proof *after* that anyone can re-derive against a published key without trusting the agent — or us (`verify_proof`).

## What it adds

- **MCP server** (`https://api.babyblueviper.com/mcp/verify`, the focused verification endpoint) exposing:
  - **`review`**: structured pre-action verdict (`approve` / `approve_with_concerns` / `reject`) with ranked issues and suggested fixes; `sign: true` returns a portable signed proof.
  - **`verify_proof`**: free, no sign-in. Confirms a signed verdict proof recomputes against the published key (trust the proof, not the presenter).
  - **`ledger`**: free, no sign-in. The public, signed track record of verdicts, including the ones that turned out wrong.
  - Also `witness` (timestamp a third party's exact claim), `validate` (backtest reality-check), `audit_agent_readiness`, `ledger_submit`, `conformance_certify`.
  - Paid tools need a linked account: OAuth sign-in when your client supports it (a free account can be created on the consent screen), or an invinoveritas API key as a Bearer token. The server never moves money or assets for you; usage is billed to your prepaid balance.
- **Skill** `verify-before-action` — triggers when an agent is about to post, sign, send, or ship.
- **Command** `/verify` — run a verdict on the current draft/transaction/diff on demand.

## Headline use case: gate your agent's X posts

The hosted X MCP lets a Grok agent post to X autonomously. The X API authenticates *who* may post; it does not judge *whether the post should go out*. Pair them:

```
draft   = agent writes the post
result  = review(artifact=draft, artifact_type="agent_output", sign=true)
if result.verdict == "reject": stop, surface result.issues
else: X-MCP createPosts(draft)   # carrying a recomputable proof
```

## Auth

`verify_proof` is free and needs no key. `review` is a paid call — register free at https://api.babyblueviper.com/register, fund with Lightning sats or USDC (x402 on Base), set your Bearer token. The verdict is recomputable by anyone; you pay for the judgment, not the right to check it.

## Links

- API + docs: https://api.babyblueviper.com
- Public verdict ledger: https://api.babyblueviper.com/ledger
- Conformance registry (how verifiers are graded): https://api.babyblueviper.com/conformance

License: MIT. This plugin is developed by invinoveritas, not xAI.
