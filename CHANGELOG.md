# Changelog

All notable changes to this plugin are listed here, newest first. To cut a release, add your notes under "Unreleased", then run `python scripts/bump_version.py patch` (or `minor` or `major`). It updates the plugin version and moves those notes under the new version.

## [Unreleased]

## [0.4.0] - 2026-10-26

- The validator now checks the Codex manifests (`plugin.json`, `.agents/plugins/marketplace.json`) against the Claude ones.

- Added triggering tests (`evals/triggers.jsonl`): every skill has an obvious, a paraphrase and an unrelated query, checked by the validator and exported as `tool_used` graders.
- The always-on rules now default to `auto`, so they load only in projects that match the markers.

## [0.3.1] - 2026-10-23

- Added the rule to trust the user's description of their own system: check the real files, report facts only, and never argue then concede. It is in the always-on rules, the router hard rules, AGENTS.md and the agent profiles.

## [0.3.0] - unreleased

- Initial skills.
