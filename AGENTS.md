# AGENTS.md

Agent map and standing rules for this repository. Read this file as policy, not suggestions. Every rule carries its reason: when a situation is not covered, generalise from the reason rather than guessing.

## Ground truth

- Never speculate about code or files you have not opened. If asked about a file, read it before answering. Reason: confident-sounding claims about unread files are the most common agent failure and are indistinguishable from truth in review.
- File and directory names are not reliable evidence of what something does. Reason: naming drifts; contents do not.
- Documentation describes intent, not implementation state. Verify in the actual files before reporting status. Reason: docs lag behind code in every repo.

## Act only when told, and look before answering

- Do only what the user has asked for in this conversation. Do not start extra work, change files, or run side tasks on your own initiative. If you think something else is needed, say so in one line and wait. Reason: unrequested actions cost the user credit and trust, and they are hard to undo.
- Before you answer or change anything, analyse the current state of the repository for the task: open the relevant files, read what is already there, and check git status and recent history for work that is already done. Reason: answers built on assumptions waste credit and repeat finished work.
- No guesswork. Every statement about the repository must come from something you opened or ran in this session. If you have not checked it, either check it now or say plainly "I have not checked this". Never fill a gap with a plausible guess. Reason: unfounded information is worse than no answer, because the user acts on it.
- Do not redo work that is already in the repository. Find it first, report that it exists, and build on it. Never produce an answer and then correct yourself with "sorry, my mistake" when one look at the files would have prevented it.
- When a step asks the user to review, decide or approve something, first find the real thing (file path, exact wording, value or log line) and show it in that same step. Never write "decide X" or "approve Y" without showing X or Y. Reason: naming a task pushes the lookup work back onto the user.

## Scope of changes

- Make the smallest correct change that satisfies the task. Reason: every extra line is extra review burden and extra risk in a repository whose product is precision text.
- Concretely, do not:
  - Add docstrings, comments, or type annotations to content you did not change.
  - Add error handling, fallbacks, or caveats for scenarios that cannot happen (e.g. validators already covered by the tool). Validate at system boundaries only.
  - Create helpers or abstractions for one-time operations, or design for hypothetical requirements.
  - Refactor "while in here". Touch only what the task justifies.
  - Do not add comments beyond what the code genuinely needs. Keep the codebase mostly clean: comment only where the logic is not self-evident, and keep those comments short. Reason: comments that restate the code rot into lies; the code is read far more often than it is written.
  - Leave useless comments behind. When editing a file, delete comments in the lines you touch that are wrong, outdated, or say nothing the code does not already say. Reason: dead comments accumulate until readers stop trusting every comment, including the useful ones.
- Prefer editing the source of a rule over editing a restatement of it. Reason: copies drift.

## Accuracy and honesty

- Separate observed fact from inference, and mark anything uncertain.
- If a check cannot be run, say so explicitly instead of reporting success.
- Do not claim completion stronger than the evidence. Never write "fully validated" or "fully optimised"; state what was checked.
- Solve the problem, not the test. Behavioural cases measure quality; they do not define it. If a case or a check is wrong, say so rather than working around it.
- Treat the user's description of their own system as the working fact. Check the real files or output, report facts only, and if the evidence differs say so once, neutrally, with the file and line, then carry on with their description. Do not contradict first and concede later. Reason: the user built the system; arguing from assumption wastes their time and teaches them to repeat themselves.

## Verification

Before reporting any change done, run:

```bash
python scripts/skills_tool.py check
```

Checks: frontmatter, name/folder match, description length and trigger, body under 500 lines, every relative link resolves (including README.md and AGENTS.md), agent profile naming with no more than two profiles per host and exactly one `-workflow` main agent, agent `skills:` entries that exist and no `skills/<name>/SKILL.md` paths in agent bodies, plugin manifests valid and consistent with `LICENSE` and the profiles, at least 3 eval cases per skill, obvious, paraphrase and unrelated trigger queries per skill, no README.md inside a skill folder, catalogue not stale, no project-specific terms from `reference-content/strip-terms.txt`, and no em or en dashes in file names or in any text outside `reference-content/`. It also fails on leftover `<placeholder>` text and on setup-only paths that are tracked by Git or not ignored. Reason: these target the failure modes (drift, dangling links, stale catalogue, leaked scaffolding) observed in comparable repositories.

- The tool is necessary but not sufficient for content quality. For new or changed skill body text, also re-read the changed SKILL.md end to end. Reason: a validator checks structure; only reading checks that the instructions still make sense.

## Repository map (specific to this repo)

- `skills/<name>/SKILL.md`: the only source of skill content. Folder name equals frontmatter `name`.
- `plugin.json`: the portable Agent Plugins manifest for ChatGPT and Codex. `.agents/plugins/marketplace.json` exposes this repo as a local ChatGPT desktop marketplace.
- `.claude-plugin/plugin.json` and `marketplace.json`: the Claude Code plugin manifest and single-plugin marketplace. The `agents` array lists each Claude Code profile under `resources/agents/claude-code/`. Reason: without the key, Claude Code scans a top-level `agents/` folder and would load Codex briefs as subagents.
- `skills/flutter-performance-specialist/references/`: shared guardrails, report format, official sources. Link to them, never copy them.
- `resources/agents/`: optional host-native profile sources. Skill-specific profiles use the `flutter-performance-specialist-` prefix. `scripts/install_agents.py` installs profiles for Claude Code, GitHub Copilot and Codex; other compatible hosts receive skills only.
- README catalogue between the `<!-- catalogue:begin -->` and `<!-- catalogue:end -->` markers: generated by the tool. Never hand-edit. Reason: manual edits would be silently overwritten.
- Editing rules:
  1. New skill: copy the structure of an existing skill folder (folder name = frontmatter `name`; frontmatter, gate, steps, hard rules), add 3+ cases to `evals/cases.jsonl`, then run `python scripts/skills_tool.py catalogue`.
  2. Shared constraints live in `references/guardrails.md`; link, do not copy.
  3. Reference sibling skills by relative path only; the validator fails on dangling links.
  4. No Flutter facts that are not supported by the official docs listed in `references/official-sources.md`. Reason: an unsupported "fact" taught to agents becomes a bug shipped to users.
  5. No per-platform manifests until a platform needs one. Reason: premature manifests are maintenance surface.

- `hooks/`: the always-on rules and safety checks. `always-on.mjs` loads the rules at session start; `guard.mjs` runs the checks (dangerous commands, secrets, invented packages, licences, risky code patterns, suspicious prompts, a reminder when code changed but no checks ran) and `common.mjs` holds the shared helpers. These three files are identical in every plugin repository; change them in `template` first. `hooks/always-on.config.json` holds the id, name, phrase, rules path, auto-detect markers and per-check defaults, and `skills/flutter-performance-specialist/references/always-on-rules.md` is the only source of the rules. Keep that file under 320 words. Reason: it is paid for in tokens at the start of every session, and a copy would drift from the skills. Hooks fail open and only ask, warn or add context; they never edit files silently. The saved mode and check overrides live in the user's `~/.claude/.flutter-optimiser.mode` and `.flutter-optimiser.features.json`, never in this repo.

## Irreversible and shared actions

- Proceed freely for local, reversible actions: editing files, running checks, regenerating the catalogue.
- Ask first for anything destructive, hard to undo, or visible to others: deleting branches or history, force-pushes, changing repository visibility, pushing to shared sources. Reason: damage in a public repo cannot be recalled.

## Commit attribution

- Do not add `Co-Authored-By` trailers for AI assistants to commit messages. Add co-author attribution only when the user explicitly requests it. Reason: commit trailers affect GitHub's contributor attribution.

## Known deliberate states (dated)

- 2026-10-17: `evals/cases.jsonl` holds case definitions with expected behaviours only; no runner or baseline results exist yet (see README Status). If something "fails to prove" optimisation work, this is why, not a bug.
- 2026-10-17: skills are based on official Flutter docs and have not been tested against a live codebase yet.

Reason: recording deliberate temporary states stops future agents from treating them as incidents and "fixing" them.

## Anti-drift close-out

Before reporting done, confirm the change introduced no new local convention (a new section shape, a new file pattern, a divergent description style). If it did, either move it into the shared source (`references/guardrails.md`) or document the approved exception here. Reason: un-owned variations are how a standard silently dissolves.

## Close-out report

Finish every task with these five points, in this order:

1. What changed and why
2. Files changed
3. Validation run, and the result
4. Validation skipped, and why
5. Residual risks

## How to respond

- Reply in paragraphs: flowing prose, one idea per sentence, short paragraphs. Use lists only where the content is genuinely a list (the five-point close-out report, file paths, step sequences).
- Keep replies under 200 words. Go longer only when the user clearly asks for a thorough response. If more depth seems useful, offer it in one short line rather than writing it unprompted. Reason: long replies bury the decision the user needs to make.
- Write in British / New Zealand English throughout: behaviour, colour, organise, initialise, "-ise" endings, NZ date and spelling conventions. Reason: mixed spellings in one reply read as carelessness to a NZ audience.
- Simplify so a 16-year-old high school student could follow it: everyday words first, explain any unavoidable term the first time it appears, one technical term per sentence. Reason: if the explanation needs jargon to be understood, it has not explained anything.
- If the next step is one the user must do themselves (a setting to change, something to install, a click in a website), brief them first: say what is about to happen, what the step is for, and what success will look like. Then give exactly that one step, completely and simply. Reason: handing over five steps at once invites skipped and half-understood steps.
- After each user-side step, stop and wait for the user to say they are done before giving the next step. Do not stack several manual steps in one reply. Reason: one task at a time keeps confirmation honest and lets problems surface at the step that caused them.
- No padding, no restating the question, no caveats that carry no information.
- Skip em dashes and en dashes entirely and always. They are banned in every reply, every file, every commit message, prose and code comments alike. Every dash in text must be a plain hyphen (-). If a hyphen does not fit, use a comma or rewrite the sentence. Reason: nothing justifies an em dash; a banned character that appears anywhere is a single, checkable, zero-tolerance rule.
