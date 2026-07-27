# Handoff doc — template, worked example, redaction checklist

The **dynamic** handoff: a disposable markdown doc that carries **one task's** context across a session
or agent boundary. Lives in the OS temp dir (`${TMPDIR:-/tmp}/handoff-<slug>.md`, or `mktemp
"${TMPDIR:-/tmp}/handoff-<slug>.XXXXXX.md"` to avoid a predictable name in a shared temp dir), **not**
the repo. Topic-neutral — fill it for a bug-fix, a refactor, a prototype probe, or an independent challenge review.

## Template

```markdown
# Handoff — <one task, named tightly>
**Done when:** <one sentence acceptance test>.
**Written:** <date> · **Disposable:** temp dir, promote-then-discard (not documentation).

## Context — pointers, not copies
Reference the addressable source; never paste its contents (a copy rots the instant it's written).
- <issue #/ URL> — <why it matters, one line>
- <commit sha> — <what it did / what's deferred in it>
- <path:line or artifact> — <the exact thing to read there>

## Conditions — stamp every measured claim
Any number or verdict carries the context it holds under, or it's a half-truth.
- <hardware/runtime CLASS (e.g. "M1 Mac 16GB, py3.12") — not a resolvable internal hostname — build, versions, input size, seed>

## Do — the fresh agent's first moves
1. <concrete first action>
2. <second>
3. <how to prove it's done — tie back to "Done when">

## Suggested skills — invoke on arrival
Teleport the parent session's *discipline*, not just its facts.
- `<skill>` — <the mode to inherit and why>

## Redacted / kept in the parent
- <secrets/paths deliberately omitted>
- <what the session that wrote this is still doing — so the receiver stays in its lane>
```

## Worked example (a bug spun off mid-feature)

You're implementing a CSV export endpoint and notice, out of scope, that session tokens are written to
the request log in plaintext. You don't fix it here — you hand it off so the current feature stays pure.

```markdown
# Handoff — redact session tokens from request logs
**Done when:** no session token (or other credential) appears in any log sink; a test asserts a
redacted placeholder and is RED before the fix.
**Written:** 2026-07-02 · **Disposable:** temp dir, promote-then-discard.

## Context — pointers, not copies
- issue #412 — "tokens in plaintext logs" (filed when spotted; has the sample log line).
- src/middleware/access_log.py:44 — the formatter that interpolates the raw header.
- CONTEXT.md#logging — the project's existing redaction convention (reuse it, don't invent one).

## Conditions
- Reproduced on main @ <sha>, Python 3.12, default log config (JSON sink).

## Do
1. Write the RED test first: drive a request carrying a token, assert the sink shows `***` not the token.
2. Confirm it FAILS against current behavior before touching the formatter.
3. Route the token header through the existing redaction helper; make the test green.

## Suggested skills — invoke on arrival
- `tdd` — red-first; the test must fail before the fix, or it pins the bug instead of catching it.

## Redacted / kept in the parent
- The actual captured token from the sample log is NOT in this doc — see issue #412 (access-controlled).
- The parent session is finishing the CSV export endpoint; the logging fix is fully separate.
```

Note what makes it a *handoff* and not a copy: every fact is a pointer (issue, file:line, CONTEXT
anchor), the one real secret is redacted and referenced instead of pasted, the verdict carries its
conditions, and the suggested-skills line makes the next window drive the fix red-first without being
told again.

## Redaction checklist (run before you save the file)

A handoff is a throwaway markdown file in a world-readable temp dir. Strip:

- [ ] **Credentials / tokens / keys** — API keys, session tokens, passwords, connection strings.
- [ ] **Private absolute paths** that leak a machine's layout or a username, when a repo-relative
      pointer or an issue reference would do.
- [ ] **Account / infra specifics** — cluster allocation accounts, login-node names, internal hostnames.
- [ ] **Anything you'd not want in a pasted-anywhere doc** — a handoff is portable *by design*; assume
      it may land in a different agent's context.

Redaction is deliberate loss *you* control — the inverse of compaction's silent omission. When in
doubt, **point at the access-controlled source** instead of inlining the sensitive value.

## The disposal rule

The handoff exists to move one task across one boundary. When the task lands:

1. **Promote** anything durably true (a convention, a units trap, a decided-against option) into
   `CONTEXT.md` / `AGENTS.md`.
2. **Delete** the temp file.

Never commit a handoff to the repo as if it were documentation — a durable copy of a disposable
pointer is the bloat this skill exists to avoid.
