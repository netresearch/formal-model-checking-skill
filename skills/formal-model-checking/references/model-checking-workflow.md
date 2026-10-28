<!-- SPDX-License-Identifier: CC-BY-SA-4.0 -->
<!-- SPDX-FileCopyrightText: Netresearch DTT GmbH -->

# Model-checking Workflow

## Contents

- [Choose a precise claim](#choose-a-precise-claim)
- [Preserve consequential steps](#preserve-consequential-steps)
- [Validate the checker result](#validate-the-checker-result)
- [Connect the model to implementation](#connect-the-model-to-implementation)
- [Validate runtime claims within scope](#validate-runtime-claims-within-scope)
- [Report and maintain evidence](#report-and-maintain-evidence)

## Choose a precise claim

Name the operation and property before selecting a tool. Examples include
"one committed mutation has its matching audit entry", "retries cannot debit
the same request twice", "two workers cannot own the same lease", and "accepted
configuration is not replaced by a stale snapshot". Make the identity, version
or ownership relation observable rather than relying on an informal label.

State the initial conditions, process/value bounds, failure model and external
assumptions. Distinguish safety from liveness: "never double-debit" differs
from "eventually completes despite retries". Delivery, recovery and termination
need their own progress and fairness assumptions. Exhausting a finite reachable
state graph covers its modeled execution lengths, including loops; bounding
the search depth alone covers only those explored prefixes. Neither result
extends automatically to more actors, values or omitted environment behavior.

Record model and input revisions, platform, checker version and configuration.
For external proof claims, inspect the exact theorem and trusted components.
Error freedom or behavior agreement does not necessarily establish termination.

## Preserve consequential steps

Keep pending state separate from committed state. Represent the steps that can
interleave: authorization/read, reservation, mutation, publication, commit,
retry, expiry or recovery, as relevant to the property. Include at least the
competing actors needed to expose the suspected ordering. Do not add actors or
failure modes unrelated to the claim merely to make the model larger.

A process crash runs no `catch` or `finally`. An exception that restores a
snapshot does not cover process loss, and restoring that snapshot after another
writer succeeds can itself lose an update. A bundled atomic success action
needs a real boundary that makes its substeps indivisible for this property.

Retain historical committed operations when the claim covers each mutation or
effect. An earlier unaudited change can disappear from a latest-value check
after another audited write or deletion. For retries, retain the request
identity and completed effects needed to detect duplicates. Abstract payloads
only when that abstraction preserves the rule being checked.

## Validate the checker result

A genuine trace violating the intended property supports a negative result and
can complete a design or bug-finding assignment. Do not hide it by weakening
the invariant, pruning its schedule or requiring an unrequested implementation.

For a **positive bounded-model claim**, retain all of the following:

| Evidence | What it establishes |
| --- | --- |
| Complete intended search | The configured property holds throughout the explored finite graph |
| Known unsafe variant | The intended invariant is violated with an interpretable counterexample |
| Reachability controls | Successful work and important failure/concurrency paths are possible |

Validate the negative control's exact failure classification. A nonzero exit,
syntax error, timeout, wrong invariant or missing tool is an incomplete or
failed check, not an intended counterexample. Parsing, exit zero or a partial
success log alone does not establish a completed positive search.

Guard against vacuity: if no operation can commit, "all commits are audited"
is true without testing useful behavior. Check relevant success, failure and
concurrent states explicitly. State constraints must not remove the disputed
behavior. Review the specification itself as well as the checker's result.

## Connect the model to implementation

When code exists, map each model action to the actual operation. Without code,
record implementation obligations instead of implying conformance was checked.

For database operations, inspect relevant connection identities, transaction
owner, savepoints, isolation, lock lifetime, relationship tables and anchors.
A nested savepoint release is not the outer commit. Releasing a chain lock
before that commit may expose the previous tip to the next writer. For queues,
leases and retry protocols, map acknowledgement, ownership, expiry, deduplication
and durable replay boundaries instead of forcing a database vocabulary.

Synchronous listeners can participate through the same transactional database
connection. External files, network calls and independently committed connections
do not share that commit. Post-commit publication alone does not promise
reliable eventual delivery. A durable delivery obligation needs its own recovery
contract; an outbox is one possible design, not a default requirement.

Keep scope explicit. A local adapter result does not cover custom adapters,
unrelated writers, hooks, counters or every path in the application. Check
secondary interactions exposed by a repair, such as stale compensation,
premature effects or IDs consumed by rolled-back inserts. Avoiding false alarms
must not conceal actual tampering; validate against the existing contract.

## Validate runtime claims within scope

When implementation verification or repair is requested, translate useful
counterexamples into regression tests on actual state and effects. Establish a
failing baseline or deliberately unsafe variant, then verify the correction.
Mocked `begin`/`commit`, lock or acknowledgement calls test orchestration, not
database, queue or process-loss behavior.

For a process-loss claim, start a separate worker and confirm an observable
checkpoint at the disputed boundary before injecting termination. Verify the
termination and durable state independently. A bootstrap error, ordinary
exception or worker that runs cleanup is not equivalent evidence. Use observable
barriers and bounded waits for concurrent tests; a timing-dependent test is not
systematic interleaving analysis.

If the assignment is design-only, report the model result and unverified runtime
questions. Do not edit application behavior or CI, and do not require runtime
work merely to finish a valid counterexample report. Reuse a relevant tested
protocol; do not impose a global lock or transaction coordinator unnecessarily.

## Report and maintain evidence

Separate checked property and bounds, implementation tests and input revision,
assumptions and excluded paths, incomplete checks and residual questions. A
model check is not a source/binary equivalence proof. Tests connecting model
actions to code do not establish a formal refinement proof either.

Within a maintained implementation assignment, keep the model-to-code mapping
and rerunnable checker command with the project. Integrate checker reruns when
the modeled boundary changes and CI changes are authorized. For design work,
the model, its reproducible result and explicit limits may be the final artifact.

## Primary sources

- [TLC: explicit finite-state model checking and completion options](https://docs.tlapl.us/using:tlc:start)
- [Lamport: safety, liveness and vacuity](https://lamport.azurewebsites.net/tla/tutorial/session9.html)
- [Doctrine DBAL 4.3: nested transactions and savepoints](https://www.doctrine-project.org/projects/doctrine-dbal/en/4.3/reference/transactions.html)
- [SQLite: atomic commit and its assumptions](https://www.sqlite.org/atomiccommit.html)

Verify framework and database details against the actual deployed versions.
