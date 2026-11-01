# Changelog

## 0.1.0 (2026-11-01)

- Initial release. Turns a pasted batch of prompts that do one multi-step job into a workflow map, routes each step against the live skillsmith.tech catalog (reuse, chain, new or inline), drafts a chain skill and writes a brief per new skill. Ships `scripts/catalog.py`, which reads the live skill index and brew table and marks installed skills.
- Built with draft-skill, validated with tune-skill's `check_skill.py`: 0 fail, 0 warn.
- Tests: 0 pass by execution, 0 fail, 12 reviewed-only (trigger cases judged against the descriptions, not run in a live agent session).
- `catalog.py` executed in live, `--offline` and `--json` modes: 51 published skills and 11 brews read from skillsmith.tech.
- Worked example, run by hand: an 8-prompt brand-identity prompt pack mapped to 8 steps, no brew match, 2 reuses, 5 new skills (colour and type merged), 2 inline steps, and a chain skill that passes `check_skill.py`. The run exposed three rule gaps (repeatability counted within the batch, no merge rule, straight-line maps only), which were fixed before this release.
