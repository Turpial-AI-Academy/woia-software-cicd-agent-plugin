---
name: cicd
description: Audit, select, implement, validate, troubleshoot, optimize, or prepare CI/CD for software repositories. Use when changing local gates, remote CI, release pipelines, deployment, test selection, Docker jobs, smoke checks, or rollback procedures.
license: MIT
metadata:
  author: Turpial AI Academy
  version: "0.5.7"
---

# CI/CD Operating Skill

Use this skill to establish a clean, reproducible delivery path with enough evidence for the repository's risks and maturity. Optimize for useful confidence and feedback, not for maximum controls or test volume.

## Operating flow

```text
DISCOVER -> DECIDE -> IMPLEMENT -> VALIDATE -> REPORT
```

For an existing repository, audit the affected delivery contract before editing. Preserve healthy standards and recommend the minimum sufficient change. Select the depth below before loading the audit procedure.

Preserve a healthy execution policy from the repository's actual constraints. `LOCAL`, `REMOTE`, and `HYBRID` are supported modes; none is the universal baseline. Load [references/EXECUTION-POLICY.md](references/EXECUTION-POLICY.md) when choosing/changing a mode or resolving a policy contradiction.

## Select scope and evidence

Use a bounded fast path for a local, understood change to a healthy existing gate map, execution policy or runbook. Anchor repository, branch, source HEAD and diff; locate the authoritative commands, their call graph and durable execution evidence. Inspect only the affected gate/configuration, its consumers and mandatory cross-cutting invariants. Amend the smallest coherent section and preserve unaffected valid gates, policy decisions, artifacts and evidence.

Use the deep path for a new delivery contract, unclear scope, missing durable required evidence, contradiction, drift or a failed invariant; toolchain/runtime/version/platform/service changes; required check, execution-policy or governance changes; public API/event/schema contracts, persisted data/migrations, auth/secrets/security/signing/trust, or release/deployment/rollback/availability risk. A bounded selection must never weaken required gates or substitute local PASS for required remote/provider/production evidence.

Load [references/AUDIT.md](references/AUDIT.md) for a new/broad audit, unfamiliar or unhealthy conventions, or those deep-path triggers. Load implementation, testing, optimization, security and release references only when the affected operation needs them. References remain authoritative; a healthy bounded amendment does not replay every inventory or the full report template.

## Execution policy

| Mode | Required validation runs |
|---|---|
| `LOCAL` | On the developer or agent machine; no remote CI is required. |
| `REMOTE` | In the repository's remote CI environment where governance, risk, or delivery requirements need it. |
| `HYBRID` | Fast local feedback plus remote checks required for integration or release. |

Keep provider configuration as an adapter to repository-owned commands. Do not impose a provider, and do not treat remote CI as inherently wrong or exceptional. Preserve a healthy existing policy unless the audit finds a concrete reason to change it.

## Logical operations

Keep the contract task-runner neutral:

```text
bootstrap
doctor
ci:fast
ci:extended        # when additional risk justifies it
release:check      # for releasable products
```

Optional operations include `jobs:local`, `release`, `deploy`, `smoke`, and `rollback`. Map these operations to the repository's existing stable interface, such as Mise, package scripts, Make, or a native script. Do not make a consumer adopt this plugin repository's authoring tools. Read [references/ADAPTERS.md](references/ADAPTERS.md) when mapping the contract.

- `bootstrap` prepares the repository-owned environment with locked dependencies.
- `doctor` checks environment readiness without exposing secrets or changing global settings.
- `ci:fast` runs inexpensive checks that catch common integration failures.
- `ci:extended` adds evidence for higher-risk changes when it has a distinct purpose.
- `release:check` validates one exact candidate SHA and its intended artifact or delivery path.

## Validation principles

- Run the cheapest useful check early; move expensive checks to the risk boundary that needs them.
- Preserve healthy existing standards and classify new requirements as `CORE`, `PROFILE`, or `REPOSITORY-SPECIFIC`.
- Select `LOCAL`, `REMOTE`, or `HYBRID` based on team size, product risk, governance, branch rules, infrastructure, external requirements, cost, platforms, and delivery model.
- Keep product profiles distinct. A web service, CLI, desktop app, package, local tool, and documentation project need different release evidence.
- Use Docker only when it adds isolation, services, or platform parity. `jobs:local` is optional.
- Avoid duplicate work, arbitrary coverage thresholds, unneeded OS matrices, and functional product changes made only to satisfy CI.
- Bind release evidence to the exact candidate. Document a practical smoke and recovery path where the product needs them.
- Reuse durable execution evidence only when its recorded source/input dependencies, gate commands, runtime, lockfile, services, platform and policy requirements still cover the current scope without drift. Setup/doctor checks may be amortized under those unchanged invariants; missing or invalidated evidence requires fresh execution.
- Distinguish reusable, invalidated and freshly executed/observed evidence from assumptions/inferences. Mutation invalidates affected proof; reconcile required invariants and rerun invalidated checks. Release acceptance still owns evidence for the exact candidate/artifact and selected environment.
- Establish the first causal error, verify repository/branch/SHA, and classify environment, generic maintenance/Factory, capability or provider failure before fixing that layer. Retain useful regressions and rerun only invalidated gates; never weaken checks or change product behavior to hide infrastructure failure.
- For performance changes, compare the same workload under comparable conditions and separate setup/queue time from execution time. Preserve valid measured baselines; configured timeout and guessed duration are not measurements.

Do not merge, force-push, tag, publish, or deploy without authorization. Follow [references/IMPLEMENTATION.md](references/IMPLEMENTATION.md) when implementation is requested.

## Load detail only when needed

- Audit or migration: [references/AUDIT.md](references/AUDIT.md), then [references/IMPLEMENTATION.md](references/IMPLEMENTATION.md).
- Execution policy choice: [references/EXECUTION-POLICY.md](references/EXECUTION-POLICY.md).
- Product-specific checks: [references/PROFILES.md](references/PROFILES.md).
- Test selection and costs: [references/TESTING.md](references/TESTING.md).
- Maturity and changing delivery needs: [references/MATURITY.md](references/MATURITY.md).
- Git branches, pull requests, or protection: [references/GIT-FLOW.md](references/GIT-FLOW.md).
- Security controls: [references/SECURITY.md](references/SECURITY.md).
- Failures or stuck checks: [references/TROUBLESHOOTING.md](references/TROUBLESHOOTING.md).
- Slow or duplicate gates: [references/OPTIMIZATION.md](references/OPTIMIZATION.md).
- Release, deployment, smoke, or rollback: [references/RELEASE.md](references/RELEASE.md).
- Core principles and requirement classification: [references/STANDARD.md](references/STANDARD.md).

## Report

State the bounded/deep path, current and selected execution policies, what each affected gate actually runs, findings and severity, changes made, commands and outcomes, measured durations, candidate SHA when relevant, and unverified or blocked items. Identify evidence reused, invalidated or freshly executed/observed and remaining uncertainty separately from assumptions.

When satisfying ASPS `cicd/v1`, amend the existing `docs/project/11-CICD.md` and prove its proportional repository-validation/release-check gate. This optional interoperability output does not require ASPS for standalone use.
