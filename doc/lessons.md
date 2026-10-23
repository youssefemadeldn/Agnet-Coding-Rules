# Lessons

Gotchas from working on this repo. One line each: what happened, what to do.

- **Review bar: flag functional slips, not cosmetic ones.** Flag wrong casing in literal code (e.g. a `WebApi`/`WebAPI` namespace slip), dangling cross-references and wrong facts. Don't flag a looser placeholder in a high-level map than in a detailed template, the spelling of an already-fictional typo example, or "rule 1" vs "rules #1" wording. (2026-07-22: three such fixes were reverted by Youssef; don't re-apply them: `<Feature>Dto.cs` vs `<Entity>DetailsDto.cs` in the Services diagram, `Piority` vs `Pirority`, "Architecture rule 1" vs "Architecture rules #1".)
- **Check Claude Code mechanics against the docs before claiming them.** How `CLAUDE.md` and rules load changes over time; read code.claude.com/docs before asserting it (an overclaim was caught here on 2026-07-24).
- **Before editing `flutter/`, remember it mirrors the live books-platform mobile rules.** A change made only here makes the two copies drift; say which copy is the source of truth for the change.
