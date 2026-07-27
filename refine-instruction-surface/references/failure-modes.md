# Prune catalogue — failure modes of an instruction surface

One definition, the tell, and the fix per mode. (For `SKILL.md`-file craft, `writing-great-skills`
holds the extended treatment; the shared terms are compatible.)

## Duplication

The same meaning at more than one place in the **loaded set** (within one file, or across the import
chain). Tell: a rule you can quote from two places that load together. Fix: pick the single source of
truth (usually the tool-neutral contract), reduce the other copies to nothing or a pointer.
**Carve-out:** an inoculation (below) is not duplication.

## Inoculation (protected — do not prune)

A deliberate restatement of a hard rule at its point of action ("`--dry-run` first" inside the
cleanup step; "never force-push" inside the release step). It exists because the agent acts where it
reads. Tell: the restatement sits adjacent to the risky action it guards. Fix: none — inventory it in
step 2 and keep it; if its enclosing block routes elsewhere, it travels along.

## No-op

A line the model already obeys by default ("write clean, high-quality code", "be careful", "think
step by step"). Pays rent, changes nothing. Tell: delete it in your head — would any session behave
differently? Fix: delete the whole sentence, don't trim it. If a real requirement hides inside, state
that requirement precisely instead.

## Sediment

A stale layer that outlived its referent — the note about a workflow that was removed, the exception
to a rule that no longer exists. Tell: it explains or cancels something you cannot find. Fix: delete.
**Not sediment:** a dated, scoped record that still binds (a version pin awaiting an upstream fix, an
audit block kept as the record) — dated ≠ stale; check whether the referent is live before cutting.

## Dangling reference

A path or link in the loaded set that no longer resolves. Tell: resolve every reference mechanically —
never by eye. Fix: re-point, restore the target, or delete the claim that depended on it.

## Machine-absolute path

An `@import` or path that only resolves on one machine (`/Users/<name>/…`, `/home/<name>/…`). Dangles
silently everywhere else. Tell: any absolute path with a username segment. Fix: home-relative
canonical path (`~/…`), repo-relative path, or vendor the content in.

## Perishable baseline

A measured claim pinned to one machine or date and presented as a standing threshold ("the suite
takes 41 s on the lab iMac; treat >60 s as a regression"). Rots without announcing it. Tell: a
number + a machine or date + a rule derived from it. Fix: scope and date it, move it to a dated doc,
or replace it with a condition-free invariant.

## Procedure-in-contract

Step-by-step process text living in an always-loaded file: every session pays for a workflow almost
no session runs. Tell: numbered steps, imperative chains. Fix: route per the tier table (skill if
broadly useful, agent brief if occasional or domain-bound) and leave at most a one-line pointer.

## Wrong-tier bulk

Reference material (flag tables, naming enumerations, long lists) inlined always-on but needed only
in occasional task types. Tell: a table the median session never reads. Fix: an on-demand doc, or the
dispatched agent that owns the task; link, don't inline.

## Adapter leakage

Tool-specific rules in the tool-neutral contract, or contract-worthy policy trapped in one tool's
adapter. Tell: one product's features named inside `AGENTS.md` (skills, Task tool, slash commands).
Fix: move it across to the right file; don't duplicate it.

## Personal-in-project

User identity or preference in a shared project file (names, response-format tastes). Binds every
collaborator to one person's preferences. Tell: first person singular. Fix: route to the user-level
file.
