---
name: handoff
description: Move one task's context to a fresh session or a different agent as a disposable, pointer-only temp-dir doc — instead of compacting or dragging the whole session history along.
disable-model-invocation: true
---

# Handoff

## Overview

A **handoff** is a small markdown doc capturing *only* the context a fresh agent needs to continue
**one specific task** — written to the OS temp dir, pasted into a new session (or a different agent),
then disposed of. Its opposite is **compaction** (the harness summarizing the whole session to keep
going). The trade: **compaction buys continuity; a handoff buys purity.** Compaction keeps one thread
alive but is lossy by *silent omission*; a handoff splits one thread into clean independent ones, lossy
only by *redaction you control*. When you notice out-of-scope work, extending the session dilutes it and
compacting clobbers it — the handoff is the third option that keeps the current session pure. Writing it
also **sharpens the current session**: naming a slice "out of scope, picked up elsewhere" collapses it
out of the live thread.

## When to use

- You spot an **out-of-scope** defect / refactor / prototype mid-task and want to spin it off cleanly.
- You're nearing the context **dumb-zone** and want to *split* rather than compact.
- You want to hand **one slice** to a fresh session, or to a **different agent** (different CLI/model) —
  e.g. for an independent challenge review of your own reasoning.
- Round-trip: a throwaway prototype/probe answers a hard-to-judge question and hands a compressed
  **verdict** back (the DIY sub-agent — see `references/round-trip-diy-subagent.md`).

**Not for:** durable documentation (a handoff is disposable — *promote* any durable fact into
`CONTEXT.md`/`AGENTS.md` and discard the doc); or bootstrapping a repo from a static read-only
snapshot, which is the opposite, *durable read-only* meaning of the word and a separate job.

**On compaction.** Barreling at one continuous problem is not a reason to write a handoff: there is no
slice to split off yet. It is *not* an argument for letting automatic compaction run either. Compaction
is lossy by silent omission — a summary you did not write, cannot audit, and did not choose the
redactions for — so **nothing durable should depend on it**. When a continuous thread outgrows its
window, the move is a deliberate split: write the state down and start clean, rather than letting the
harness decide what to forget.

## Method — do in order

1. **Name the one task.** State the single slice and, in one line, what "done" looks like. Tempted to
   name two? Write two handoffs.
2. **Write the doc to the OS temp dir** — `${TMPDIR:-/tmp}/handoff-<slug>.md` (or
   `mktemp "${TMPDIR:-/tmp}/handoff-<slug>.XXXXXX.md"`), **not** the repo. Fill from
   `references/handoff-template.md`. While filling:
   - **Point, don't copy.** Reference the issue / commit / `file:line`; never paste contents. A pointer
     stays correct as its target evolves; a copy rots the instant it's written.
   - **Redact.** Strip credentials, tokens, private absolute paths, account details — lossy *by your
     deliberate choice*. When in doubt, point at the access-controlled source.
   - **Stamp conditions on any measured verdict** (machine class, build, versions, input size). A
     verdict that drops its conditions is compaction's wrong-by-omission in a nicer coat.
   - **Add a suggested-skills / invoke-on-arrival section.** Name the working mode the receiver should
     adopt on arrival, so pasting the doc into a blank window auto-selects the right discipline. This
     teleports the *discipline* of the parent session, not just its facts.
3. **Hand it off.** Open a fresh session/agent and paste or point it at the doc. For a review, hand the
   same markdown to a *different* agent told to verify assumptions independently and look for unsupported
   conclusions (see the reference).
4. **Dispose — promote before you discard.** When the task lands, pull any durably-true fact into
   `CONTEXT.md`/`AGENTS.md`, then delete the temp file. The handoff is the courier, not the archive.

## Output shape (the handoff doc)

Full template + a worked example + the redaction checklist: `references/handoff-template.md`. Skeleton:

```markdown
# Handoff — <one task>
**Done when:** <one sentence>.   **Written:** <date> · disposable (temp dir, not the repo).

## Context — pointers, not copies
- <issue / commit / file:line> — <why it matters, one line>

## Conditions — for any measured claim
- <machine class / build / versions / input size>

## Do
1. <first concrete action for the fresh agent>

## Suggested skills — invoke on arrival
- <the discipline/mode to inherit, and why>

## Redacted / kept in the parent
- <what you deliberately left out; what the parent session keeps doing>
```

## Common mistakes

- **Copying instead of pointing** → the doc bloats and rots. Point at the addressable source.
- **Leaving it in the repo** as if it were documentation. It's sprint-lived; promote-then-discard.
- **Dropping the conditions** on a measured verdict → the receiver proceeds on a half-truth.
- **Using it for continuity** when there is no slice to split — a handoff moves *one task*, not a whole
  thread. If the thread itself has outgrown its window, split it deliberately; don't fall back on
  automatic compaction.
- **Two tasks in one handoff** → diluted context on the receiving end. One slice per doc.
- **Leaking secrets** into a throwaway temp file. Redact; point at the access-controlled source.

## References

- `references/handoff-template.md` — the fill-in doc, a worked example, and the redaction checklist.
- `references/round-trip-diy-subagent.md` — the prototype round-trip (DIY sub-agent) and the
  cross-agent challenge-review pattern (a handoff is plain markdown, so it crosses *agent* boundaries,
  not just session boundaries).
