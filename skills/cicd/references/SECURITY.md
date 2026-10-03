# CI/CD Security Controls

Apply controls according to the repository's assets, threat model, governance, and delivery path. A control should reduce a concrete risk without adding unrelated work to every change.

## Baseline

- Use a reproducible dependency lock when the stack has external dependencies.
- Declare the repository's supported runtimes and tool versions using its chosen toolchain mechanism.
- Keep secrets outside Git and portable artifacts; do not print or cache them.
- Give local and remote automation only the permissions it needs.
- Pin or otherwise control third-party automation dependencies when the selected policy runs them.
- Keep credentials out of Docker mounts unless the isolated check requires them.

## Remote automation

For `REMOTE` or `HYBRID` policies, review trigger scope, token permissions, third-party actions, secret access, artifact retention, and runner isolation. Prefer repository-owned commands for validation and keep provider configuration as a thin adapter.

When remote checks are required, document who owns them and how the team recovers from runner or provider outages. A single unavailable runner should not become an unexplained permanent integration block.

## Docker and local isolation

When Docker adds value:

- Pin an image to the required runtime or service version.
- Limit mounts, ports, and environment variables to the check's needs.
- Use read-only source mounts when tests can write to temporary storage.
- Remove disposable containers after the check.
- Do not use a container to conceal an unsupported or broken host workflow.

## Expensive security analysis

SAST, SBOM generation, license review, dependency vulnerability analysis, image scanning, and supply-chain provenance may belong in `ci:extended`, `release:check`, a scheduled job, or a manual review. Place them where the risk and freshness requirements justify the cost. Do not invent arbitrary universal thresholds.

## Evidence

Routine gates should not modify tracked files. Release evidence should identify its candidate and artifact without storing secrets or unnecessary environment details.
