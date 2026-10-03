# CI/CD Audit Procedure

Use the relevant part before modifying an existing repository. For a bounded amendment to a healthy existing gate map/policy/runbook, anchor source state, inspect affected commands/call graphs and required invariants, and preserve unaffected decisions and durable evidence. Do not replay all inventories or fill a new full report for a local change.

Use full discovery for a new contract, missing evidence, contradiction, drift, failed invariant or material toolchain/platform, governance/required-check, public contract, persistence, security or release/deployment/rollback risk. Those triggers require the relevant detailed policy and fresh affected evidence.

## 1. Establish repository state

Record, without changing anything:

- current branch and HEAD SHA;
- clean/dirty working tree;
- default branch if known;
- local versus remote state if remote access is available;
- current `LOCAL`, `REMOTE`, or `HYBRID` execution policy, when documented;
- repository product type;
- platforms actually supported.

Do not call a dirty working tree a CI failure. Preserve unrelated user changes.

## 2. Inventory toolchain

Inspect:

- `mise.toml` or equivalent runtime declarations;
- Node/Bun/Python/Rust/etc. versions;
- package manager;
- lockfiles;
- Docker and local services;
- environment files and secret conventions;
- build/package scripts.

Determine whether tools are pinned and installs are locked.

## 3. Inventory quality commands

Find all commands that look like:

```text
quality
quality:ci
quality:dev
check
validate
full
jobs:local
test
test:ci
build
release
publication
```

For each command, map the full call graph.

Example:

| Command | Executes | Calls another gate? | Candidate role |
|---|---|---|---|
| `quality` | lint + types + unit | no | `ci:fast` |
| `jobs:local` | quality + DB integration | yes | integration only |
| `full` | quality + integration | yes | `ci:extended` |

Identify exact duplication rather than relying on names.

## 4. Inventory tests

Classify by cost and risk:

```text
FAST
INTEGRATION
E2E
RELEASE
```

Record counts only as context. More important:

- actual or historical duration;
- services required;
- platform required;
- failure history;
- criticality;
- whether another gate already runs the same test.

Do not recommend removing a suite purely because it is large.

## 5. Inventory CI execution and remote controls

Do not infer the required execution policy from the presence or absence of a workflow. If remote CI exists or the product, organization, branch rules, or delivery model requires it, treat it as part of the current system. For each workflow record:

- trigger;
- runner;
- timeout;
- concurrency/cancellation;
- permissions;
- setup/install work;
- command invoked;
- artifacts/evidence;
- whether it duplicates local gates;
- last known successful run when verifiable.

Also determine whether each remote workflow is required under the current policy. Look for duplicate `push` + `pull_request` runs on the same change.

## 6. Inventory branch rules

When access permits, determine:

- protected branch/ruleset;
- required checks;
- required reviews;
- strict update requirement;
- force-push/deletion protection.

Do not recommend adding every available branch rule. Evaluate whether team size, product risk, governance, and the selected execution policy justify it.

## 7. Inventory release/CD

Determine what "delivery" means for the repo:

```text
web deployment
service deployment
container image
CLI/package publish
desktop installer
GitHub Release
archive handoff
manual distribution
none
```

Find:

- candidate selection;
- exact SHA linkage;
- build/package;
- checksums;
- publication/deploy;
- smoke;
- rollback;
- provider checks such as Vercel.

## 8. Identify problems in this order

1. broken or unavailable infrastructure;
2. contradictory sources of truth;
3. duplicated work;
4. unnecessary work;
5. missing reproducibility;
6. missing fast feedback;
7. missing release safety;
8. security baseline gaps;
9. optimization opportunities.

## 9. Classify each requirement

Use:

```text
CORE
PROFILE
REPOSITORY-SPECIFIC
```

This prevents special-case logic from becoming global policy.

## 10. Select policy and produce a minimal-delta recommendation

Choose `LOCAL`, `REMOTE`, or `HYBRID` using team size, risk, governance, branch rules, infrastructure, external requirements, cost, platforms, and delivery model. Read `EXECUTION-POLICY.md` for the decision procedure. Preserve a healthy existing policy unless evidence supports changing it.

Recommend the fewest changes that create this contract:

```text
bootstrap
doctor
ci:fast
ci:extended
release:check
```

Retain aliases when useful.

## 11. Evidence discipline

Separate:

- reusable durable execution evidence with unchanged source/input dependencies, commands and environment/policy invariants;
- evidence invalidated by mutation, drift or expired validity;
- freshly executed/observed evidence for affected and mandatory checks;
- verified current facts;
- historical evidence;
- configured timeouts;
- measured duration;
- assumptions.

Never report a timeout as actual runtime.
Never imply a past successful run covers the current SHA unless it does.
Verify actual coverage and recorded inputs before reuse. Amortize setup/doctor only while runtime, lockfile, service and platform invariants remain unchanged and no drift is observed. Exact release acceptance still requires proof for its own candidate/artifact.

## Audit output

Amend the existing report for bounded work; use `../assets/AUDIT-REPORT.md.template` when creating a new full audit or when its sections are materially affected.
