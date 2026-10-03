# CI/CD Troubleshooting

Diagnose the failing layer before changing the pipeline.

Identify the first causal error and verify repository, branch and exact SHA before repairing. Classify environment, generic maintenance/Factory, capability and provider failures; repair the owning layer, preserve useful regressions and rerun only invalidated evidence plus mandatory gates. A green retry without root-cause evidence is insufficient.

## Universal decision tree

```text
Failure
  │
  ├─ environment/tool unavailable? -> doctor/toolchain
  │
  ├─ repository-owned gate fails?   -> code/config in that environment
  │
  ├─ only one selected environment fails? -> compare runtime, services, secrets, and permissions
  │
  ├─ CI passes, deploy fails?       -> release/provider
  │
  └─ deploy succeeds, app fails?    -> smoke/runtime/rollback
```

## 1. Environment failure

Run or inspect `doctor` first.

Check:

- pinned runtime;
- `mise` resolution;
- package manager;
- frozen lockfile;
- required services;
- PATH/shims;
- permissions;
- platform mismatch.

Do not change application code to fix a broken toolchain.

## 2. `ci:fast` fails locally

Run the failing sub-check directly if available.

Determine whether the failure is:

- syntax/lint;
- type;
- deterministic test;
- build;
- generated file expectation;
- stale local environment.

Fix the root cause. Do not disable the check solely to obtain green status.

## 3. Local passes, retained remote CI fails

Compare:

```text
runtime
OS
architecture
package manager
lockfile behavior
working directory
environment variables
secrets
network access
service availability
filesystem case sensitivity
line endings
```

Also inspect runner image changes and action versions.

## 4. Required remote runner queued/offline

Treat this as infrastructure, not product failure.

Check:

- runner online status;
- service/process;
- labels;
- repository/org access;
- capacity/concurrency;
- billing/hosted-runner availability.

Do not rewrite tests to solve an offline runner.

If a required check depends on one unreliable runner, report the outage and recovery path as an operational design risk. Revisit the execution policy with its owner when availability no longer meets the requirement.

## 5. Recursive `mise`/shim issue

Inspect:

- which executable is resolved;
- PATH ordering;
- shims versus actual binary;
- task invoking `mise` through a shim that points back to itself;
- repository instructions prohibiting bypasses.

Do not silently use an alternate executable or change system permissions if repository policy says to stop on that boundary.

## 6. Docker failure

First ask whether the failing gate needs Docker.

If not, remove Docker from that path.

If yes, check:

- Docker Desktop/service running;
- context;
- image availability;
- bind mounts;
- filesystem sharing;
- network/ports;
- architecture;
- container exit code;
- cleanup from prior runs.

## 7. Flaky test

Procedure:

1. reproduce;
2. rerun once to establish flakiness;
3. identify timing/network/shared-state cause;
4. quarantine from the fast gate only when necessary and visible;
5. create/track the fix;
6. return it after repair.

Do not configure many retries until green.

## 8. Cache issue

Run without cache.

If the clean run succeeds, inspect cache key inputs and restore scope.

Cache must optimize the pipeline, not determine correctness.

## 9. Provider check stuck

Examples: Vercel or another deployment integration permanently queued.

Determine:

- whether the integration is still intentional;
- whether the repository is actually connected to a project;
- whether the check is required;
- whether billing/permissions/provider status block it.

Remove obsolete provider checks when the selected policy and delivery requirements no longer need them. Do not remove a required check without resolving its policy ownership.

## 10. Release evidence stale

Verify that evidence references the current candidate SHA.

Historical green evidence is not proof for a newer commit.

Do not mark release-ready until the current candidate has appropriate evidence.

## 11. CI dirties working tree

Identify generated tracked files.

Move routine receipts to ignored/temp/artifact storage. Generate versioned evidence only intentionally during a release acceptance step.

## 12. Suite timeout

Do not immediately increase timeout.

First determine:

- actual runtime;
- hung test versus slow test;
- duplicate work;
- service startup cost;
- serializable versus parallel work;
- whether that suite belongs in the current gate.

Configured maximum timeout is not measured duration.
