# Working Preferences

How to work with me on any project, whatever the stack. These are workflow rules, not code style. Each project's own `CLAUDE.md` still governs its code. Where a project states something more specific (for example its deploy mechanics), the project wins.

## 1. Clarify with AskUserQuestion, at every stage

Use the **AskUserQuestion tool** for anything unclear, never a guess or a plain-text question buried in a reply.

1. **Before plan mode:** collect the scope and product decisions first.
2. **During plan mode:** ask again when exploring the code exposes a new choice.
3. **During implementation:** if something unexpected comes up, stop and ask. Don't silently pick.

Put the recommended option first and give the reason. Record every answer in the handoff's "Clarifications & Decisions" table, and in the plan doc when it's a product decision.

## 2. Enter plan mode yourself when the task deserves it

At each step boundary, decide whether the task needs plan mode. If it does, say why in one line and **enter plan mode yourself** (EnterPlanMode). I approve the switch. Never start editing a plan-worthy task outside plan mode.

| Needs plan mode | Goes straight ahead |
|---|---|
| Schema migrations, table drops | One-file fixes |
| API contract changes across backend + clients | Doc updates |
| Multi-file rewrites, new seeders | Scripts that only write review files |
| Any write to production data | Anything small, local and easy to undo |
| Changes across a project boundary (e.g. AI ↔ backend DB) | |

Order: AskUserQuestion → plan mode (ask more there) → implement (ask if surprised).

## 3. Git: branches and commits

- **Branch per change, never commit on `main`:** `<type>/<short-kebab-topic>`, type = `feat` | `fix` | `chore`. Add the plan item id when there is one: `fix/m3-whole-word-matching`, `feat/web-fridge-load-more`.
- **Commit subject:** a conventional commit with the project as scope, in plain-English outcome terms, plus the plan id:
  `fix(backend): match typed ingredients as whole words (fix plan M3)`
  Several projects → `fix(backend,web): …`. Docs → `docs(<area>): …`. Ops → `chore(deploy): …`.
- **Commit body:** why it was wrong, what the change does, and the **proof**: real before/after numbers (ideally from a copy of production) and the test count (`728/728 backend tests`).
- **Merge into main:** a `--no-ff` merge commit titled `Merge <branch>: <one-line outcome>`.
- **Before merging:** pull `origin/main` and resolve, then build and test.
- **Pushing:** pushing a branch is always safe. Merge and push `main` only on my explicit go-ahead, because in my projects **pushing `main` usually deploys to production** (webhooks redeploy and run migrations). If deploy order matters (backend before its consumers), push in two steps.
- Doc- or handoff-only commits ride along with the next push instead of triggering a deploy alone.
- Merged branches can be deleted later. Ask first.

## 4. Keep docs and memory current

Documentation is part of finishing a step, not an extra:

- **Plan of record** (the fix/feature plan doc): status per step, new requirements, answered decisions.
- **Technical-decision docs** (e.g. `docs/context/*-technical-decisions.md`, deployment decisions): what was chosen and why, new env vars and one-shot flags.
- **`CLAUDE.md`** for repo-wide rules. **`CLAUDE.local.md`** (git-ignored) for machine-only facts such as where secrets live, never the secrets themselves.
- **Memory** for what the next session must know: decisions, my preferences, production state, backup locations.
- **Session handoff** (`/session-handoff`) at the end of a session, saved to `docs/handoffs/<topic-folder>/NNN-YYYY-MM-DD-<slug>.md` with the next free number. It holds: what was done (with commits), bugs, changed files, remaining tasks, the Clarifications & Decisions table, and local setup notes.
- **Never** put passwords, connection strings or tokens in any doc, commit or chat.
- **Client-facing docs** never show folder paths or internal details. Write a separate, customised version for the client.

## 5. Back up before any important production operation

1. **Backup:** a full `pg_dump -Fc` (or the stack's equivalent) into a folder outside the repo, chmod 600, with a timestamped name saying what it's before: `<db>-<YYYYMMDD-HHMMSS>-pre-<operation>.dump`. Keep a `LATEST` pointer. Backups hold user data, so never put them in the repo or chat.
2. **Prove the backup:** test-restore it into a scratch database and compare row counts with production.
3. **Rehearse:** run the migration or data change on that restore first. Check the results, and check that a second run changes nothing.
4. **Run it:** data changes ship as **flag-gated, idempotent seeders**, not ad-hoc SQL. I turn the flag on, then you verify on production **read-only**, then I turn the flag back off.
5. **After big loads:** take a post-operation backup too.
6. Name the undo point (which backup file) in the handoff and memory.

Production database access is **read-only by default**. Any write (DDL, grants, data) needs my explicit go-ahead, plus a backup first for data writes.

## 6. Smaller standing preferences

- **Mobile release builds** (AAB/IPA, store uploads): I do them myself. Don't offer or run them. Just say that a change reaches users with the next build.
- **Audit before claiming:** check the real code or data before telling me what's wrong.
- **Settings on the hosting side** (env vars, flags in the deploy dashboard): I change them. Tell me exactly what to set, and confirm afterwards.
