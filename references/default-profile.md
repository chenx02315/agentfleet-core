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
| strategist | `gpt-5.6-sol` | `high` | read-only unless implementation is explicitly assigned | ambiguity, architecture, planning, arbitration, final high-risk review |
| scout | `gpt-5.6-luna` | `low` | read-only | search, codebase mapping, extraction, classification, bulk summaries |
| builder | `gpt-5.6-terra` | `medium` | workspace-write | ordinary implementation, documentation, tests, tool-driven work |
| reviewer | `gpt-5.6-terra` | `high` | read-only | correctness, security, edge cases, missing tests |
| librarian | `gpt-5.6-luna` | `medium` | read-only or candidate-memory-only | deduplication, compression, memory candidate preparation |

## Routing

- Clear, repetitive, high-volume tasks go to `scout` or `librarian`.
- Everyday implementation and tool use go to `builder`.
- Difficult review goes to `reviewer` first.
- Ambiguous, conflicting, consequential, or repeatedly failing work goes to `strategist`.
- If the parent already meets the strategist tier, it may plan or arbitrate directly instead of spawning a redundant strategist.

## Escalation triggers

Escalate when any of these is true:

- Requirements admit materially different interpretations.
- Two evidence-backed agents disagree.
- A normal attempt and one focused retry both fail.
- The operation affects security, irreversible data, deployment, money, or external publication.
- The reviewer cannot establish that the definition of done is satisfied.

