# skills

Five Claude Code Skills I actually run, published as worked examples. Small on purpose: a shelf to
read and take from, not a framework to install.

A Skill is a folder holding a `SKILL.md` and whatever reference files it needs. Each folder here is
self-contained and nothing depends on anything else here. Copy one into a project's
`.claude/skills/`, or into `~/.claude/skills/` to have it everywhere.

| Skill | What it does | Invocation | Origin |
|---|---|---|---|
| [`interview-design-decisions`](interview-design-decisions/) | Walks the design tree one question at a time, with a recommended answer on each, before you build | model-invoked | adapted from [`mattpocock/skills`](https://github.com/mattpocock/skills) |
| [`diagnosing-bugs`](diagnosing-bugs/) | Refuses to form a hypothesis until a tight, red-capable reproduction exists | user-invoked | [`mattpocock/skills`](https://github.com/mattpocock/skills), lightly adapted |
| [`handoff`](handoff/) | Moves one task to a fresh session, or to a different agent, as a disposable pointer-only doc | user-invoked | original |
| [`writing-great-skills`](writing-great-skills/) | The vocabulary and principles that make a Skill predictable | user-invoked | [`mattpocock/skills`](https://github.com/mattpocock/skills), near-verbatim |
| [`refine-instruction-surface`](refine-instruction-surface/) | Routes each block of an always-loaded instruction file to its cheapest tier, prunes dead weight, and leaves load-bearing restatements alone | user-invoked | original |

**Model-invoked** means Claude can reach for it on its own, so its description sits in the context
window every turn. **User-invoked** means it only runs when you name it: nothing in context, but you
have to remember it exists.

Full attribution and licensing for the adapted Skills: [CREDITS.md](CREDITS.md).

## How these were designed

Four rules, which are most of what separates a maintained Skill from a saved prompt.

- **A prompt earns its packaging.** The need has to recur, the trigger has to be recognizable, the
  procedure has to be reusable, and I have to be able to tell whether it did the job. Advice I have
  written down twice is not yet a Skill.
- **Invocation is a cost, not a default.** Four of these five are user-invoked. A model-invoked Skill
  keeps its description in the context window every single turn, and every Skill that can fire on its
  own widens what the agent might do unasked. Pay that only where the agent genuinely must reach the
  Skill itself.
- **Adopted work is read in full, changed where my practice differs, and recorded.** Three of these
  came from someone else. Sometimes the change is substantial and sometimes it is a single word;
  either way the origin gets written down. The Origin column and [CREDITS.md](CREDITS.md) are that
  record.
- **Observe for drift.** These are prompts, and the arrangement that worked best a few months ago is
  not guaranteed to be what works best now. A Skill that stops helping gets narrowed or deleted, not
  kept out of politeness.

## Taking this further

Fork it. Keep what fits how you work, replace what does not, and delete the rest. The reasoning is
the part worth taking; my particular five are not a standard.

---

Snapshot as of 2026-07-27. These are the public versions, written to be read on their own. The
working copies live in a private library and keep changing, so this repository will drift from them
over time rather than tracking them automatically.
