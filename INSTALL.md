# Installation guide

Detailed installation instructions for each platform. For a quick overview, see [README.md](README.md#installation).

## Claude Code

### Option A — via plugin marketplace (recommended)

This gives one-command install and easy updates.

```
/plugin marketplace add veeskelad/vacancy-study
/plugin install vacancy-study@vacancy-study
```

After install:
1. Restart Claude Code (or run `/skills reload`)
2. Verify: type `/skills` — `vacancy-study` should appear
3. Try: paste any JD with the word "interview" — the skill should auto-activate

### Option B — manual install (user-scope)

Available globally across all your projects.

```bash
git clone https://github.com/veeskelad/vacancy-study.git ~/.tmp-vacancy-study
mkdir -p ~/.claude/skills
cp -r ~/.tmp-vacancy-study/skills/vacancy-study ~/.claude/skills/
rm -rf ~/.tmp-vacancy-study
```

Restart Claude Code. Verify with `/skills`.

### Option C — manual install (project-scope)

Only available in the current project.

```bash
git clone https://github.com/veeskelad/vacancy-study.git /tmp/vacancy-study
mkdir -p .claude/skills
cp -r /tmp/vacancy-study/skills/vacancy-study .claude/skills/
rm -rf /tmp/vacancy-study
```

If your project uses git: add `.claude/skills/vacancy-study/` to `.gitignore` if you don't want the skill committed to the project repo, or commit it if you want the whole team to share.

### Updating

For marketplace installs:

```
/plugin update vacancy-study@vacancy-study
```

For manual installs: re-run the install command (it's a clean copy each time).

## Claude.ai

Claude.ai accepts skills as `.skill` files (ZIP archives).

1. Clone the repo:
   ```bash
   git clone https://github.com/veeskelad/vacancy-study.git
   cd vacancy-study
   ```

2. Package the skill. If you have the [official Anthropic skill-creator](https://github.com/anthropics/skills) installed:
   ```bash
   python -m scripts.package_skill skills/vacancy-study
   ```

   Otherwise, create the archive manually:
   ```bash
   cd skills
   zip -r vacancy-study.skill vacancy-study -x "*.DS_Store" "*/__pycache__/*"
   mv vacancy-study.skill ../
   cd ..
   ```

3. In Claude.ai:
   - Open **Settings** → **Capabilities** → **Skills**
   - Click **Upload**
   - Select the `vacancy-study.skill` file
   - The skill will appear in your skills list and auto-activate based on your prompts

## Cowork

Cowork uses the same filesystem layout as Claude Code. Either:

**A.** Install at workspace level by copying `skills/vacancy-study/` into your workspace's `.claude/skills/` directory.

**B.** Reference the GitHub repo in your workspace setup script:
```bash
git clone https://github.com/veeskelad/vacancy-study.git
cp -r vacancy-study/skills/vacancy-study .claude/skills/
```

## Claude API (Skills API)

Upload the packaged `.skill` file via the [Skills API](https://docs.claude.com/en/api/skills-guide).

```bash
curl -X POST https://api.anthropic.com/v1/skills \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -F "file=@vacancy-study.skill"
```

The returned skill ID can then be referenced in tool calls.

## Troubleshooting

### "Skill not appearing in /skills list"

- Did you restart Claude Code after install?
- Run `/skills reload`
- Check the path: `ls ~/.claude/skills/vacancy-study/SKILL.md` — file should exist
- Validate YAML frontmatter: open `SKILL.md`, make sure the `---` delimiters and indentation are correct

### "Skill activates but doesn't read references"

The skill's references are relative paths (`references/resume-intake.md`). Make sure the entire `skills/vacancy-study/` directory was copied, not just `SKILL.md`.

### "Permission denied" on macOS

If `~/.claude/skills/` doesn't exist yet:
```bash
mkdir -p ~/.claude/skills
chmod 755 ~/.claude/skills
```

### "On Claude.ai the .skill file is rejected"

- Make sure it's a valid ZIP — `unzip -l vacancy-study.skill` should list all files
- The top-level directory inside the archive should be `vacancy-study/`, containing `SKILL.md` at its root
- File size limit on Claude.ai is around 30MB — this skill is well below that

## Uninstalling

### Marketplace install

```
/plugin uninstall vacancy-study@vacancy-study
```

### Manual install

```bash
rm -rf ~/.claude/skills/vacancy-study      # user-scope
rm -rf .claude/skills/vacancy-study         # project-scope
```

### Claude.ai

In **Settings** → **Capabilities** → **Skills**, click the trash icon next to `vacancy-study`.
