# Release, Deployment, Smoke, and Rollback

Release validation is different from daily CI.

## Candidate rule

Every formal release candidate must be identified by an exact commit SHA.

The evidence used to accept it must apply to that content.

## Release check

`release:check` should answer:

> Is this exact candidate ready for its intended distribution path?

Possible components:

- confirm clean/known candidate;
- `ci:fast` result;
- relevant extended validation;
- final build/package;
- package smoke;
- checksum/digest;
- release-specific compatibility checks;
- concise evidence receipt.

Do not execute deep suites multiple times simply because old wrapper commands chain into each other.

## Artifact identity

Prefer producing one artifact and promoting that artifact when the platform allows it.

Avoid validating one build and deploying a materially different rebuild without need.

## Web/SaaS

Typical flow:

```text
candidate
-> release:check
-> preview/staging
-> smoke
-> production promotion
-> health check
```

Production promotion may remain manual during V1.

## Desktop

Typical flow:

```text
candidate
-> release:check
-> installer/package
-> installation/launch smoke
-> checksum
-> tag
-> GitHub Release or distribution
```

Packaging does not need to run on every development commit.

## CLI/library/package

Typical flow:

```text
candidate
-> tests/build
-> package
-> package smoke
-> checksum when useful
-> tag
-> registry/release
```

## Documentation/standard

Typical flow:

```text
candidate
-> docs/schema/link validation
-> package/archive smoke
-> checksum
-> tag/release
```

Do not invent an application deployment.

## Post-deploy smoke

Keep smoke small and high-value:

- health endpoint;
- homepage/app boot;
- login if critical and safely testable;
- one primary action;
- CLI `--version`;
- installer launch.

Do not rerun the full pre-release suite post-deploy.

## Rollback/recovery

Define a practical path before production release.

Examples:

- promote previous Vercel deployment;
- redeploy prior server/container image;
- restore previous installer download;
- publish a patch/deprecate package;
- restore DB from a known procedure when schema risk exists.

Automation is optional during V1. Knowledge of the procedure is not.

## Publication authorization

Do not tag, publish, deploy production, or promote a release unless the task explicitly authorizes it or repository procedure clearly grants that action.

A release audit can stop after proving readiness.

## Evidence

Prefer a concise receipt:

```json
{
  "version": "1.0.0",
  "commit": "<sha>",
  "quality": "passed",
  "artifact": "<name>",
  "sha256": "<digest>",
  "deployment": "<id-or-null>",
  "smoke": "passed"
}
```

Do not store secrets or volatile environment data in evidence.
