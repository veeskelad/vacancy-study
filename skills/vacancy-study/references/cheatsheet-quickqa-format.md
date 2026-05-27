# Format for THEORY-CHEATSHEET.md and QUICKQA.md

> Reference examples: [devops-labs/THEORY-CHEATSHEET.md](../../../../devops-labs/THEORY-CHEATSHEET.md) and [devops-labs/QUICKQA.md](../../../../devops-labs/QUICKQA.md).

## THEORY-CHEATSHEET.md

Location: `<area>-labs/THEORY-CHEATSHEET.md` (at the root of `devops-labs/` or `ai-labs/`).

**Purpose:** condensed theory across every topic in the area, for reading the morning before an interview (30-40 minutes). It's not a full duplicate of `theory/NN-*.md` — it's a distillation of the key facts.

### Structure

```markdown
# Theory Cheatsheet — <Area>

Condensed theory across all topics. Read in 30-40 minutes before the interview. If you can't recall something — the details are in `theory/NN-*.md`.

---

## 1. <Topic>

**What it is:** one phrase.

**Why we moved to it / Why we need it:**
- 3-5 bullets with the key benefits

**Key bits:**
- `command/syntax` — what it does, in one line
- (4-6 bullets with concrete commands/patterns)

**Gotchas:** listed semicolon-separated, 3-5 gotchas on one line.

---

## 2. <Topic>
...

---

## Bonus: 2-3 sentence Q&A

**Question?** — Answer in 1-2 sentences.
**Another question?** — Answer.
(10-15 expanded Q&As for topics that need a little more depth than a one-liner)
```

### THEORY-CHEATSHEET principles

1. **Each topic — 10-20 lines.** No more. It's a cheatsheet, not a textbook.
2. **Specifics, no fluff.** No "this is an important concept" — only commands and facts.
3. **Gotchas on a single semicolon-separated line** — saves space.
4. **A link to the details** at the top of the file — anyone who wants to go deeper heads into `theory/`.
5. **A bonus Q&A block** at the end — for questions that don't fit into a one-liner.

### Length

100-300 lines total across 7-10 topics.

---

## QUICKQA.md

Location: `<area>-labs/QUICKQA.md`.

**Purpose:** a combat cheatsheet of one-liners (the one-liner Q&A format — question → answer in one line, originally inspired by a Russian engineering team's note-taking habit). Opened in Sublime/iA Writer right before the call — the user skims the questions and refreshes the associations.

This is the **final artifact** — assembled AFTER running the labs, reflecting what's actually been done hands-on.

### Structure

```markdown
# Quick Q&A — <Area>

> Format: question → answer in a single line. Open in Sublime/iA Writer before the call, skim with your eyes.

---

## 1. <Topic>

- **<Question>?** — <Answer in a single line, concrete, with a command/number/term>.
- **<Question>?** — <Answer>.
- (10-15 one-liners per topic)
- **Main gotcha?** — <Which one> (one line)

---

## 2. <Topic>
...

---

## Talking pattern — how to answer in the interview

- **If you know it** — answer concretely with commands/flags/numbers: "At <company> we did X through Y, and got Z"
- **If you don't know it** — be honest: "I haven't used it in prod, but I know the concept — I ran a lab on it yesterday in [<area>-labs/](./)". That earns you karma.
- **If the task is diagnostic** — talk through your reasoning: "First I'd look at X, then Y"
- **Don't guess** — better to ask a counter-question: "Is the symptom X or Y?"
- **Your own experience is currency** — <companies from the bio>
```

### QUICKQA principles

1. **One-liner = strictly one line.** If it doesn't fit — trim it. This is a tool for eye-skimming.
2. **Format: `**Question?** — Answer.`** Markdown-bold question, em dash, answer. No sub-bullets under the question — one line.
3. **Specifics — commands/numbers/names.** "ldapsearch -x -b dc=..." is stronger than "the LDAP search command".
4. **10-20 one-liners per topic.** Too few — doesn't cover it. Too many — eyes get tired skimming.
5. **Bonus categories** (Linux/Networking/K8s/Ansible/SQL etc.) — for topics that don't have their own dedicated labs.
6. **Talking pattern at the end** — the meta layer, how to answer in general.

### Length

200-400 lines. For large areas (like devops-labs) it can grow to 300+.

### When to update

- **After running a new lab** — add 10-15 one-liners on the topic
- **After an interview** — add the questions that were asked (this is the most valuable content — real interview questions)

---

## Integration with the other files

```
<area>-labs/
├── theory/NN-*.md            # ← Full theory. Read before the lab.
├── labs/labNN-*/README.md    # ← Hands-on. Run after the theory.
├── THEORY-CHEATSHEET.md      # ← Distillation of all topics. Morning of the interview.
└── QUICKQA.md                # ← One-liners. In Sublime right before the call.
```

The chain:
- Theory → Lab → CHEATSHEET → QUICKQA
- Each next artifact compresses the previous one
- In the interview the user moves backwards: QUICKQA refreshes the memory → if forgotten → CHEATSHEET → if details are needed → theory/

## What vacancy-study adds

When the skill fires on a new vacancy:

1. If the JD topics already exist in `<area>-labs/theory/` → update the CHEATSHEET in one line (if it wasn't there yet) + add one-liners to QUICKQA
2. If the topics are new (gap) → after creating theory + lab, add a section to the CHEATSHEET and one-liners to QUICKQA
3. Don't rewrite older sections — only extend them

This lets material accumulate between vacancies. After 5 interviews, QUICKQA becomes a serious combat artifact.
