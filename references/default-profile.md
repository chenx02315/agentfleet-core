# Default fleet profile

This file is the editable outer layer. The stable coordination invariants remain in `SKILL.md`.

## Runtime defaults

- Maximum concurrent subagents: 4
- Default context policy: selected context only
- Default write policy: one owner per file or module
- Default escalation: retry once with improved evidence, then escalate capability

## Roles

| Logical role | Preferred model | Reasoning | Default access | Use for |
|---|---|---|---|---|
| strategist | `gpt-6.1-sol` | `medium`; `high` for substantial ambiguity | read-only unless implementation is explicitly assigned | architecture, planning, arbitration, consequential review |
| scout | `gpt-6-luna` | `high`; `low` for mechanical extraction | read-only | bounded search, codebase mapping, extraction, classification |
| builder | `gpt-6.1-sol` | `medium` | workspace-write | coupled implementation and sustained tool workflows; Luna/high for small clear changes with direct checks |
| reviewer | `gpt-6.1-sol` | `high` | read-only | correctness, security, regressions, missing validation; Luna/high for narrow low-risk checks |
| librarian | `gpt-6-luna` | `high` | read-only or candidate-memory-only | deduplication, compression, memory candidates; parent approves storage and sharing |
| expert | `gpt-6-astra` | `low` initially | read-only | hardest unresolved reasoning, conflicting evidence, difficult failures and consequential architecture |

## Routing

- Clear, repetitive, high-volume tasks go to `scout` or `librarian`.
- Small, clear changes with cheap acceptance checks may use Luna/high; coupled changes and sustained tool workflows go to Sol/medium `builder`.
- Difficult review goes to `reviewer` first.
- Ambiguous, conflicting, consequential, or repeatedly failing work goes to `strategist`.
- If the parent already meets the strategist tier, it may plan or arbitrate directly instead of spawning a redundant strategist.

## Model and effort decisions (checked 2026-10-07)

These are routing defaults, not measured performance guarantees. Preserve explicit user choices and local overrides. Check models and supported efforts exposed by the actual runtime before dispatch; API availability does not establish Codex account access.

- Prefer total task cost: context, reasoning, retries, review, and latency. Do not infer subscription credit usage from API token pricing.
- Start from the role's effort; use Sol/high for difficult dependencies and review, and xhigh only for a concrete unresolved issue. Effort labels are not equivalent across generations.
- Improve evidence or the task packet after failure, allow one focused retry, then escalate Luna to Sol. Escalate Sol to Astra for unresolved difficulty, evidence-backed disagreement or consequential uncertainty. Immediate expert routing is appropriate for the hardest work.
- Start Astra at low; raise to medium or high when needed. Do not exhaust repeated weaker-model attempts to save nominal token cost.
- Reserve max for unusually difficult single tasks. Ultra is a runtime multi-agent execution mode, not a universal reasoning value; do not configure it on every agent or Luna.
- 6.1 Sol and Astra do not support none or minimal; use supported runtime values only.
- Keep simple tasks with the parent when handoff and review would cost more. A skill cannot switch the parent model itself; explain a needed model change if delegation cannot provide it.

## Availability and maintenance

- Preferred Sol fallback: 6.1 Sol, then 6 Sol, then 5.6 Sol when actually available.
- Preferred Luna fallback: 6 Luna, then 5.6 Luna when actually available.
- Keep 5.6 Terra as an optional legacy execution/review choice if available and local preferences or observed results favor it. Do not invent a GPT-6 Terra ID.
- If Astra is unavailable, use Sol at a supported higher effort and disclose the missing expert tier. Do not silently downgrade consequential work.
- Existing terra_builder and terra_reviewer names remain compatibility identifiers; their templates now use Sol. Each TOML template pins one model: this profile does not automatically implement fallback in TOML. The parent must route to an available alternative or update a template explicitly.
- Limit concurrency to the runtime's remaining slots even if the profile allows more.
- A stronger model does not justify sharing complete history. The parent still selects relevant context and approves persistent memories.
- On requested upgrades or unavailable-model errors, verify current official documentation and runtime support, update this outer layer and relevant templates, preserve local overrides and record the verification date. No per-task web lookup is needed while the verified mapping works.

Sources: [Codex models](https://learn.chatgpt.com/docs/models), [model selection](https://developers.openai.com/api/docs/guides/model-selection), [GPT-6 guidance](https://developers.openai.com/api/docs/guides/latest-model).

## Escalation triggers

Escalate when any of these is true:

- Requirements admit materially different interpretations.
- Two evidence-backed agents disagree.
- A normal attempt and one focused retry both fail.
- The operation affects security, irreversible data, deployment, money, or external publication.
- The reviewer cannot establish that the definition of done is satisfied.
