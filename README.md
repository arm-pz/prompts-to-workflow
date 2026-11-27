# prompts-to-workflow

An agent skill that turns a pile of prompts you keep pasting into a workflow, then decides how to split it into skills. Most prompt batches aren't one skill, and they aren't one skill per prompt either. They're a chain: research, then draft, then check, then publish. This skill finds the chain first. Then it checks every step against the live [skillsmith.tech](https://skillsmith.tech) catalog and reuses the skills that already exist (`tune-and-polish`, `wordsmith`, `findable` and the rest). It writes a thin chain skill that runs the steps in order, and briefs a new skill only where nothing in the catalog covers a step.

It has two confirmation points: the workflow map, and the split. No files are written on a split you haven't seen.

## Install

```
npx skills add arm-pz/prompts-to-workflow --skill prompts-to-workflow
```

Or upload `prompts-to-workflow.zip` in your skills settings, or copy the folder into `~/.claude/skills/` (Claude Code), `~/.qoder/skills/` (Qoder) or your agent's skills directory, then start a new session.

Needs Python 3 for `scripts/catalog.py` (standard library only; it reads the live index, falling back to installed skills when offline). Install [`tune-skill`](https://github.com/arm-pz/tune-skill) to validate the chain skill it writes, and [`prompts-to-skill`](https://github.com/arm-pz/prompts-to-skill) or [`draft-skill`](https://github.com/arm-pz/draft-skill) to build the new skills it briefs.

## Try it

- "Here's my prompt log from launching my last three skills. Make it a workflow."
- "[pastes 10 prompts] Which of these are already skillsmith skills, and what's missing?"
- "I do these seven steps by hand every Monday. Can it be one thing?"

## What you get

- `workflow.md`: the workflow map (the steps, what each one receives and hands on, its gates, which prompts showed it), the split table with evidence for each verdict, and install lines.
- `<chain-name>/SKILL.md`: a chain skill that runs the steps and stops at each skill's own gate.
- `briefs/<new-skill>.md`: one per genuinely new skill, ready for `prompts-to-skill`.

## Boundaries

- **Handles:** a batch of prompts that together do one multi-step job, turned into a workflow plus a split.
- **Does not handle:**
  - prompts that each become their own skill (`prompts-to-skill`)
  - a single idea (`draft-skill` or `skill-launch`)
  - an outcome described without prompts (`workflow-router`)
  - an existing skill (`tune-and-polish` or `tune-skill`)
  - packaging what it built (`prompts-to-skill-release`)
- If the chain is already a brew (for example "crawl, fix keywords, rewrite copy" is Brew 4 · Cold brew), it says so, hands off to `workflow-router`, and writes nothing new.

## Test results

Version 0.1.0:
- **Trigger cases:** 0 pass, 0 fail, 12 reviewed-only, 0 not run. All 12 were judged against the descriptions; none was run in a live session.
- **`catalog.py`:** executed in all three modes.
- **Worked example:** an 8-prompt brand-identity pack was run by hand. It produced 8 steps, 2 reused skills, 5 new skills and a chain skill that passes the validator.

## Version

See CHANGELOG.md. MIT licence.
