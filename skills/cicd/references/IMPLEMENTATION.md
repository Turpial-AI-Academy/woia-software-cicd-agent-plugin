# CI/CD Implementation Playbook

Use after completing the relevant scoped audit and establishing that implementation is requested. For a healthy bounded amendment, apply only affected phases and required cross-cutting invariants; preserve unrelated commands, policy and evidence. New contracts, contradictions, missing proof, drift or material safety/delivery changes require deep handling.

## Phase 1 — Preserve a baseline

Before edits:

- record current branch and SHA;
- confirm unrelated user changes;
- note current commands, local gates, and any existing remote workflows;
- avoid deleting old commands before replacements are validated.

## Phase 2 — Normalize the local contract

Create or map:

```text
bootstrap
doctor
ci:fast
ci:extended
release:check
```

Use the repository's existing stable task interface when possible. The CI/CD contract is task-runner neutral.

Read `ADAPTERS.md` before introducing or changing task-runner configuration. If another plugin or repository policy owns Mise or another task runner, defer to that source for syntax and toolchain details.

### Migration aliases

Example:

```text
quality          -> ci:fast
quality:ci       -> ci:fast
quality:full     -> ci:extended
jobs:local       -> environment-specific integration
```

Do not break developer muscle memory unnecessarily during migration.

## Phase 3 — Build `ci:fast`

Select the lowest-cost checks that catch common integration failures.

Typical composition:

```text
lint
+ typecheck
+ fast/unit tests
+ cheap build
```

Use path-aware or change-aware selection when the repository already has reliable support for it. Do not build a complex selector simply to avoid seconds of work.

## Phase 4 — Build `ci:extended`

Move expensive or context-specific checks here:

- integration;
- services;
- Docker parity;
- targeted E2E;
- migration validation;
- expensive build variants;
- broader regression suites.

Only include checks that add evidence beyond `ci:fast`.

## Phase 5 — Fix evidence behavior

Routine CI outputs should go to ignored/temp/artifact locations.

Do not make running a quality command modify a tracked "latest evidence" file.

For release evidence, capture only candidate-bound information worth preserving.
Classify reusable durable proof, invalidated proof, fresh execution and assumptions. Reuse setup and gate results only when recorded input/environment/policy invariants still cover the current scope. Rerun affected invalidated gates and independently prove the exact release candidate.

## Phase 6 — Implement the selected execution policy

Map the audited `LOCAL`, `REMOTE`, or `HYBRID` policy to the repository's gates. Keep the same repository-owned operations available to every environment that needs them.

For `LOCAL`, document the local integration procedure:

```text
doctor
ci:fast
```

For `REMOTE`, run required operations in the selected remote environment. For `HYBRID`, keep a quick local path and enforce only the required shared checks remotely.

Optional higher-cost local path:

```text
ci:extended
jobs:local
```

Use `jobs:local` only when Docker/local isolation adds evidence that the host-native gate does not provide.

When a remote adapter is required by the selected policy, use repository-owned commands rather than reimplementing the build in provider configuration. Add or change provider configuration only when implementation is authorized and the audit justifies it.

---

## Phase 7 — Integration policy

For `LOCAL`, require the documented local gate procedurally and do not add a remote required status check. For `REMOTE` or `HYBRID`, configure only the required, owned, reliable remote checks. Keep branch review and force-push protections separate from execution policy where the platform permits it.

## Phase 8 — Release path

Create `release:check` for the exact candidate SHA.

It should add evidence that normal CI does not provide, such as:

- final package;
- platform-specific smoke;
- checksum;
- release-only regression;
- artifact validation.

Do not run `ci:fast` three times through nested wrappers.

## Phase 9 — Delivery adapter

Choose the product profile in `PROFILES.md`.

Implement only the deployment/distribution behavior that product needs.

## Phase 10 — Validate

At minimum, where safe and requested, execute the repository-specific commands that implement:

```text
doctor
ci:fast
ci:extended
release:check
```

Run only operations that make sense for the repository and current task.

Run the gates required by the selected policy and record actual measured durations. Report unavailable providers or runners as infrastructure evidence, not as successful validation.

## Phase 11 — Documentation

Create/update a concise runbook. It should tell a new contributor or agent the **actual repository commands** for:

```text
Development:
  <ci:fast command>

Environment diagnosis:
  <doctor command>

Extended validation:
  <ci:extended command>

Release candidate:
  <release:check command>
```

Use `../assets/CICD-RUNBOOK.md.template` as a starting point.

## Phase 12 — Report

Explain:

- what changed;
- what was deliberately not changed;
- duplicated work removed;
- measured timings;
- tests preserved/moved;
- remaining risks;
- any optional provider configuration still required for release/deployment;
- whether existing remote CI was removed, retained, or deliberately left untouched.
- the selected `LOCAL`, `REMOTE`, or `HYBRID` policy and its reason.
