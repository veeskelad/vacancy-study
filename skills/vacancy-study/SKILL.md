---
name: vacancy-study
description: Prepares a candidate for a technical interview against a specific job posting. Accepts the user's resume in any format (PDF / docx / md / txt / direct text / URL to LinkedIn / HH.ru / habr.com/career) and a job description, then generates a tailored study kit — hands-on labs (theory + practice) with platform-specific commands (macOS / Linux / WSL), an interview prep file with a 2-minute "tell me about yourself", a theory cheatsheet, and one-liner Q&A. Works in Claude Code, Cowork, and Claude.ai. Activate when the user pastes a job description, attaches a resume file, says "prepare me for the interview", "adapt the labs for this JD", "study this vacancy", "make theory for this role", "help me with the interview", or invokes /vacancy-study.
license: MIT
metadata:
  author: veeskelad
  version: "1.0.0"
  repository: https://github.com/veeskelad/vacancy-study
compatibility: Designed for Claude Code, Cowork, and Claude.ai. Optional integrations with the `pdf` and `docx` skills for resume parsing, `WebFetch` for URL-based resumes and JDs.
---

# vacancy-study — Tailor labs and prep materials to a specific job posting

This skill turns a pair of inputs — the user's resume + a job description — into a personalised interview preparation kit:

- Hands-on labs (theory + step-by-step practice) for each gap topic from the JD
- A `prep` file with a 2-minute "tell me about yourself", JD-to-experience mapping, top-10 questions with answer skeletons, STAR+R stories, weak spots with preventive answers, and questions to ask the company
- A `THEORY-CHEATSHEET.md` to skim on the morning of the interview
- A `QUICKQA.md` with one-liner Q&A for the battle phase right before the call

The skill is universal — it works for any role family (DevOps / SRE, AI / ML / MLOps, Backend, Frontend, Data, Mobile, mixed), in any language, and adapts shell commands to the platform the user will run them on.

## Communication language

**Detect the user's language from their first message and respond in the same language**. This applies to all conversational output — questions, summaries, status updates.

- Inside generated artefacts (theory, labs, prep file, cheatsheets), match the language of the resume + JD. If the JD is in Russian and the resume is in English, ask the user which language to use for the artefacts.
- Code, file names, and technical identifiers stay in their original form (no transliteration).
- For Russian and other languages with diacritics — preserve all accents and special characters (ё, й, ñ, ü, etc).

If the user explicitly switches languages mid-conversation, follow them.

## When to use

- The user provides a JD (text, URL, or path to a file) and asks for interview prep
- The user attaches a resume in any format (PDF / docx / txt / md / direct text / URL)
- The user says "adapt the labs to this vacancy", "make theory for this JD", "prepare me for the interview at X", or invokes `/vacancy-study`
- The user mentions an upcoming interview date with a specific company and needs materials

## What NOT to do

- Never invent experience the user doesn't have. The resume from step 1 is the only source of truth.
- Don't claim familiarity with tools the user has not touched. If a tool from the JD is missing from the resume, mark it as a gap and write a preventive answer ("haven't used in production, ran a lab yesterday — happy to explain the concept and trade-offs").
- Don't assume specific file names or project structure — probe and adapt to whatever the project layout is.
- Don't score the fit of the vacancy or recommend whether to apply — that's a different concern.

## Execution environments

This skill runs across three Claude surfaces. The logic differs slightly:

| Surface | Filesystem | Resume intake | Output location |
|---------|-----------|---------------|-----------------|
| **Claude Code (local)** | Full project access | Search standard locations first, ask if nothing found | Write to project files |
| **Cowork** | Sandboxed filesystem | Ask the user to upload or paste | Write to the working directory |
| **Claude.ai (web/desktop)** | Limited (unless a filesystem MCP is connected) | Ask for attachment or pasted text | Return artefacts as conversation files; persist to filesystem if MCP is available |

Detect the surface by checking available tools — if `Read` / `Write` exist, treat the environment as Code or Cowork; otherwise treat it as Claude.ai.

## Workflow

### Step 0 — Detect the user's platform

Needed once, so that lab commands use the right package manager (`brew install` vs `apt install` vs `dnf install`).

1. If a platform record file already exists in the project (e.g. `<area>-labs/PLATFORM.md` or `.vacancy-study/platform.txt`) — read it.
2. Otherwise, try `uname` on Claude Code if available.
3. Otherwise, ask once:
   > "Which OS will you run the labs on? macOS / Linux (Debian/Ubuntu) / Linux (RHEL/Fedora) / WSL?"

Remember the choice for the session. If a filesystem is available, persist it to a one-line file (`darwin` / `linux-debian` / `linux-rhel` / `linux-arch` / `wsl`) so the question doesn't repeat.

Full platform adaptation details → [references/platform-adaptation.md](references/platform-adaptation.md).

### Step 1 — Collect the resume / profile

This is the critical step. Without an accurate picture of the user's experience, the skill becomes a template generator.

**Algorithm:**

1. **Look for an existing resume** (only if a filesystem is available):
   - Probe standard names: `cv.md`, `resume.md`, `profile.md`, `about.md`
   - Also check the home directory: `~/cv.md`, `~/resume.md`, `~/.career-ops/resume.md`
   - Look for any `*career*.md`, `*resume*.md`, `*profile*.md` in the project root
   - Ask the user too: "Do you have a resume somewhere in this project or on disk? If yes, tell me the path."

2. **If a file is found:**
   - Read it
   - Compare `mtime` to today, and ask:
     - `< 30 days`: "I found your resume at `<path>` (updated `<date>`). Using it as-is. Tell me if anything new isn't in there yet."
     - `30–180 days`: "I found `<path>` (updated `<date>`). Anything added in the last couple of months — new projects, technologies, role change?"
     - `> 180 days`: "I found `<path>` (updated `<date>` — getting old). What's changed since then?"

3. **If no file (or the surface has no filesystem):**

   Say to the user:
   > "To make the prep honest, I need your resume. You can:
   > 1. Attach a file (PDF / docx / txt / md) — I'll read it
   > 2. Paste the text directly in chat
   > 3. Give me a URL (LinkedIn / HH.ru / habr.com/career / GitHub / personal site)
   > 4. Or just tell me about yourself — I'll ask clarifying questions"

   Handle each format:
   - **PDF / docx** → use the `pdf` / `docx` skills if available, otherwise ask for paste/markdown
   - **txt / md** → read directly
   - **URL** → `WebFetch` if accessible, otherwise ask the user to export and paste
   - **Free-form story** → start asking clarifying questions

4. **Clarifying questions** (after the first pass):
   Read what you have → identify gaps → ask 3–5 short questions relevant to **this** JD. Don't run a long survey without reason. See [references/clarifying-questions.md](references/clarifying-questions.md).

5. **Persist the understanding** (optional, only with filesystem and explicit consent):
   - If the resume came as a free-form story → offer to save it as `resume.md` in the project so the user doesn't have to retell it next time.
   - If the resume was updated → offer to append a `## Recent updates` section with today's date, never silently overwrite.
   - On Claude.ai without filesystem the resume lives only in conversation context — that's fine.

Full intake rules → [references/resume-intake.md](references/resume-intake.md).

### Step 2 — Receive the JD and metadata

If the JD wasn't included, ask. Frequently the user sends the resume + JD in one message — parse both at once.

Capture:
- The JD body (responsibilities, requirements, "nice to have")
- The company name
- The interview date if known (used in the prep file name)
- The interview format if known (technical / HR / system design / live coding / take-home)

If no date is provided, use today's date from the system context.

### Step 3 — Classify the role

Pattern-match the JD against keyword clusters:

| Cluster | Keywords (non-exhaustive) |
|---------|--------------------------|
| **DevOps / SRE / Platform** | Kubernetes, Docker, Ansible, Terraform, GitLab CI, GitHub Actions, Prometheus, observability, on-call, infrastructure |
| **AI / ML / MLOps** | LLM, agents, prompts, RAG, embeddings, LangGraph, vector DB, GPU inference, MLflow, model serving |
| **AI Engineer / AI Architect** | AI products, LLM in production, MCP, AI tooling, agent orchestration |
| **Backend** | REST / GraphQL APIs, microservices, databases, queues, backend languages |
| **Frontend** | React / Vue / Svelte, TypeScript, SSR, design systems, accessibility |
| **Data Engineering / Analytics** | ETL, Airflow, Spark, Snowflake, dbt, BI, warehouse SQL |
| **Mobile** | iOS / Swift, Android / Kotlin, React Native, Flutter |
| **Mixed** | two clusters strongly represented |

If the JD is ambiguous, ask the user — never guess silently.

### Step 4 — Extract topics from the JD

Pull a list of technologies and competencies from "responsibilities", "requirements", and "nice to have" sections. Group related items (e.g. `multistage + BuildKit + Buildah + Kaniko` → "Docker builders") so labs don't fragment.

### Step 5 — Cross-reference existing materials

If a filesystem is available, look for prior lab directories in the project:

- `<role>-labs/` (`devops-labs/`, `ai-labs/`, `backend-labs/`, …) — labs and theory
- `labs/`, `practice/labs/`, `interview-prep/labs/` — alternative locations
- `interview-prep/` — past prep files
- `notes/`, `learning/` — long-form notes

Match each topic from step 4 to one of three states:
- **Full lab already exists** → reuse, link from the prep file
- **Theory exists, lab missing** → add only the lab
- **Nothing yet** → create both theory and lab

On Claude.ai without filesystem, skip this step.

### Step 6 — Resume gap analysis

Compare JD topics against the resume from step 1:
- **Topic + production experience** → strength, mapped in the prep file with concrete project references
- **Topic missing from the resume** → gap, with a preventive honest answer ready
- **Topic in "nice to have"** → separate bucket, not critical

### Step 7 — Generate the missing labs

Choose the target directory `<area>-labs/`:
- If the project already has a structure, reuse it
- Otherwise create `devops-labs/`, `ai-labs/`, `backend-labs/`, etc. matching the role family
- For mixed roles, distribute labs by topic (Docker → devops-labs, LangGraph → ai-labs)

For each gap topic:
- `<area>-labs/theory/NN-topic.md` — five-section theory write-up (see [references/lab-format.md](references/lab-format.md))
- `<area>-labs/labs/labNN-topic/README.md` + supporting files (Dockerfile, yaml, scripts)

**Lab commands adapt to the platform** detected in step 0. For example, Buildah:
- macOS → Lima VM or a Docker-based wrapper
- Linux Debian/Ubuntu → `apt install buildah`, native
- Linux RHEL/Fedora → `dnf install buildah`, native
- WSL → like Linux Debian/Ubuntu with caveats

Full adaptation table → [references/platform-adaptation.md](references/platform-adaptation.md).

**Parallelisation:** if the environment supports subagents (Claude Code / Cowork) and more than three new labs are needed, delegate one topic per subagent in parallel with strict constraints and a reference example.

### Step 8 — Generate the prep file

Path: `interview-prep/<vacancy-slug>-<YYYY-MM-DD>.md` (or whichever directory the project uses for prep notes — e.g. `prep/`, `interviews/`).

Contents (full format in [references/prep-format.md](references/prep-format.md)):
- **TL;DR of the company** — 2–3 paragraphs
- **2-minute "tell me about yourself"** — see [references/tell-me-about-yourself.md](references/tell-me-about-yourself.md)
- **JD → experience mapping** — table linking each requirement to a concrete project from the resume
- **Top-10 likely questions** — each with a 3–5 point answer skeleton
- **2–3 STAR+R stories** — pulled from the resume or from a project-local `story-bank.md` if it exists
- **Weak spots** — preventive answers for gaps from step 6
- **5 questions to ask the company**
- **Morning-of checklist**

### Step 9 — Theory cheatsheet and one-liner Q&A

In `<area>-labs/`:
- `THEORY-CHEATSHEET.md` — add a compressed summary for the new topics. Create the file if it doesn't exist.
- `QUICKQA.md` — add one-liner Q&A for the new topics. Create the file if it doesn't exist.

Format details → [references/cheatsheet-quickqa-format.md](references/cheatsheet-quickqa-format.md).

### Step 10 — Report back

Short summary at the end:
- What was reused (links to existing labs)
- What was newly created (file list)
- Where the prep file lives
- Reading order: theory → lab → cheatsheet → quickqa → prep
- Platform used for lab commands

## Principles

1. **The resume is the foundation.** Without an accurate picture of the user's experience, the skill produces template content. Step 1 is mandatory.
2. **Be universal.** Don't assume specific file names or directory structures — probe and adapt.
3. **Don't duplicate.** If a topic already exists in the project, reuse it.
4. **Theory completeness.** Every theory file has the same five sections (what/why, key concepts, architecture, trade-offs, production pitfalls). Tone is "personal note" — no marketing, no "this powerful tool".
5. **Commands must run.** Lab commands are tailored to the user's platform from step 0.
6. **Every lab ends with 5 talking points** ready to drop into the interview.
7. **The "tell me about yourself" must cover the full delivery cycle:** architecture → development → testing → deployment → observability/operations. Never invent experience.
8. **The source of truth for experience is the resume** collected in step 1. If multiple resume files exist with conflicts, ask the user which one is current — don't silently pick one.

## Output structure summary

```
<role>-labs/                           # devops-labs/, ai-labs/, backend-labs/, etc
├── PLATFORM.md                       # platform record (first run only)
├── theory/NN-newtopic.md             # if gap
├── labs/labNN-newtopic/              # if gap
│   └── README.md + supporting files  # commands tailored to platform
├── THEORY-CHEATSHEET.md              # updated or created
└── QUICKQA.md                        # updated or created

interview-prep/                        # or whichever directory the project uses
└── <vacancy-slug>-<YYYY-MM-DD>.md    # new prep file

resume.md                              # optional — only if the resume was new and the user consented
```

## Reference index

| Reference | When to read |
|-----------|--------------|
| [references/resume-intake.md](references/resume-intake.md) | Step 1 — receiving a resume in any format across platforms |
| [references/clarifying-questions.md](references/clarifying-questions.md) | Step 1 — follow-up questions to fill gaps in the resume |
| [references/platform-adaptation.md](references/platform-adaptation.md) | Steps 0 and 7 — adapting lab commands to macOS / Linux / WSL |
| [references/lab-format.md](references/lab-format.md) | Step 7 — theory + lab structure |
| [references/prep-format.md](references/prep-format.md) | Step 8 — prep file structure |
| [references/tell-me-about-yourself.md](references/tell-me-about-yourself.md) | Step 8 — the 2-minute self-introduction |
| [references/cheatsheet-quickqa-format.md](references/cheatsheet-quickqa-format.md) | Step 9 — cheatsheet and one-liner Q&A format |

## Future extensions

- **Mock interview mode** — drill the user with random questions from QUICKQA
- **Company research** — short investigation of the product, stack on job boards, recent news
- **Post-interview capture** — record what was actually asked and append to QUICKQA so the kit gets sharper each cycle
- **Resume auto-update** — after the interview, offer to record any new projects/technologies the user mentioned, with explicit consent
