# Where this skill could be listed

Nothing here has been submitted. Rules read on 2026-11-06; re-read each CONTRIBUTING file
before submitting, they change.

## 1. skills.sh (Vercel skills directory)

- URL: https://skills.sh
- Rule: no submission form. The docs say the leaderboard "ranks skills based on anonymous
  telemetry data collected from the `skills` CLI" (https://skills.sh/docs). Listing follows installs.
- Not verified: whether the CLI finds a skill in the `usedby/` subfolder of this repo. Test
  `npx skills add ckrzento/usedby-skill` once before advertising it.
- Ready to paste (README or launch post):

```text
npx skills add ckrzento/usedby-skill
```

## 2. ComposioHQ/awesome-claude-skills

- URL: https://github.com/ComposioHQ/awesome-claude-skills
- Rule (CONTRIBUTING.md): fork, branch `add-skill-name`, add the skill folder with its `SKILL.md`
  inside their repo, add a README line in alphabetical order in its category (Business &
  Marketing, Communication & Writing, Creative & Media, Development, Productivity & Organization),
  commit and PR titled "Add [Skill Name] skill". No emojis.
- Fit: good. Category: Development (or Business & Marketing).
- Ready to paste:

```markdown
- [usedby](./usedby/) - Ask usedby.ai which companies publicly use which AI tools, with a source and proof level per line, when choosing between tools.
```

## 3. spencerpauly/awesome-cursor-skills

- URL: https://github.com/spencerpauly/awesome-cursor-skills
- Rule (README): read CONTRIBUTING.md, open a PR with the link, a short description and the
  category. External skills link to their repository.
- Fit: good for the Cursor audience.
- Ready to paste:

```markdown
- [`usedby`](https://github.com/ckrzento/usedby-skill) - Checks which companies publicly use a tool (usedby.ai, sourced and proof-levelled) before you pick one.
```

## 4. VoltAgent/awesome-agent-skills

- URL: https://github.com/VoltAgent/awesome-agent-skills
- Rule (CONTRIBUTING.md): public working repo, name with an author prefix, description of 10
  words or fewer, entry at the end of the matching "Community Skills" subcategory, PR title
  `Add skill: author/skill-name`. "Brand-new skills are not accepted": needs real adoption first.
- Fit: later, once the skill has installs or stars.
- Ready to paste:

```markdown
- **[ckrzento/usedby](https://github.com/ckrzento/usedby-skill/tree/main/usedby)** - Which companies publicly use which AI tools, with sources
```

## 5. travisvn/awesome-claude-skills (poor fit today, listed for completeness)

- URL: https://github.com/travisvn/awesome-claude-skills
- Rule (CONTRIBUTING.md): one skill per PR, entry `- **[Name](link)** - description`. PRs for
  skills under 10 stars "will be closed automatically". "SaaS-based skills are unlikely to get
  reviewed". PRs "explicitly generated / submitted with AI-assistance" are closed.
- Fit: poor. This skill calls a service (usedby.ai). Skip unless the rules change.
- Ready to paste (if ever):

```markdown
- **[usedby](https://github.com/ckrzento/usedby-skill)** - Answers "which tool do companies like mine use?" with public, sourced usage evidence from usedby.ai. No key.
```
