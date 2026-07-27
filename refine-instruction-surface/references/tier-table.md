# Tier table — where instruction content belongs

The test for every block: **does it earn always-on?** Always-on is the most expensive tier; a block
earns it only when it must shape *every* session (a durable fact, a standing policy, a default
posture). Everything else routes down.

| If the block is… | Route it to… | Cost profile |
|---|---|---|
| A durable fact or standing policy every session needs | the tool-neutral contract (`AGENTS.md`) | always-on, every tool |
| Tool-specific operating rules (one agent product only) | that tool's adapter (`CLAUDE.md`, `.codex/`, …) | always-on, one tool |
| Personal identity or preference (applies to any project) | the user-level file (`~/.claude/CLAUDE.md`) | always-on, one user |
| A repeatable procedure, broadly useful | a skill (auto- or user-invoked) | on-demand |
| A repeatable procedure, occasional / domain-specific / needing isolation | a dispatched sub-agent brief | zero until dispatched |
| Bulk reference (tables, flag lists, enumerations) | an on-demand doc, linked not inlined | zero until read |
| A rule that must *always* happen | a hook or CI gate (prose alone cannot enforce) | enforced |
| Not yet corroborated | a dated watch note | zero |

Rules of thumb:

- **Adapter leakage** — tool-specific rules in the tool-neutral contract (or contract-worthy policy
  trapped in one tool's adapter) — is a routing defect even when only one tool is in use today.
- When a block routes out, its **inoculations travel with it**: a point-of-action restatement moves
  to the new home's point of action; it is not deleted.
- Routing beats compression: if a block fails the always-on test, moving it is the fix; shortening it
  in place only shrinks the defect.
