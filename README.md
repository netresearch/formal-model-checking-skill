<!-- SPDX-License-Identifier: CC-BY-SA-4.0 -->
<!-- SPDX-FileCopyrightText: Netresearch DTT GmbH -->

# Formal Model Checking

Check a concurrency, persistence or recovery guarantee with a small finite-state
specification. Report the property, counterexamples, bounds and implementation
evidence separately. A Netresearch agent skill for engineering teams.

The package and catalog entry are proposed in their initial pull requests.
Marketplace installation becomes available when those changes merge.

## What this skill solves

A green checker can examine the wrong abstraction: a modeled operation can be
more atomic than its implementation, success can be unreachable, or only the
latest value can be checked while an earlier unaudited mutation is hidden.

## Why this is a skill (model delta)

Value categories: retro-born failure patterns, inference suppression and
anti-rationalization guards. The targeted failure is accepting a checker result
without validating its abstraction, reachability and negative controls.

The workflow is grounded in the bounded model and real failure tests in
[nr-vault's persistence change](https://github.com/netresearch/t3x-nr-vault/pull/480).
[Eval cases](skills/formal-model-checking/evals/evals.json) define expected behavior;
independent fixture tests check concrete model and database outcomes. These
checks do not measure a general agent-quality improvement.

## Use when

- An invariant depends on competing requests, retries or recovery.
- Process loss can interrupt a sequence of durable operations.
- A finite-state checker or TLA+/TLC result needs validation.
- A design needs systematic state exploration or a counterexample.

Choose a suitable formalism for the question. Ordinary bug fixes use the
project's testing workflow. A design review can finish with a counterexample.

## Use cases

- Explore whether an interrupted mutation can survive without its audit.
- Check competing workers that claim, retry and complete an operation.
- Investigate vacuous model success and invalid negative controls.
- Translate a useful trace into a regression when implementation work is requested.

## Expected outputs

- A precise property, model, configuration and explicit bounds and assumptions.
- A completed search or an interpretable execution violating the property.
- Valid negative controls and reachability evidence supporting positive claims.
- A model-to-code mapping and separately reported runtime evidence.
- Excluded paths and incomplete checks.

## Context requirements

- A concrete design claim, protocol or implementation boundary.
- Relevant source and storage semantics when implementation mapping is requested.
- A suitable available checker or interpreter to execute the chosen model.
- Python 3.11 or later for the repository's standard-library fixture tests.

## Example prompts

- "Can two workers acknowledge the same job after a lease expires? Check our protocol."
- "A worker can stop between saving a version and its audit. Explore the durable outcomes."
- "Review this TLA+ result: what do the invariant, bounds and comparison run establish?"

## Related skills

- [Security Audit](https://github.com/netresearch/security-audit-skill): security claims and audit boundaries.
- [TYPO3 Testing](https://github.com/netresearch/typo3-testing-skill): implementation and real database/process tests.
- [Enterprise Readiness](https://github.com/netresearch/enterprise-readiness-skill): review evidence and quality gates.

## Keywords

- formal verification
- model checking
- TLA+ and TLC
- concurrency
- persistence and recovery
- counterexamples and invariants

## Installation

After the initial skill and catalog pull requests merge:

```text
/plugin marketplace add netresearch/claude-code-marketplace
/plugin install formal-model-checking@netresearch-claude-code-marketplace
```

For source installation, copy `skills/formal-model-checking/` into the host's
skill directory. The payload includes its reference and Codex
`agents/openai.yaml` display metadata.

Composer package name: `netresearch/formal-model-checking-skill`.
Registry publication and tagged releases are separate steps.

## Classification

| Field | Value |
| --- | --- |
| action_level | runs_commands |
| risk_level | medium |

Checkpoints: none (justified — the properties depend on the selected protocol;
fixture-based evals cover the workflow rather than a universal repository checklist).

## Development

```sh
python3 -m unittest discover -s tests -p 'test_*.py'
git diff --check
```

CI uses Netresearch reusable skill, eval and test validators. Default tests use
the Python standard library and make no model or grader calls. Their model and
SQLite controls validate fixture sensitivity, with limits recorded in the tests.

## Contributing

Open a pull request against
[netresearch/formal-model-checking-skill](https://github.com/netresearch/formal-model-checking-skill).
Keep the skill self-contained and report executed checks. Distinguish fixture
validity, skill selection and agent outcomes. Update discovery metadata in the
[Netresearch catalog](https://github.com/netresearch/claude-code-marketplace)
through a related pull request.

## License

Code and configuration: [MIT](LICENSE-MIT).
Skill content and documentation: [CC-BY-SA-4.0](LICENSE-CC-BY-SA-4.0).
Copyright Netresearch DTT GmbH.

## German summary

Der Skill prüft konkrete Garantien für konkurrierende Abläufe, dauerhafte
Schreibvorgänge und Wiederanläufe mit kleinen Zustandsmodellen und benennt die
Grenzen von Modellprüfung und Implementierungsnachweisen.
