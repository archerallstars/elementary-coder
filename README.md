# Elementary Coder

A coding-agent skill. It asks the model to write readable, self-documenting code: full names, living comments, explicit structure, a project-root `TESTING.md` so later agents can see what is proven, a project-root `SECURITY.md` so the threat model is written down, and living `docs/features/` notes plus citation-stable audit files so other models can review the same work.

Works with **Grok**, **OpenCode**, and **MiMoCode**. Copy `SKILL.md` into the skills directory for the harness you use.

## Install

The skill directory must be named `elementary-coder` and contain `SKILL.md`.

**Grok**

```bash
git clone https://github.com/archerallstars/elementary-coder.git ~/.grok/skills/elementary-coder
```

**OpenCode**

```bash
git clone https://github.com/archerallstars/elementary-coder.git ~/.config/opencode/skills/elementary-coder
```

**MiMoCode**

```bash
git clone https://github.com/archerallstars/elementary-coder.git ~/.config/mimocode/skills/elementary-coder
```

Update later with `git -C <skill-dir> pull`.

Ask the agent to use it: `/elementary-coder`, or “apply elementary-coder standards”.

## What it requires

The full rules live in [SKILL.md](SKILL.md). In short:

| Rule | Intent |
|---|---|
| 1–5, 11–12 | Descriptive names and comments that stay true to the code |
| 6–8 | File section order, queued local I/O, concurrency when it is safe |
| 9–10 | Upgrade skeletons in place; edit legacy code only in the changed area |
| 13 | Read the real SDK in the project before calling it |
| 14–22 | Explicit `else`, no unused params, grouped functions, no passthrough wrappers |
| 23 | Never put secrets in comments |
| 24 | Every agent commit records harness + plan/build model + prompt/round summaries |
| 25 | Living `TESTING.md`: live-verified feature boxes; a round covers only what it touched |
| 26 | Living `SECURITY.md`: threat model at the repo root; README links to it |
| 27 | Living `docs/features/` notes, `docs/researches/`, and `AUDIT-REPORT` / `BUG-REPORT` / `BUG-FIX` |

Rule 24 fills harness version from `opencode --version`, `grok --version`, or `mimo --version`, then records plan vs build model for that harness.

Rule 25: boxes track live-verified features across sessions and agents (a layer besides git). A round covers only the boxes its code changes touch. Check a touched box after a live pass in the real environment. Unit tests are a gate. Use generic fixture names (`portrait.jpg`, `sample.txt`). Agent files such as `AGENTS.md` may point at `TESTING.md`; they must not copy the boxes.

Rule 26: every project keeps `SECURITY.md` (what secrets exist, where they live, who can read them, known limits). Never put real secrets in that file. The README links to it; a long analysis stays in `SECURITY.md` with a short README summary. Update it in the same round as auth or secret-storage changes.

Rule 27: application projects keep `docs/features/<slug>/<SLUG>.md` (why, locked decisions, what shipped) and `docs/researches/` for investigation notes. Date-named directories are not used. An audit pass writes `AUDIT-REPORT-…` when the feature is clean, or `BUG-REPORT-…` then `BUG-FIX-…` with the **same** 8-hex unique id (`openssl rand -hex 4`) when bugs were found. Model slugs match the rule-24 Plan/Build model ID. These files are not `TESTING.md` boxes. A skill-only repo may omit `docs/features/` (`SKILL.md` is the feature doc).

This repository’s own outline is [TESTING.md](TESTING.md). Threat model: [SECURITY.md](SECURITY.md) (skill text only; no runtime credentials). This repo omits `docs/features/` because it is skill text.

## License

[Unlicense](LICENSE) — public domain. Use it however you want.
