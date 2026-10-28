---
# SPDX-License-Identifier: CC-BY-SA-4.0
# SPDX-FileCopyrightText: Netresearch DTT GmbH
name: formal-model-checking
description: "Use when checking finite-state specifications of concurrency, persistence, retries or recovery; when asked for model checking or TLA+/TLC; or when systematic exploration of interleavings and failures is needed for a specific invariant. Not for routine unit tests, ordinary bug fixes or whole-program proof claims."
license: "(MIT AND CC-BY-SA-4.0). See LICENSE-MIT and LICENSE-CC-BY-SA-4.0"
metadata:
  author: Netresearch DTT GmbH
  version: "0.1.0"
  repository: https://github.com/netresearch/formal-model-checking-skill
---

# Formal Model Checking

Check one precise design claim with a small executable state model. Keep that
result separate from evidence that implementation follows the model. This
workflow applies to stateful protocols, including concurrency, retries,
persistence and recovery; it does not require a database or TLA+.

## Workflow

Read [Model-checking workflow](references/model-checking-workflow.md) for the
applicable modeling, checker-control and implementation-evidence details.

1. **Scope the claim.** Name the property, initial states, actor/value bounds,
   assumptions and authorized deliverable. Distinguish safety from liveness.
   Record input revision, checker version and configuration.
2. **Preserve the real boundaries.** Separate pending and committed state.
   Include competing actors and failure/recovery steps when relevant. A crash
   runs no `catch` or `finally`; an exception is not a substitute. Retain history
   when the claim covers every operation. Justify any atomic model step.
3. **Execute and interpret.** A genuine counterexample is useful and can
   complete a bug-finding or design assignment. For a positive claim, require
   a complete search, a known unsafe variant violating the intended invariant
   with a trace, and reachable successes and important failure paths. A parse
   error, timeout, wrong invariant or unreachable success is not that evidence.
4. **Connect to code when it exists.** Map model actions to concrete boundaries
   and record unverified obligations. When implementation verification or
   repair is requested, reproduce counterexamples against actual state and
   effects, establish a failing baseline, and verify the correction. A process
   loss claim needs a separate worker, confirmed checkpoint and verified
   termination. Mocked boundary calls do not establish runtime behavior.
5. **Report honestly.** Separate model results, implementation tests, bounds,
   assumptions, exclusions and incomplete checks. Tests do not supply a formal
   refinement or equivalence proof. A finite exhaustive graph covers modeled
   execution lengths; a depth-limited search covers only explored lengths.

For design-only work, leave runtime conformance explicitly unverified; do not
edit application behavior or CI. Within a maintained implementation assignment,
keep the model-to-code mapping and a rerunnable check. Do not impose a global
lock, coordinator, outbox or unrequested deployment architecture.

## Verification

- Keep the counterexample or complete positive result with validated controls.
- Check that the specification states the intended rule and permits the
  successes, competing actions and failures under investigation.
- State the evidence boundary: model, runtime or a separately supplied proof.
- Stop at the requested outcome; routine tests and debugging do not acquire a
  model-checking requirement merely because this skill exists.

## Evaluation assets

The [eval rubric](evals/evals.json), [selection queries](evals/eval_queries.json)
and [self-contained fixture](evals/fixtures/formal-model-checking/README.md)
support evaluation. They are not required runtime tools or measured proof of
skill effectiveness.
