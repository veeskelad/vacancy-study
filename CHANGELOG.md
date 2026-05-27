# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-05-27

### Added

- Initial public release.
- `SKILL.md` with a 10-step workflow: platform detection → resume intake → JD parsing → role classification → topic extraction → cross-reference with existing materials → gap analysis → lab generation → prep file → cheatsheet + Q&A → report.
- Seven reference files in `references/`:
  - `resume-intake.md` — accept a resume in any format (existing file, attached PDF/docx, pasted text, URL, free-form story) across Claude Code, Cowork, and Claude.ai.
  - `clarifying-questions.md` — follow-up questions grouped by role family (DevOps/SRE, AI/ML, Backend, soft, JD-specific) with anti-patterns.
  - `platform-adaptation.md` — `brew` / `apt` / `dnf` / `pacman` command mapping for macOS, Linux Debian/Ubuntu, Linux RHEL/Fedora, Arch, and WSL; notes on Buildah workarounds for macOS, kind on M1, systemd on WSL.
  - `lab-format.md` — 5-section theory template and lab README structure with parallelisation guidance.
  - `prep-format.md` — interview prep file structure (TL;DR, mapping, top-10 Q&A, STAR+R, weak spots, questions to ask, morning checklist).
  - `tell-me-about-yourself.md` — 2-minute self-introduction template with example scripts for DevOps, AI Engineer, and Backend roles.
  - `cheatsheet-quickqa-format.md` — `THEORY-CHEATSHEET.md` and `QUICKQA.md` formats.
- Plugin marketplace metadata (`.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json`) for one-command install in Claude Code.
- Bilingual README (English + Russian).
- MIT license.
- CI workflow that validates SKILL.md frontmatter on every PR.
- Example JD + sample output in `examples/`.

[1.0.0]: https://github.com/veeskelad/vacancy-study/releases/tag/v1.0.0
