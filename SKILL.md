---
name: agentfleet-core
description: Orchestrate a cost-aware multi-agent team with explicit model tiers, reasoning effort, context isolation, review gates, and curated memory. Use when the user asks for an agent team, parallel subagents, Sol/Terra/Luna routing, or reusable multi-agent execution. Do not activate for a routine single-step task unless explicitly invoked.
---

# AgentFleet Core

Run a bounded agent team while keeping planning authority, context, and persistent memory under explicit control.

## Load the operating profile

Read [references/default-profile.md](references/default-profile.md) before delegating. If `references/team-profile.local.md` exists, read it afterward and treat it as an outer-layer override.

Read only the additional reference needed for the current operation:

- For delegated task packets and result packets, read [references/handoff-contract.md](references/handoff-contract.md).
- When work may create or update persistent memory, read [references/memory-policy.md](references/memory-policy.md).
- When the user asks to customize, fork, or extend the fleet, read [references/customization.md](references/customization.md).

## Stable kernel

Preserve these invariants even when the outer profile changes:

1. The parent agent owns requirements, irreversible decisions, final validation, and the user-facing answer.
2. Use the lowest-cost model and lowest reasoning effort that can reliably complete each bounded task.
3. Keep subagent context minimal. Send a task packet, relevant evidence, and selected memory; do not share the complete conversation by default.
4. Parallelize independent read-heavy work. Partition write ownership by file or module, or serialize it.
5. Every delegated task has a definition of done, allowed tools, output contract, and escalation condition.
6. Treat subagent output as evidence, not truth. Validate material claims and changes before acceptance.
7. Workers may propose persistent memories; only the parent may approve promotion. Never persist hidden reasoning or raw chain-of-thought.
8. Delegation does not broaden permissions. Stop for authorization before external publication, destructive actions, secrets access, or scope expansion unless already authorized.

## Execute

1. Classify the request.
   - Stay single-agent for straightforward work unless the user explicitly requests a fleet.
   - Use a fleet when work divides into meaningful independent parts, needs specialized context, or benefits from separate implementation and review.
2. Build a small task graph. Name dependencies, write ownership, and acceptance checks. Avoid spawning an agent whose work is cheaper to do directly.
3. Route each task through the active profile. Pin both model and reasoning effort whenever the runtime permits; do not rely on accidental inheritance.
4. Create a minimal handoff packet. Share full history only when the task cannot be completed from a bounded packet and the privacy or context-cost tradeoff is justified.
5. Delegate independent tasks in parallel up to the profile limit. Keep dependent and overlapping write tasks sequential.
6. Wait for required results. Ask for concise evidence and artifacts, not verbose internal process logs.
7. Review outputs against the original definition of done. Escalate disagreements, repeated failures, high-risk decisions, or weak evidence to the strategist tier.
8. Synthesize the final result. If durable lessons emerged, produce memory candidates and apply the memory policy.

## Degrade gracefully

If a configured model, reasoning level, custom agent, or subagent tool is unavailable, choose the closest available capability, reduce parallelism if necessary, and disclose the substitution briefly. Do not pretend the requested fleet ran when it did not.
