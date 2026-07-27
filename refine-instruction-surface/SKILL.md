---
name: refine-instruction-surface
description: Trim and refine always-loaded instruction files (CLAUDE.md, AGENTS.md, tool adapters) — route each block to its cheapest tier, prune dead weight, never touch protected sites.
disable-model-invocation: true
---

# Refine instruction surface

## Overview

An instruction file (`CLAUDE.md`, `AGENTS.md`, a tool adapter) is an **always-loaded surface**: every
line is paid for in every session, whether or not it bears on the task. Refining one is two moves plus
a guardrail:

- **Route** — move each block to the *cheapest tier that still does its job* (stay in the contract, or
  move out to a skill, a dispatched agent, a hook, an on-demand doc, a user-level file, an adapter).
- **Prune** — delete lines that pay rent and change nothing (duplication, no-ops, sediment, rotted
  references).
- **Protect** — some restatements are load-bearing. Inventory them first; the pass may not touch them.

The unit of analysis is the **loaded set** — everything that arrives in context together (the file,
its `@imports`, co-loaded user/project/adapter files) — never a single file in isolation. Duplication
only exists relative to the loaded set; a "duplicate" on a standalone load path is the only copy.

## When to use

- A `CLAUDE.md`/`AGENTS.md` has accreted past its role — sessions start with material nobody routed.
- Symptoms of surface bloat: the agent reaches for ceremony or tools the task didn't need, follows
  stale rules, or misses hard rules buried mid-file.
- After merging or importing instruction packs, or vendoring another repo's contract.
- Periodic hygiene, same rhythm as a dependency audit.

**Not for:** `SKILL.md` files (that's `writing-great-skills`); writing a contract from scratch;
one-line additions.

## Method — do in order

1. **Map the loaded set.** Name the file's tier (user file / project contract / tool adapter) and its
   loader; walk every `@import` (flag machine-absolute paths while there); list co-loaded siblings.
   Done when: every always-loaded file is listed with what loads it — this list is the corpus the
   rest of the pass judges.
2. **Inventory protected sites — before proposing any cut.** Hard rules (safety, attribution,
   licensing, data-loss guards), **inoculations** (a rule deliberately restated at its point of
   action), dated-but-live records (a scoped pin or audit note that still binds), and anything a
   hook, eval, or CI gate depends on. Write the list down. **Inoculation beats deduplication** — a
   naive pass deletes exactly the restatement doing the most work.
3. **Route every block** through the tier table (`references/tier-table.md`). For each block ask:
   does this earn always-on? If not, name its destination — don't just shorten it. A procedure
   squeezed to half length is still a procedure in a contract.
4. **Prune what remains** with the catalogue (`references/failure-modes.md`): duplication across the
   loaded set, no-ops, sediment, dangling references, machine-absolute paths, perishable baselines.
   One finding per defect, evidence-quoted. Resolve every reference mechanically, never by eye.
5. **Report, verify, then apply.** Findings table first (shape below) — diagnosis and application are
   separate steps. For judgment-heavy cuts, adversarially verify (refute-by-default: a finding
   survives only if the quote is real, the term fits, and no protected site is touched). Apply
   lowest-risk first; afterwards re-check that every kept reference resolves and each
   behaviour-bearing rule has exactly one home plus its inventoried inoculation sites.

## Output shape

| where | term | evidence (quote) | fix (destination if routed) | risk |
|---|---|---|---|---|
| `CLAUDE.md:31` | procedure-in-contract | "Run at the start of each month: 1. …" | route → skill; leave a one-line pointer | none |

End the report with the protected-site inventory and the loaded-set map, so the applier inherits both.

## Common mistakes

- **Pruning an inoculation as "duplication".** The point-of-action restatement guards the destructive
  step; deleting it is the costliest mistake this pass can make. Step 2 exists to prevent it.
- **Judging one file alone.** Misses import-chain duplication, and falsely flags cross-file copies
  that are each the only copy on their load path.
- **Shortening instead of routing.** Compression is not the fix for wrong-tier content.
- **Reading dated as stale.** A dated, scoped, still-binding record (a pin awaiting an upstream fix)
  is not sediment; sediment is the layer that outlived its referent.
- **Rewriting when asked to diagnose.** Produce findings; apply only after verification (step 5).

## References

- `references/tier-table.md` — the routing destinations and the always-on test.
- `references/failure-modes.md` — the prune catalogue: one definition, tell, and fix per mode.
- For `SKILL.md`-specific craft (descriptions, leading words, disclosure), the sibling skill
  `writing-great-skills` is the authority; the vocabulary shared with it is defined compactly in the
  catalogue here so this skill stands alone.
