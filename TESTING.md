# Feature testing outline

Living checklist for this skill. Use it on every verification pass. Behavior lives in [SKILL.md](SKILL.md).

**How to mark.** Check a box when that behavior has been live-verified (install into a harness, or inspect the published files). Uncheck it if a later pass fails.

**Generic fixture names.** This repo has no media kit. Do not put secrets or personal paths in this file.

## Gates

- [x] `SKILL.md` YAML frontmatter `name` is `elementary-coder`
- [x] Rule 24 names OpenCode (`opencode --version`), Grok (`grok --version`), and MiMoCode (`mimo --version`)
- [x] Rule 24 Plan Model covers OpenCode SQL, Grok session context, and MiMoCode session DB
- [x] MiMoCode DB path is portable (`XDG_DATA_HOME` / `~/.local/share` / `MIMOCODE_DB`)
- [x] Rule 25 mandates a project-root `TESTING.md`
- [x] README install paths for Grok, OpenCode, and MiMoCode
- [x] LICENSE is Unlicense

## Install

- [ ] Clone into the Grok skills dir; the skill appears
- [ ] Clone into the OpenCode skills dir; the skill appears
- [ ] Clone into the MiMoCode skills dir; the skill appears
- [ ] `/elementary-coder` or an explicit “apply elementary-coder” prompt loads the skill

## Commits (rule 24)

- [ ] An agent commit from Grok records `Grok Build v…` from `grok --version`
- [ ] An agent commit from OpenCode records `OpenCode v…` from `opencode --version`
- [ ] An agent commit from MiMoCode records `MiMoCode v…` from `mimo --version`
- [ ] Prompt Summary and Round Summary use the labeled block

## Not features

- A runtime linter that rewrites code on save
- A language-specific formatter (gofmt, prettier, ruff, …)
- A hosted service or binary — this repo is the skill text only
