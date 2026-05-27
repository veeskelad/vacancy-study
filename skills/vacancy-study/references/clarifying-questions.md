# Clarifying questions about experience

These questions are asked at step 1 — after the initial resume is in, but something substantial is missing for a quality prep file. The goal is to pick up what's missing without a long interrogation.

## Principles

1. **Don't ask everything at once.** Pick 3-5 questions relevant to the current JD.
2. **Open questions beat closed ones.** "Tell me about your toughest project" is stronger than "Have you worked with K8s?"
3. **Listen not only to what they say, but to what they DON'T say.** If someone is dodging a topic — that's probably a weak spot, and the prep file should reflect that.
4. **Don't ask for the sake of asking.** If FreeIPA isn't in the JD — don't ask about LDAP.
5. **Group questions.** One question — one topic. Don't pile three questions into a single paragraph.

## Baseline minimum (always)

If the resume you received does NOT have this data — definitely ask:

1. **Current role and tenure.** "Are you currently a [role] at [company]? How many years in this role?"
2. **Main production technologies.** "Which technologies have you touched in production over the last year? Not from theory — the ones you actually configured and supported."
3. **Location and work format.** Only if the JD mentions remote/hybrid/onsite or a specific country.

## For technical roles (DevOps / SRE / Backend / AI Engineer)

Pick 2-3 from this list for the specific JD:

### Depth of experience

> "Tell me about the toughest technical incident in your career. What happened, how did you fix it?"

Goal: gauge troubleshooting depth. A good answer is a concrete case with diagnosis and fix. A bad one is generalities.

### Architecture and systems

> "Describe the system you know best from the inside. How many services, what stack, what did you do in it?"

Goal: gauge scale of experience. A 3-endpoint microservice vs a distributed system of 30 services — different worlds.

### Production ownership

> "What did you personally own in production — what breaks if you take vacation without handing it off?"

Goal: gauge real responsibility. Often 0 at junior level; at senior — specific systems/processes.

### Technologies (if the JD has a specific list)

> "The JD mentions [X, Y, Z]. Which of these have you actually worked with in production, and which have you only seen / read docs on?"

This is the key question for gap analysis. Never assume that all the theory from the JD = user's experience.

## For AI / ML / MLOps

### Project depth

> "Which of your AI projects do you consider the most serious one? What was the task, the stack, who were the users?"

### Production AI

> "Is there an AI project running in production right now? How many requests per day, how do you monitor it, what was hard about the deployment?"

### Tooling

> "Which AI frameworks have you worked with hands-on: LangGraph, LlamaIndex, CrewAI, OpenAI Assistants, MCP, anything else? And what do you like or dislike about them?"

### Evaluation and safety

> "How do you test the quality of AI outputs? Which metrics do you watch?"

### GitHub visibility

> "Are any AI projects on public GitHub? This is critical for AI interviews — the interviewer will want to look at the code."

## Soft / Career

### Motivation (if "looking for work" isn't stated explicitly)

> "Why are you looking for a new job right now? What's not working in the current one, what do you want?"

This is for the prep file — the phrasing for "why are you leaving" should be honest but without complaints.

### Preferences (for "5 questions for them")

> "What do you want to do more of at work, and what less? What energizes you, what drains you?"

### Unique achievement

> "What are you proudest of in your career? One project/story — the thing that sets you apart from 100 other candidates."

This is for tell-me-about-yourself — the main argument.

## JD-specific

### If the JD mentions a specific programming language

> "Which language did you write the most code in over the last year? Sketch level, maintaining existing code, or production development of new code?"

### If the JD mentions management

> "Do you have team lead experience or are you an IC (individual contributor)? How many people, what kind of work?"

### If the JD mentions a specific domain (fintech, healthcare, gaming)

> "Have you worked in [domain]? What domain-specific nuances do you know — regulation, load characteristics, anything else?"

## Anti-patterns

**Don't do:**

❌ "Tell me about yourself" — too broad for the first stage, better to get into specifics

❌ "What's your English level?" — not relevant to the prep file for most interviews

❌ "Where do you live?" — only if the JD mentions location

❌ A long list of 15 questions in a row — the user will get tired

❌ Repeat questions — if they already mentioned K8s, don't come back to it

## How many questions at once

- **Ideal:** 1-2 questions per message
- **Maximum:** 3-4 if they're thematically related
- **Never:** more than 4

It's better to ask 4 questions one at a time — the user opens up more deeply. Than 4 questions all at once — a shallow answer to each.

## When to stop clarifying

When the context contains:
- Current role and tenure
- Main production-level technologies
- 2-3 key projects (not just names — what they did in them)
- An understanding of the motivation for changing jobs

→ move on to step 2 (JD).

If the user says "enough questions, let's move on" — move on. Better to work with an imperfect picture than to wear the user out with the survey.
