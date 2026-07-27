# The round-trip (DIY sub-agent) and cross-agent challenge review

Two patterns that use a handoff as more than a one-way pointer. Both keep the parent session's budget
clean while borrowing a *separate* budget for the hard part.

## 1 · DIY sub-agent — hand off a question, hand back a verdict

When you're planning or interviewing a method, two kinds of question come up:

- **Known-unknowns** the agent can just ask you ("which input? which seed?").
- **Can't-answer-from-the-armchair** — you have to *see it in code or measured numbers* first (a
  prototype, a profiling probe, a convergence/perf shakedown). The answer doesn't exist until something
  runs.

The move: from the planning session, **hand the hard-to-judge bit off to a prototype session**. That
session may balloon far past what would have fit in the parent — fine, it's a *separate* budget. It does
the disposable work, learns the verdict, and **hands a compressed document back**:

> "Take everything non-obvious from this prototype — anything not already obvious in the committed
> artifact — and give me a doc I can paste back into the planner."

You've built a **DIY sub-agent**: a dedicated context window for one task, its learnings compressed and
returned to the parent, without a real sub-agent harness and without polluting the parent's budget.

**Two rules for the return doc:**

1. **Stamp the verdict with its conditions** (machine, build, input size). A verdict that drops its
   conditions is compaction's wrong-by-omission in disguise.
2. **Return a small re-runnable artifact plus a one-paragraph verdict**, not a notebook of vibes. The
   artifact stays behind in the prototype window; the verdict is what survives being pasted into a
   fresh window.

Round-trip shape:

```
planner  --handoff(question + pointers)-->  prototype session (disposable, own budget)
planner  <--handoff(verdict + conditions + pointer to the artifact)--  prototype session
```

### Return-doc template

```markdown
# Handoff-back — <question that was answered>
**Verdict:** <one paragraph: the answer + the number that decided it>.
**Holds under:** <machine / build / versions / input size>.
**Artifact (stays in the prototype):** <path to the re-runnable probe / RESULTS.md>.
**Non-obvious learnings (not already in the committed code):**
- <thing the planner can't see from the artifact alone>
**Discard after promoting** any durable fact into CONTEXT.md.
```

## 2 · Cross-agent challenge review

A handoff is plain markdown carrying no harness-native state, so it crosses **agent** boundaries: the
agent that *wrote* it and the agent that *reads* it can be different CLIs or different models.
Compaction can't do this — it's a feature of the harness you're in. A markdown pointer doc pastes
anywhere.

The high-value use is an **independent challenge review**. Write the design/result with one agent, then
hand the markdown to a *different* agent and ask it to verify the reasoning from first principles:

- Re-derive the numbers; look for a silently-wrong result (the plausible-looking value that's off by
  a factor nothing crashes on).
- Aim it at the exact places where "looks done" and "is correct" diverge — a backend that's
  *documented as deferred* but reads as *implemented*; a unit conversion that a suite sharing the
  author's assumption would never catch.

Independence is the whole point: a second agent that doesn't inherit the first's assumptions is the
closest automated analogue to a fresh human reviewer. Give it a pointed question, not "review this":

```markdown
# Handoff (independent challenge) — verify the <X> reasoning
Do not assume the conclusion is correct. Specifically check:
- <the one conversion / boundary / claim most likely to be silently wrong>
- <where "implemented" might actually be "deferred, identical to the fallback">
Return: the first thing that doesn't hold, with the pointer to where it breaks — or a signed-off
"verified these N things independently; all hold; here's how I checked."
```

Portability turns the handoff from a continuity tool into a **review tool**: the same disposable
markdown that moves a task forward can, with a different reader and an independent challenge prompt, move it
*under scrutiny*.
