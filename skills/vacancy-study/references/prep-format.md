# Prep file format for an interview

> If the project already has prep files from past interviews (for example in `interview-prep/`) — look at them as a reference for tone and format. If not — use the structure below.

## File path

```
interview-prep/<vacancy-slug>-<YYYY-MM-DD>.md
```

- `<vacancy-slug>` — a short name: `flant-devops`, `tbank-sre`, `realweb-ai-architect`, `fintech-cards-devops`
- `<YYYY-MM-DD>` — interview date (if scheduled) or file creation date

## Structure

```markdown
# <Vacancy slug> — interview <YYYY-MM-DD>

## Company TL;DR

(2-3 paragraphs. What kind of company, what kind of product, what kind of team. What that means for the interview:
— expect deep dive or soft chat
— what stack to expect
— what's important to emphasize)

## Behavior during the interview

- **Format:** length, format (if known)
- **Tactics:** how to carry yourself
- **Main thing:** what to show first
- **What NOT to do:** traps and red flags

## Tell-me-about-yourself (60-90 sec) or (2 minutes)

> The full cycle has to be spelled out (see rule 2 in `principles.md`)

**Script:**

"..." (detailed text — see references/tell-me-about-yourself.md)

**Micro-variants:**
- If they interrupt about X — "..."
- If they ask why you left Y — "..."

## JD → experience mapping

| Vacancy requirement | My experience | Where exactly |
|---------------------|---------------|---------------|
| ... | ... | ... |

(Table with at least 8-12 rows. Every JD requirement → a concrete case from the biography + a source in story-bank.md or habr_career_profile.md.)

## Top-10 questions with prepared answers

### 1. <Question>

**Answer structure:**
- Point 1 (concept)
- Point 2 (how I did it myself)
- Point 3 (trade-offs / alternatives)
- Point 4 (recent experience — link to a lab)

(Make 10 questions, ordered by importance for this JD. Each one — a 3-5 point structure, not the full text of the answer.)

## 2-3 STAR+R stories for this interview

> If the project has a `story-bank.md` or a similar story collector — link there. Otherwise summarize the story directly in the prep file.

**Primary:** <Story title>
- When: which questions to trigger on
- Anchor: the main strength of the story for this vacancy

**Backup:** <Story title>
- When: which questions to trigger on
- Anchor: the main strength of the story for this vacancy

**Backup:** [<Story title>](story-bank.md#<anchor>)
- ...

**Soft / product thinking:** [<Story title>](story-bank.md#<anchor>)
- ...

## Weak spots — preemptive answers

### "<Weak question>"
> A pre-baked answer in 2-3 sentences. Honest. Note that a lab/experience has filled the gap (if true).

(At least 4-5 weak spots from gap analysis: what's in the JD that's not in the experience.)

## 5 questions for them (must ask)

1. **<Question about the product>** — why it matters
2. **<Question about observability/processes>**
3. **<Question about the team>**
4. **<Question about what hurts right now>**
5. **<Clarifying question about their specifics>**

## Morning / pre-interview checklist

- [ ] Re-read `principles.md`
- [ ] Run tell-me-about-yourself aloud twice, time it
- [ ] Open `<area>-labs/THEORY-CHEATSHEET.md` — skim for 30 minutes
- [ ] Open `<area>-labs/QUICKQA.md` in Sublime — glide through
- [ ] Prepare questions for them (pick 3-4 for a live conversation)
- [ ] Test camera/mic 15 minutes before start
- [ ] Cup of coffee and a glass of water within reach

## After the interview

- [ ] Write down impressions and the questions they asked
- [ ] Update `practice/PROGRESS.md`
- [ ] If they asked anything new — add it to `<area>-labs/QUICKQA.md`
```

**Length:** 200-350 lines. The file is compact, the goal is to open it and orient quickly, not produce literature.

## Writing principles

1. **JD → experience mapping is the currency.** The more precisely each requirement maps to a specific case from the biography (Astra, 9TECH, Aktru, AI projects), the stronger the prep file.
2. **Don't bury weak spots.** An honest preemptive answer + a link to a lab (if you ran one) is stronger than trying to dodge the question. That's rule 3 from principles.md.
3. **Work on tell-me-about-yourself separately.** It's the first impression — most of the polishing goes there. See `references/tell-me-about-yourself.md`.
4. **Top-10 questions — experience, not theory.** If the interviewer said "not theory, but why it breaks and how you fix it" — frame answers around problems, not definitions.
5. **5 questions for them — mandatory.** Good questions show the candidate is thinking about the product/team, not just ticking JD boxes.

## What NOT to write

- Literary musings about "what DevOps is as a profession"
- Listing every tool you've ever heard of "just to mention it"
- Full answer text for Q&A — only structures (5 points per question)
- Self-doubts ("afraid I won't get the job") — this isn't a note to your therapist

## Alignment with principles.md

| Rule | Where to honor it in the prep file |
|------|------------------------------------|
| 1 — Full cycle in tell-me-about-yourself | Tell-me-about-yourself explicitly walks through architecture → development → tests → deploy → observability |
| 2 — GitHub for AI interviews | If the vacancy is AI — mention specific repositories |
| 3 — Don't mention what you haven't verified | The "Weak spots" section honestly names the gaps |
| 4 — Tech reminders | In the Top-10, anchors on real patterns |
