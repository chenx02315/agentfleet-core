# Memory policy

Use memory to prevent repeated work without turning raw history into permanent context.

## Scopes

- `run`: temporary state for the current task.
- `project`: validated facts and decisions relevant to one project.
- `global`: stable preferences or procedures useful across projects.
- `private`: material intentionally restricted to one agent or role.

## Admission

A worker may only propose a memory candidate. The parent approves, edits, scopes, rejects, or defers it.

Promote a candidate only when it is reusable, evidence-backed, non-secret, and more valuable than its future context cost. Keep failed attempts in run history unless they establish a verified lesson.

Each persistent memory should contain:

```yaml
type: fact | decision | lesson | procedure | preference
scope: project | global | private
summary: short reusable statement
evidence: source or validation reference
confidence: low | medium | high
owner: approving agent or user
created_at: date
expires_at: optional date
supersedes: optional prior memory id
```

## Retrieval

Retrieve summaries first and full payloads only on demand. Prefer a few directly relevant memories over broad history. When facts conflict, retain provenance and mark which fact supersedes the other instead of silently overwriting history.

## Boundaries

- Never store credentials, access tokens, private keys, or unnecessary personal data.
- Never store hidden reasoning or chain-of-thought.
- Do not write outside the authorized workspace or to a shared external memory system without the required authorization.
- Convert stable repeatable procedures into a skill or project guidance rather than repeatedly injecting long episodic logs.

