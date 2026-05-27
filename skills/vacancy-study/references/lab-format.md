# Format for theory and labs

> Reference example: [devops-labs/theory/01-buildkit.md](../../../../devops-labs/theory/01-buildkit.md) + [devops-labs/labs/lab01-buildkit/](../../../../devops-labs/labs/lab01-buildkit/). Before writing a new topic, always read the reference — match the tone, depth, and structure.

## Principles

1. **Theory and lab are two separate files.** Theory explains the concept (read before getting hands-on). The lab is a step-by-step hands-on (run in the terminal).
2. **Tone is a personal-notebook recap, not marketing.** No "this is a powerful tool" or "a modern framework that...". Only specifics: what it does, which commands, which gotchas.
3. **Explain WHY, not just what.** If you write "use cache mount" — explain why it beats a plain RUN.
4. **Don't invent commands/flags.** If you're unsure about syntax — verify with `mcp__context7__*` or WebSearch.

## Format of `theory/NN-<topic>.md`

```markdown
# <Topic> — Theory

## What it is and why it matters

(2-4 dense paragraphs. Definition + why you'd want it + how it broadly works + how it differs from its closest neighbors. No bullet lists in this section — it's the intro.)

## Key concepts

**<Term 1>** — definition + context of use, one paragraph.

**<Term 2>** — definition + context of use, one paragraph.

(5-10 key terms. Each in bold, expanded with one substantive paragraph.)

## Architecture / How it works under the hood

(Description of how it works mechanically + ASCII diagram if appropriate. Example diagram:)

```
client request
    │
    ▼
component A ─────► component B ─────► storage
    │                  │
    ▼                  ▼
   log                cache
```

(Text after the diagram — how to read it, what the nuances are.)

## When to choose it / Trade-offs

(Comparison table against alternatives, minimum 3-4 rows.)

| Scenario | This tool | Alternative |
|----------|----------|--------------|
| ... | ... | ... |

## Typical production gotchas

**1. <Gotcha>.** Symptom → cause → fix. One paragraph.

**2. <Gotcha>.** Symptom → cause → fix.

(5-7 numbered items. Each is a real production problem with a concrete fix.)

## Further reading (optional)

- Official documentation: <url>
- In-depth guide: <url>
- Specific pattern: <url>
```

**Length:** 100-250 lines. If it grows beyond that — the topic is too broad, split it into two theory files.

## Format of `labs/labNN-<topic>/README.md`

```markdown
# Lab N — <Topic>

> **Read the theory first:** [../../theory/NN-<topic>.md](../../theory/NN-<topic>.md)

## Goal

(1-2 sentences. What we'll end up seeing/getting hands-on.)

## Prerequisites

- Tool 1 (`brew install ...` or a link to the install guide)
- Tool 2
- Optional: alternative path if macOS lacks native support

## Steps

### Step 1 — <Action>

```bash
command
```

**What to watch:** what should happen / what will show up in the output.

### Step 2 — <Action>

```bash
command
```

**What to watch:** ...

(Steps in increasing difficulty. Each block: what to run → expected output → what it means.)

## What to see/verify

- [ ] Checkbox 1 — concrete verifiable condition
- [ ] Checkbox 2 — ...
- [ ] Checkbox 3 — ...

## Talking points for the interview

1. **"How did you do X?"** — Answer in 1-2 sentences, referencing something concrete.
2. **"When would you pick X over Y?"** — Trade-off answer.
3. **"What gotchas did you hit?"** — A concrete story.
4. **"Why this technology at all?"** — A short value proposition.
5. **"What would you do differently?"** — Reflection.
```

**Length:** 100-300 lines. Supporting files (Dockerfile, yaml, sh scripts) — working, verifiable, with "why it's like this" comments.

## What NOT to write

- Marketing language ("revolutionary", "powerful", "modern" without specifics)
- Bullet lists in "What it is and why it matters" — that section should be prose
- ASCII diagrams where they add no meaning (not for every topic)
- Made-up "gotchas" — only real production problems known to the community

## Parallelization

If you need to create 4+ new topics — delegate to subagents in parallel. Hand each agent:
- The path to the reference (`devops-labs/theory/01-buildkit.md`)
- The path to the topic plan/brief (what to cover)
- Their ownership zone (which files to create, don't touch others')
- Style constraints (5 sections, notebook tone, length)

Example subagent prompt:

> You're creating study materials for interview prep. Reference example: `devops-labs/theory/01-buildkit.md` — read it and match the structure/tone/depth.
>
> Your ownership zone (DO NOT TOUCH OTHER FILES):
> - `<area>-labs/theory/NN-<topic>.md` — theory
> - `<area>-labs/labs/labNN-<topic>/README.md` — lab
> - `<area>-labs/labs/labNN-<topic>/<supporting files>`
>
> Specific content:
> - Cover the topics: <list>
> - The lab must contain working commands to run
> - Talking points at the end — 5 items
>
> Return a short summary of what was created; don't duplicate the content.
