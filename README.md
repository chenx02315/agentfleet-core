# AgentFleet Core

AgentFleet Core is an open Codex skill for running a cost-aware multi-agent team. It keeps a small coordination kernel stable while letting users replace models, reasoning effort, roles, memory policy, and specialist agents in the outer profile.

## Default fleet

- GPT-6.1 Sol/medium: planning and general implementation; high for difficult decisions and review.
- GPT-6 Luna/high: bounded discovery, small clear changes, and memory candidates.
- GPT-6 Astra/low: hardest unresolved problems; raise effort when evidence warrants it.
- The parent retains memory approval and final acceptance. Role effort is a starting point, adjusted to task difficulty and runtime support.

Model guidance checked 2026-10-07. Existing Terra template names are retained for compatibility, but now select Sol. The outer profile defines availability checks, fallback and escalation; custom TOML agents each pin one model.

## Install

Ask Codex:

```text
$skill-installer install the skill from https://github.com/chenx02315/agentfleet-core
```

Or copy this repository to `~/.agents/skills/agentfleet-core`.

Optional custom-agent templates live in `assets/agents/`. Copy the agents you want to `~/.codex/agents/`, then restart Codex if they do not appear automatically.

## Use

```text
Use $agentfleet-core to plan and execute this task with the right agent team.
```

Codex may also select the skill automatically when you explicitly ask for an agent team, parallel subagents, model-tier routing, or curated multi-agent memory.

## Customize without changing the kernel

Keep `SKILL.md` stable. Edit `references/default-profile.md`, or create `references/team-profile.local.md` with only your overrides. Add or modify TOML agent templates under `assets/agents/`.

Examples of safe customization:

- Replace Sol, Terra, or Luna with another available model family.
- Change reasoning effort by role.
- Add domain specialists such as security, finance, research, or frontend agents.
- Tighten concurrency, permissions, retries, and memory retention.
- Connect external memory through MCP while preserving parent approval.

See `references/customization.md` for the extension contract.

## Design boundary

Extensions may add capability but should not remove parent accountability, permission checks, output validation, minimal-context sharing, write ownership, or the memory approval gate.

## License

MIT
