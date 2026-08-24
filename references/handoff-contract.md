# Handoff contract

Use a compact task packet instead of forwarding the entire parent conversation.

```yaml
task_id: stable-short-id
role: scout | builder | reviewer | strategist | librarian
goal: one bounded outcome
definition_of_done:
  - observable acceptance check
constraints:
  - scope, compatibility, safety, or style constraint
allowed_tools:
  - only tools needed for this task
write_scope:
  - explicit files or "read-only"
context:
  requirements: selected requirements
  evidence: relevant files, links, or facts
  memory: selected approved memories only
output:
  format: concise structured summary
  include:
    - result
    - evidence or changed artifacts
    - validation performed
    - unresolved risks
escalate_when:
  - condition that requires parent judgment
```

Require this result shape:

```yaml
status: complete | partial | blocked
result: concise outcome
evidence: files, commands, links, or observations
validation: checks performed and outcomes
memory_candidates: reusable facts or procedures, if any
risks: unresolved uncertainty or side effects
```

Do not request private chain-of-thought. Request decisions, evidence, assumptions, and observable validation.

