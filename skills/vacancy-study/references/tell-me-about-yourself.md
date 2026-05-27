# Two-minute self-introduction (Tell-me-about-yourself)

> This is the first impression in the interview. The main artifact of the prep file. Give it dedicated attention — it's not a bullet list, it's a coherent story that the user will say out loud.

## Principles

### 1. Source of truth — the user's resume from step 1

Before writing, read the user's resume (obtained in step 1 of SKILL.md). Every fact in the story must rest on that resume. Never invent experience, titles, numbers, or technologies.

If the project has multiple resume files (`cv.md`, `resume.md`, `*_profile.md`) and they disagree — ask the user which one to treat as current, or use the most recent by `mtime`.

### 2. Length — 75-90 seconds spoken

In text — 130-180 words in English. When the user reads it aloud at a normal pace, that produces 75-90 seconds. Real "2 minutes" is typically 90-110 seconds + pauses.

**Check:** `echo "<text>" | wc -w` → should land between 130-180.

### 3. The full cycle has to be spelled out

Rule: the self-introduction has to explicitly walk through the full work cycle — architecture → development → tests → deploy → observability/support.

This doesn't mean "list it literally". It means mention several stages of the cycle so the interviewer can tell this person sees the work end-to-end, not just their slice. It works for any role (DevOps, AI, Backend, Frontend) — each stage adapts to the specifics.

### 4. Tuning to the vacancy

The story adapts to the vacancy type:

| Vacancy type | Emphasis in the story |
|--------------|-----------------------|
| **DevOps / SRE** | Production K8s, GitLab CI, observability, infra projects (Astra hybrid, 9TECH bare-metal). Mention AI in one phrase as a bonus |
| **AI Engineer / AI Architect** | AI projects in production (Aktru, hybrid RAG, multi-agent), MCP, LangGraph. DevOps is the foundation that lets the AI be taken seriously |
| **Mixed (DevOps + AI)** | Both tracks equally: production DevOps experience + AI engineering with the same production mindset |
| **Backend / Platform** | Architecture, backend services, systems design. DevOps as infra, AI as a bonus |

### 5. Structure

```
1. Name + current role and years of experience (1 sentence)
2. The most relevant current context (1-2 sentences) — company + what it does + key technologies for the JD
3. Previous experience (1-2 sentences) — short, to show depth/breadth
4. Parallel track (optional, 1 sentence) — if there's AI/side-projects/open source
5. Closing line — why looking for something new (1 sentence, no complaints)
```

**Don't put a "My name is..." header on a separate line — this isn't a presentation. It's connected prose.**

## Template

```markdown
**Script:**

"My name is <Name>, I'm a <role> with <N> years of production experience. The last <X> years
at <current company> — <2-3 words about the company> with <stack/context>. I drive
<the full cycle in one phrase: architecture → infra → CI/CD → deploy → observability>.
Before that, <Y> years at <previous company> doing <what — 1-2 key projects/
technologies>. <Optional: parallel track — AI projects / open-source>.
Right now I'm looking for <kind of team/product> where I can <why — should resonate
with the JD of the current vacancy>."

(<computed word count> words, ~<seconds> seconds when read at a normal pace)
```

## Micro-variants (optional)

After the main script, add 2-3 short lines in case you get interrupted:

```markdown
**Micro-variants:**
- If they interrupt about <topic X> — "<short answer>"
- If they ask why you left <company> — "<honest answer without complaints>"
- If they want more on <technology Y> — "<reference to a specific case>"
```

## Examples for different roles

### Example 1 — DevOps/SRE

> "My name is [Name], I'm a DevOps/SRE engineer with 4 years of production experience. The last 2 years at [Company A] — [short description: domain, infra specifics]. I drive the full cycle: from architecture and Terraform code through GitLab CI pipelines to Kubernetes deploys via ArgoCD and observability on VictoriaMetrics + Grafana + Loki. Before that, 2 years at [Company B] administering bare-metal Kubernetes and PostgreSQL — full node lifecycle, etcd backups, control-plane upgrades. Right now I'm looking for a team where I can [motivation, clearly stated]."

~150 words, ~75 seconds.

### Example 2 — AI Engineer

> "My name is [Name], I'm an AI Engineer with [N] years in production AI. The last [X] years at [Company A] — [product]. I drive the full cycle of an AI feature: from problem analysis and proof-of-concept through LLM pipeline design (LangGraph + retrievers + evals), Kubernetes deploy, all the way to observability via [LangSmith / our own metrics infra]. The main project is [one-line description]. In parallel, [open-source/portfolio — if applicable]. Right now I'm looking for [kind of team/problem I want to work on]."

### Example 3 — Backend

> "My name is [Name], backend engineer on [Go/Python/Java], [N] years of experience. The last [X] years at [Company A] — working on [system/domain]. I do the whole cycle: from proposal and API design through implementation and tests to production rollout and on-call support. The hardest project — [one phrase]. Before that [context in 1-2 phrases]. Looking for [where I want to go]."

All three — about 130-160 words, when read aloud they fit into 75-100 seconds.

## Practice before the interview

1. Read it aloud **three times**, timing yourself
2. If you're over 90 sec — cut (drop one detail or compress the previous experience)
3. If under 75 sec — expand (add one more detail about a key project)
4. Record yourself once on the voice memo — listen from the outside
5. Run it in front of a mirror or in a Zoom window (if the interview is video) — watch your face and pace

## What NOT to do

- **Don't make it a bulleted list.** This is a connected story.
- **Don't start with "I was born in..."** — no pre-career details.
- **Don't mention tools you haven't touched with your hands.** If a technology is only "I know it from articles/courses" — it's not in the story. If the interviewer digs in, you'll get caught on the details.
- **Don't apologize** ("I'm not as experienced as I could be...") — it kills the impression.
- **Don't read literally from a sheet** — it shows. Practice so you can say it nearly from memory.
- **Don't stretch the past roles.** Compact sounds stronger: "2 years at X, 2 years at Y" beats "first there, then over there, then somewhere else...".
- **Don't mention reasons for leaving** in the main script. Only if asked (have a micro-variant ready).

## Universal principles for the story

- **GitHub for AI interviews:** if the vacancy is AI — mention "AI projects in production, repositories are public" with a concrete link
- **Full cycle:** explicitly walk through architecture → development → tests → deploy → observability in one phrase
- **Don't mention what you haven't verified:** the technologies in the story are only the ones in the resume as production experience or that you've run through in labs

## What to pull from the resume

Before writing the story, extract from the user's resume:

1. **Current role and years of experience** — total N years
2. **Current workplace** — company, what it does, key technologies (pick the ones in the JD)
3. **Previous role (1-2)** — for depth/breadth
4. **Parallel tracks** — AI projects, open-source, side-projects
5. **A unique achievement** — the thing that sets you apart from 100 other candidates (one phrase, if it fits)

Don't dump everything in — the story has to resonate with the JD. If the vacancy is about K8s — talk K8s. If it's about AI — talk AI. The rest gets cut.
