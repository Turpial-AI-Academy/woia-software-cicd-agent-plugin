# Git Integration and Branch Rules

Choose an integration flow that fits the team and the selected execution policy.

## Branches and integration

- Prefer short-lived branches and a simple trunk-oriented flow when they improve review or coordination.
- Avoid permanent `develop`, `qa`, `integration`, or pre-release branches unless they solve a documented delivery need.
- Split long-running changes or use feature flags when incomplete work must integrate safely.
- A single maintainer may integrate directly when repository policy permits it and the required gate has passed.
- Do not require someone to review their own pull request only to satisfy a generic convention.

## Pull requests

For collaborative repositories, pull requests can combine review with required checks:

```text
branch -> pull request -> selected checks -> review -> merge
```

The required checks must match the chosen `LOCAL`, `REMOTE`, or `HYBRID` policy. Local policy uses a documented procedural gate; it must not wait on a nonexistent remote status check. Read [EXECUTION-POLICY.md](EXECUTION-POLICY.md) before changing required checks.

## Branch protection

For shared branches, consider preventing force-pushes and accidental deletion. Add reviews or required status checks when team practice, risk, or governance benefits from them.

Do not enable merge queues, signed-commit requirements, multiple approvals, mandatory deployment, or a large set of status checks without a concrete requirement. Avoid protections that make recovery or routine integration impossible when their required service is unavailable.

## Diagnose integration blockers

Separate code failures from policy and infrastructure failures:

- A failing repository-owned command is a code or configuration issue.
- A remote-only failure requires comparison of runtime, OS, architecture, dependencies, secrets, network, services, and working directory.
- A queued or offline runner is an infrastructure issue. Determine whether the selected policy requires that runner and identify the documented recovery path.
- A deployment provider failure is not automatically a CI failure.

Do not remove a required remote check simply because it is inconvenient. Revisit the policy with its owner when a check no longer provides required evidence or the operating cost exceeds its value.
