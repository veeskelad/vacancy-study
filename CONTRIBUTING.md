# Contributing to vacancy-study

Thanks for considering a contribution. This skill stays useful only if it stays accurate to real interview practice, so feedback from working engineers is the most valuable thing you can offer.

## What kind of contributions help most

- **Bug reports** — the skill misbehaves, generates nonsense, or fails in a specific environment. Open an issue with the conversation transcript (anonymise resume/JD content as needed).
- **Better example scripts** in `references/tell-me-about-yourself.md` for role families not yet covered (Data Engineer, ML Researcher, Mobile, SRE, etc).
- **Platform additions** in `references/platform-adaptation.md` — NixOS, OpenSUSE, FreeBSD, etc.
- **Translations** of `README.md` into additional languages (place them as `README.<lang>.md` at root and link from the top of the English README).
- **New role-family clarifying questions** in `references/clarifying-questions.md` — what does a good follow-up look like for an ML researcher? a security engineer? a staff-level IC?
- **CI improvements** — additional checks beyond frontmatter validation.

## What does NOT belong here

- **Job-board scrapers, applicant tracking, or vacancy scoring** — those are separate concerns; see [career-ops](https://github.com/santifer/career-ops) for that workflow.
- **LeetCode trainers, system design simulators** — different scope, different skill.
- **Auto-apply or auto-message recruiters** — explicitly out of scope; this skill never submits anything.

## Workflow

1. Open an issue first for anything substantial — saves rework
2. Fork → branch → commit → PR against `main`
3. The CI must pass (validates `SKILL.md` frontmatter and YAML)
4. Add a `CHANGELOG.md` entry under `## [Unreleased]`
5. Keep changes scoped — one logical change per PR

## Style

- The tone across all files is **practitioner's notebook** — terse, specific, no marketing fluff. Avoid "powerful", "modern", "revolutionary".
- Code blocks, command names, and file paths stay verbatim; explanations go in prose around them.
- Tables for comparisons; ASCII diagrams for flows; prose for everything else.
- 80-100 column soft wrap, no hard wrap.

## Testing your change

After editing the skill:

1. Copy your local checkout into `~/.claude/skills/vacancy-study` (or symlink it)
2. Restart Claude Code or run `/skills reload`
3. Try the skill against a sample JD + resume — verify the output reads correctly and the cited reference files are reachable

## License

By contributing, you agree your contributions are licensed under MIT (same as the rest of the project).

## Questions

Open a discussion in the GitHub repo. For private feedback, you can reach the maintainer at the email listed in the GitHub profile.
