# Customization and extensions

AgentFleet Core separates a stable coordination kernel from editable outer profiles.

## Safe customization surface

Customize any of these without changing `SKILL.md`:

- Role-to-model and role-to-reasoning mappings in `default-profile.md`.
- A local override named `team-profile.local.md`.
- Agent definitions copied from `assets/agents/`.
- Concurrency, retry, escalation, and context budgets.
- New specialist roles with a bounded description, permissions, output contract, and escalation path.
- Memory destinations and retention periods, provided the approval boundary remains intact.

## Local override

Create `references/team-profile.local.md` and include only differences from the default profile. The skill reads it after the default profile. Keep the file outside version control when it contains machine-specific paths, private model providers, or organization details.

## Adding a specialist

Define:

1. A unique logical role and when it should be selected.
2. Model and reasoning effort.
3. Read/write and tool boundaries.
4. Expected task and result packets.
5. Conditions that return control to the parent.

An extension may specialize the fleet but must not bypass the stable kernel's parent accountability, permission boundaries, validation gate, minimal-context default, or memory approval gate.

## Updating the project

Upstream changes should normally modify the kernel only for genuine invariant fixes. New models, providers, roles, and personal preferences belong in the outer profile so forks can update without repeatedly resolving core conflicts.

