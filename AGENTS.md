<!-- SPDX-License-Identifier: CC-BY-SA-4.0 -->
<!-- SPDX-FileCopyrightText: Netresearch DTT GmbH -->

# Formal Model Checking Skill

This repository owns the Netresearch `formal-model-checking` skill.

## Layout

| Path | Purpose |
| --- | --- |
| `skills/formal-model-checking/SKILL.md` | Skill entry point |
| `skills/formal-model-checking/references/model-checking-workflow.md` | Detailed workflow |
| `skills/formal-model-checking/evals/` | Cases, selection queries and fixtures |
| `skills/formal-model-checking/agents/openai.yaml` | Native Codex metadata |
| `tests/test_model_checking_fixture.py` | Independent offline fixture controls |
| `plugin.json` | Portable metadata and source version |
| `.claude-plugin/plugin.json` | Generated Claude manifest |
| `composer.json` | Composer distribution metadata |

## Commands

Run from the repository root:

```sh
python3 -m unittest discover -s tests -p 'test_*.py'
git diff --check
```

CI runs reusable Netresearch skill/eval validators and the same test glob.
Default tests use the standard library and make no model or grader calls.

## Rules

- Keep model properties, bounds and implementation evidence distinct.
- A valid counterexample may finish a design review.
- Preserve the assignment's implementation and external-action scope.
- The skill directory is the portable payload; its references travel with it.
- Call reusable Netresearch workflows at `@main`, as required by organization
  policy; keep third-party actions pinned to full commit hashes.
- Version changes start in `plugin.json`; regenerate the Claude manifest.
- Code is MIT; content is CC-BY-SA-4.0, attributed to Netresearch DTT GmbH.
