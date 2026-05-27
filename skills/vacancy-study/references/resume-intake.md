# Resume intake across platforms

The skill must accept a resume in **any** format and through any channel — otherwise you'll cut off users who didn't prep a file in advance. The goal is to remove the barrier.

## Sources in priority order

1. **Existing file in the project** (Claude Code/Cowork with filesystem)
2. **File attached to the message** (PDF / docx / txt / md / anything else)
3. **Plain text in chat** (paste)
4. **URL to an external profile** (LinkedIn / HeadHunter / habr.com/career / Хабр / etc — HeadHunter and habr.com/career are Russian developer career portals)
5. **Free-form story** — the user just talks about themselves, the skill asks clarifying questions

Move top to bottom. If source 1 gives a result — don't ask about sources 2-5.

## 1. Searching for an existing file

Only if filesystem is available (Read tool accessible).

**Standard names** (in priority order):
```
./cv.md
./resume.md
./profile.md
./about.md
./*career_profile.md      (any *.md with "career_profile" in the name)
./*resume*.md             (any *.md with "resume")
~/cv.md
~/.career-ops/resume.md
~/Documents/resume.md
```

**Strategy:**
```bash
# Pseudocode
for candidate in [cv.md, resume.md, profile.md, *career_profile*.md]:
    if exists(candidate):
        return read(candidate)
return None
```

If you find several — take the most recently modified one (`mtime`) and let the user know multiple were found, ask which to use.

**Check freshness.** Compare `mtime` to the current date:
- < 30 days → "I found `<path>` (updated `<date>`). Using as is. Tell me if there are recent projects that aren't in there."
- 30-180 days → "I found `<path>` (updated `<date>`). Anything added in the last couple of months — new projects, technologies, role change?"
- > 180 days → "I found `<path>` (updated `<date>` — a while ago). The resume might be stale — what's been added or changed?"

## 2. Attached file

The user attached a file to the message.

**By extension:**

| Extension | How to handle |
|-----------|---------------|
| `.md`, `.txt` | Read as text |
| `.pdf` | The `pdf` skill (Skill tool with pdf) or Read with extract-text |
| `.docx` | The `docx` skill (Skill tool with docx) |
| `.html` | Parse as text, strip tags |
| `.rtf`, `.odt` | Ask the user — can you send it as pdf/md/txt? Most environments can't handle these |
| `.json`, `.yaml` | Structured resume — read as is |

**Strategy — on any platform:**
1. Try to read the file with the tools available
2. If that fails — ask: "I can't read this format. Could you send it as PDF, markdown, or paste the text?"
3. Never guess the contents from the filename

## 3. Plain text in chat

The user pasted the resume directly into the message. Simplest case — just read it.

Check for signs of a structured resume:
- Headings like "Опыт работы" / "Experience", "Образование" / "Education", "Навыки" / "Skills"
- Dates in YYYY-MM or MM/YYYY format next to companies
- Position titles (Senior X, Lead Y, etc.)

If the structure is weak — ask 1-2 clarifying questions before moving on.

## 4. URL to an external profile

Accept the URL and try to read it via WebFetch (if available).

**Typical sources:**
- LinkedIn (`linkedin.com/in/<user>`)
- HeadHunter (`hh.ru/resume/<id>`)
- Хабр Карьера (`career.habr.com/<user>`)
- GitHub README (`github.com/<user>`)
- Personal site / portfolio

**If WebFetch doesn't work or the site requires auth:**
> "Looks like I can't reach `<URL>` — profiles on that platform usually require a login. You can:
> 1. Export to PDF and send it
> 2. Copy the key sections (Experience, Skills) as text into chat
> 3. Just tell me in your own words — I'll ask clarifying questions"

## 5. Free-form story

If the user just says "I did X, worked at Y" without structure — that's a normal path. Many people prefer talking over files.

**Tactics:**
1. Listen to what the user is telling you
2. Structure it in your head: roles, companies, technologies, projects, years
3. Ask clarifying questions from `references/clarifying-questions.md` — but only what's actually needed for the current JD, don't run a long interrogation
4. At the end offer to save what you got into a file (with consent): "Want me to write this into `resume.md` — so next time you won't have to repeat yourself?"

## What should come out of step 1

After any intake path — the skill should have a structured understanding in context:

```
- Current role: <title>
- Current company + tenure
- Previous 1-2 roles (company + what they did)
- Key production-experience technologies (only those they actually touched)
- 2-3 main projects/achievements
- Education (if relevant to the JD)
- Working languages (if relevant to the JD)
- Location and work format
- Future preferences (what they want to do)
```

If something substantial is missing after the initial intake — fill it in via clarifying-questions.

## Saving the resume (optional)

Only with filesystem and explicit user consent.

**If the resume is new** (received this session, not from a file):
> "Want me to save this as `resume.md` in the project? Then next time I can use it right away without you repeating yourself."

If they agree → create `resume.md` at the project root in markdown with sections Summary / Experience / Projects / Skills / Education.

**If the resume was updated** (found an old one + the user added to it):
> "Want me to append the new projects/technologies to the existing `<path>`? I'll add a separate `## Recent updates` section with the date so the previous text isn't overwritten."

Never rewrite an existing resume file in full without explicit consent.

## Platform-specific nuances

| Environment | What's available | What's not |
|-------------|------------------|------------|
| **Claude Code locally** | Read/Write on the project + ~/, WebFetch, Bash, PDF skill | — |
| **Cowork** | Read/Write in the sandbox, WebFetch, PDF skill (if installed) | Read on ~/ is limited |
| **Claude.ai (web)** | Attached files (PDF, docx via built-in), conversation context | Filesystem (only if MCP is connected) |
| **Claude.ai (desktop)** | Filesystem MCP if connected | Without MCP — conversation only |

On Claude.ai without filesystem MCP there's nowhere to save the resume — it lives in conversation context. That's fine, the skill still works.
