# Feature testing outline

Living checklist of **which skill behaviors are live-verified**. This is the cross-session, cross-agent tracker (a layer besides git). Behavior lives in [SKILL.md](SKILL.md).

**What the boxes mean.** Each box is one checkable feature. `[x]` = last live pass (install into a harness, or inspect the published files) succeeded. `[ ]` = not yet live-verified, or a later pass failed.

**How to mark (per round).** A round covers only the boxes its changes touch. Leave unrelated boxes as they are. Check a touched box after this round's live pass succeeds. Uncheck a touched box if this round's live pass fails.

**Generic fixture names.** This repo has no media kit. Do not put secrets or personal paths in this file.

## Gates

- [x] `SKILL.md` YAML frontmatter `name` is `elementary-coder`
- [x] Rule 24 names OpenCode (`opencode --version`), Grok (`grok --version`), and MiMoCode (`mimo --version`)
- [x] Rule 24 Plan Model covers OpenCode SQL, Grok session context, and MiMoCode session DB
- [x] MiMoCode DB path is portable (`XDG_DATA_HOME` / `~/.local/share` / `MIMOCODE_DB`)
- [x] Rule 25 mandates a project-root `TESTING.md`
- [x] Rule 25 states boxes are live-verified features (cross-session / cross-agent, besides git)
- [x] Rule 25 per-round marking covers only boxes the code changes touch
- [x] Rule 26 mandates a project-root `SECURITY.md`
- [x] Rule 26: README links to `SECURITY.md`; no real secrets in that file
- [x] This repo has a `SECURITY.md` (skill text only; no runtime credentials)
- [x] README install paths for Grok, OpenCode, and MiMoCode
- [x] LICENSE is Unlicense
- [x] Rule 27 mandates `docs/features/<slug>/<SLUG>.md` and `docs/researches/` (no date-named directories)
- [x] Rule 27: clean audit is `AUDIT-REPORT` with no fix file
- [x] Rule 27: bugs are `BUG-REPORT` then `BUG-FIX` with the same 8-hex unique id
- [x] Rule 27 unique_id is `openssl rand -hex 4`; model slug matches rule 24
- [x] Rule 27 states audit files are not `TESTING.md` boxes
- [x] Rule 27 lets a skill-only repo omit `docs/features/`

## Install

- [ ] Clone into the Grok skills dir; the skill appears
- [ ] Clone into the OpenCode skills dir; the skill appears
- [ ] Clone into the MiMoCode skills dir; the skill appears
- [ ] `/elementary-coder` or an explicit “apply elementary-coder” prompt loads the skill

## Commits (rule 24)

- [x] An agent commit from Grok records `Grok Build v…` from `grok --version`
- [ ] An agent commit from OpenCode records `OpenCode v…` from `opencode --version`
- [ ] An agent commit from MiMoCode records `MiMoCode v…` from `mimo --version`
- [ ] Prompt Summary and Round Summary use the labeled block

## Not features

- A runtime linter that rewrites code on save
- A language-specific formatter (gofmt, prettier, ruff, …)
- A hosted service or binary — this repo is the skill text only
