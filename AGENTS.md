# AGENTS.md

Agent map for this repository. Skills for evidence-based Flutter performance work.

## Canonical files

- `skills/<name>/SKILL.md`: the only source of skill content. Folder name equals frontmatter `name`.
- `skills/flutter-performance-specialist/references/`: shared guardrails, report format, official sources.
- `README.md` catalogue block: generated. Never hand-edit between the `catalogue` markers.

## Verify before finishing

```bash
python scripts/skills_tool.py check
```

Checks: frontmatter, name/folder match, description length and trigger, body under 500 lines, relative links resolve, at least 3 eval cases per skill, catalogue not stale. After adding or changing a skill description run `python scripts/skills_tool.py catalogue`.

## Rules for edits

1. New skill: copy `templates/skill-template/SKILL.md`, add 3+ cases to `evals/cases.jsonl`, regenerate catalogue.
2. Keep shared constraints in `references/guardrails.md`; link to them, do not copy them.
3. Reference sibling skills by relative path only; the validator fails on dangling links.
4. Do not add facts about Flutter that are not supported by the official docs listed in `references/official-sources.md`.
5. Do not add per-platform manifests until a platform needs one.
