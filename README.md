# vacancy-study

> 🌐 **Languages:** [English](README.md) · [Русский](README.ru.md)

A [Claude Agent Skill](https://agentskills.io/specification) that turns *"I have an interview next week"* into a complete, tailored study kit.

Give it your **resume** (any format — PDF, docx, markdown, plain text, or a URL) and a **job description**. It will:

1. **Parse your background** — and ask follow-up questions if something is missing
2. **Detect the role family** — DevOps, AI, Backend, Frontend, Data, Mobile, or mixed
3. **Cross-reference the JD with your real experience** — what's a strength, what's a gap
4. **Generate hands-on labs** for each gap topic — with theory + step-by-step practice
5. **Adapt every command to your OS** — macOS, Linux (Debian/RHEL/Arch), or WSL
6. **Write a prep file** — 2-minute "tell me about yourself", JD→experience mapping, top-10 questions with answer skeletons, STAR+R stories, preventive answers for weak spots, 5 questions to ask the company
7. **Build a cheatsheet and one-liner Q&A** — for the morning of the interview and the minutes before the call

Built for Claude Code, [Cowork](https://www.anthropic.com/news/cowork), and Claude.ai. Works in any language — it detects the language of your conversation and replies in kind.

---

## Why this exists

Most interview prep gets either too generic ("read the docs for Kubernetes") or too anxious ("memorize 500 leetcode questions"). What actually moves the needle is:

- **Honest gap analysis** — knowing what's in the JD that isn't in your resume
- **Hands-on practice on the missing pieces** — so you can honestly say *"I ran a lab on Kaniko yesterday — here are the trade-offs"* instead of bluffing
- **A spoken intro you've rehearsed** — the 2-minute "tell me about yourself" is the first impression and it's worth shaping deliberately
- **A short, scannable cheatsheet** — to skim on the morning of the call

This skill encodes that whole workflow. It assumes you're a working engineer, not a candidate. The tone is *practitioner's notebook*, not *bootcamp curriculum*.

---

## Installation

### Claude Code (plugin marketplace, recommended)

```
/plugin marketplace add veeskelad/vacancy-study
/plugin install vacancy-study@vacancy-study
```

After install, the skill activates automatically on phrases like *"prepare me for the interview"* or by invoking `/vacancy-study` explicitly.

### Claude Code (manual install)

```bash
git clone https://github.com/veeskelad/vacancy-study.git
cp -r vacancy-study/skills/vacancy-study ~/.claude/skills/
```

Restart Claude Code (or run `/skills reload`). The skill is now available in every project.

For a project-scoped install:

```bash
cp -r vacancy-study/skills/vacancy-study .claude/skills/
```

### Claude.ai

1. Clone or download this repo
2. Package the skill: `python -m scripts.package_skill skills/vacancy-study`
3. In Claude.ai → **Settings → Capabilities → Skills → Upload** → choose the generated `.skill` file

### Cowork

Same as Claude Code manual install. Copy `skills/vacancy-study/` into the Cowork workspace, then trigger the skill from chat.

---

## Quick start

Once installed, just paste a JD and a resume:

```
Here's a Senior DevOps role at [Company]. JD attached.
My CV is in cv.md. Prepare me for the interview on Tuesday.
```

Or step by step:

```
/vacancy-study
```

The skill will then:

1. Ask for your OS (one-time question, remembered after)
2. Look for `cv.md` / `resume.md` / `profile.md` in your project. If nothing is found, ask you to paste / upload / link your resume.
3. Ask 3–5 clarifying questions if anything important is missing
4. Ask for the JD if you haven't pasted it yet
5. Generate everything

Total output you'll get on disk:

```
devops-labs/              # or ai-labs/, backend-labs/, etc — whatever matches your role
├── theory/
│   └── NN-newtopic.md     # 5-section theory write-up per gap topic
├── labs/
│   └── labNN-newtopic/
│       ├── README.md      # platform-adapted hands-on steps
│       └── ...            # Dockerfiles, manifests, scripts
├── THEORY-CHEATSHEET.md   # condensed theory across all topics — for morning skim
└── QUICKQA.md             # one-liner Q&A — for the minutes before the call

interview-prep/
└── <vacancy-slug>-<YYYY-MM-DD>.md   # the prep file itself
```

---

## What's in the box

```
vacancy-study/
├── .claude-plugin/             # plugin marketplace metadata
│   ├── plugin.json
│   └── marketplace.json
├── skills/
│   └── vacancy-study/
│       ├── SKILL.md            # entry point — the 10-step workflow
│       └── references/
│           ├── resume-intake.md             # how to accept a resume in any format
│           ├── clarifying-questions.md      # follow-up questions by role family
│           ├── platform-adaptation.md       # macOS / Linux / WSL command mapping
│           ├── lab-format.md                # theory + lab file structure
│           ├── prep-format.md               # interview prep file structure
│           ├── tell-me-about-yourself.md    # the 2-minute self-intro template
│           └── cheatsheet-quickqa-format.md # cheatsheet and one-liner Q&A format
├── examples/                   # sample JDs and outputs
├── README.md                   # this file
├── README.ru.md                # Russian translation
├── LICENSE                     # MIT
├── CHANGELOG.md
├── CONTRIBUTING.md
└── .github/
    └── workflows/
        └── validate.yml         # CI: validates SKILL.md frontmatter on every PR
```

---

## Examples

See [`examples/`](examples/) for:

- A real JD parsed into topics
- A generated lab (theory + practice + talking points)
- A finished prep file with the 2-minute self-intro
- The QUICKQA one-liner format in action

---

## Design principles

1. **The resume is the foundation.** Without an accurate picture of the user's experience, the skill becomes a template generator. Step 1 (resume intake) is mandatory and the skill will ask follow-up questions rather than fabricate.

2. **Be universal.** Don't assume specific file names or directory structures. Probe and adapt to whatever's in the project.

3. **Don't duplicate.** If a topic already exists in the user's project (under `devops-labs/`, `ai-labs/`, etc), reuse it. Only generate what's missing.

4. **Commands must run.** Every lab is tailored to the user's platform — `brew install` for macOS, `apt install` for Debian/Ubuntu, `dnf install` for RHEL/Fedora. No copy-paste failures.

5. **Theory completeness.** Every theory file has the same five sections (what/why, key concepts, architecture, trade-offs, production pitfalls). Tone is "personal note", not marketing.

6. **The tell-me-about-yourself must cover the full delivery cycle** — architecture → development → testing → deployment → observability. And it must never invent experience.

7. **Be honest about gaps.** A preventive answer like *"haven't used in production, ran a lab yesterday — happy to explain the concept and trade-offs"* beats any bluff.

---

## Language support

The skill detects the language of your first message and **responds in the same language**. Tested with English, Russian, and German. Other languages should work automatically — Claude is multilingual.

Inside the generated artefacts (theory files, lab READMEs, prep file), the language matches the JD + resume by default. If the JD is in Russian and your resume is in English, the skill will ask which language to use for the output.

Code, file names, and technical identifiers always stay in their original form — no transliteration.

---

## What this skill is NOT

- **Not a job scoring system.** It doesn't tell you whether to apply or how well the role fits. For that, look at separate tools like [career-ops](https://github.com/santifer/career-ops).
- **Not a leetcode trainer.** It generates infrastructure/system labs, not algorithmic drills.
- **Not a CV writer.** It reads your resume; it doesn't reformat or polish it.
- **Not automated apply.** It never submits anything on your behalf.

---

## Contributing

Issues and PRs welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for guidance.

Some ideas already planned (open issues if you want to take any of these):

- **Mock interview mode** — drill the user with random questions from QUICKQA
- **Company research module** — short investigation of the product, stack, recent news
- **Post-interview capture** — record what was actually asked, append to QUICKQA so the kit gets sharper each cycle
- **Multilingual SKILL.md** — currently the SKILL body is English; reference files could be optionally translated for offline use

---

## License

[MIT](LICENSE) — use it however you want, including commercially. Attribution appreciated but not required.

---

## Acknowledgements

- The [Claude Agent Skills](https://agentskills.io/specification) standard
- Skill-creator workflow from [anthropics/skills](https://github.com/anthropics/skills)
- Inspired by years of real interview preparation cycles — both the wins and the awkward "I read about it once" moments

---

**Built for engineers who'd rather run a lab than bluff their way through a question.**
