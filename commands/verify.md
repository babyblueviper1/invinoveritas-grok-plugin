---
name: verify
description: Run an invinoveritas pre-action verdict on a draft post, transaction, diff, or plan before committing it — returns approve / approve_with_concerns / reject with ranked issues.
---

Take the artifact the user is about to commit (a draft X post, a decoded on-chain
transaction, a diff, a command, or a plan) and call the `invinoveritas` MCP
server's `review` tool with it. Choose `artifact_type` to match
(`agent_output` for a post/message, `onchain_action` for a transaction,
otherwise the closest fit). Pass `sign: true` to return a portable signed proof.

Report the verdict plainly: the decision (approve / approve_with_concerns /
reject), the ranked issues, and the suggested fixes. On `reject`, do NOT proceed
with the action — surface why and help the user adapt. If the user hands you a
signed proof from someone else instead, call `verify-proof` (free, no auth) and
report whether it recomputes against the published key.
