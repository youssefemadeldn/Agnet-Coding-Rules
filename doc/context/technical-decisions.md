# Technical decisions

Decisions about how this rules repo is built, and why. Newest facts win; check the rule files themselves before trusting a line here.

## What the repo is

- A hub of AI-agent coding rules: one self-governed folder per stack (`.net/`, `flutter/`) and a thin root `CLAUDE.md` that routes between them.
- This shape is the same one Youssef's real repos use (backend + mobile, sometimes + frontend, in one repo, each with its own `CLAUDE.md` + `.claude/rules/` + `.claude/templates/`). So this repo is also the template for that pattern.

## The quality bar every rule encodes

Rules are written to make agent output meet Youssef's 9 quality pillars:

1. **Reliability** — real error handling, no crash on unexpected input, log on failure.
2. **Security** — no vulnerabilities, no exposed secrets, every input validated.
3. **Modularity / separation of concerns** — SOLID, one job per unit.
4. **Consistency** — one style and naming scheme across the project.
5. **DRY** — no duplicated logic.
6. **Documentation** — comments only where needed, a clear README for larger projects.
7. **Performance** — no wasted resources.
8. **Version control** — clear commits, an understandable branching strategy.
9. **One pattern** — one architectural approach throughout, never mixed styles.

## Decisions

| Date | Decision | Why | Rejected |
|---|---|---|---|
| 2026-07-22 | `.net/` rules are reusable for any .NET backend, not tied to one project. The VetLink backend is only the reference they were derived from (see the `<Solution>` placeholder in `.net/CLAUDE.md`). | One rule set for every future backend. | VetLink-only rules. |
| 2026-07-22 | Where the reference project was inconsistent, the rules define a clean target state instead of copying today's mix. | Rules should raise the bar, not freeze tech debt. | Codifying the current inconsistency. |
| 2026-07-22 | `.net/` mirrors the Flutter structure (master `CLAUDE.md` + scaffold prompt + feature prompt + governed templates) but its content is derived independently from .NET evidence. | Same mentality, no cross-stack rule bleed. | Porting Flutter content. |
| 2026-07-22 | `.net/.claude/rules/dotnet_feature_prompt.md` has its own 7 Non-Negotiable Rules, numbered separately from the 7 Architecture rules in `.net/CLAUDE.md`. Always say which file a rule number belongs to. | Same topics, different split; a bare "rule 3" is ambiguous. | One shared numbering. |
| 2026-07-24 | `flutter/` is a byte-identical mirror of the live books-platform mobile rules (`CLAUDE.md` + `.claude/rules/` + `.claude/templates/`). The originals stay in books-platform. | Keep both stacks' rules in one hub without moving the live copy. Still identical on 2026-10-23. | Moving the files here. |
| 2026-07-24 | Root `CLAUDE.md` is a thin router (projects table + cross-project rules), not an inline merge of both rule sets. | Root `CLAUDE.md` loads in every session; the two stack files are large (~345 and ~318 lines) and mostly unrelated, so inlining doubles the fixed cost and risks rule bleed. | Inlining both. |
| 2026-07-24 | The router does not tell Claude to "read the project's `CLAUDE.md` first". | Claude Code already loads a subfolder's `CLAUDE.md` and `.claude/rules/` as soon as any file in that folder is read (official docs: code.claude.com/docs/en/memory and /large-codebases; Anthropic's own monorepo example uses this shape). | A redundant routing instruction. |
| 2026-07-24 | The "Team mindset" and "Completion checklist" overlap between `.net/CLAUDE.md` and `flutter/CLAUDE.md` stays duplicated; no shared file. | Youssef's choice; a shared file is over-engineering for two stacks. | Extracting a shared meta file. |
| 2026-07-24 | Adding a stack (e.g. frontend) = new top-level folder with its own `CLAUDE.md` + `.claude/rules/` + `.claude/templates/` + one row in the root table. Nothing else changes. | Keeps every stack self-contained. | — |
