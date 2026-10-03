# Task Interface Adapters

`agile-cicd` owns CI/CD semantics, not the repository's task runner.

The logical contract is:

```text
bootstrap
doctor
ci:fast
ci:extended
release:check
```

Optional operations:

```text
release
deploy
smoke
rollback
jobs:local
```

## Selection rule

Use the repository's existing stable task interface when it is clear and maintainable.

Do **not** introduce Mise, npm scripts, Make, Just, Taskfile, PowerShell wrappers, or another runner only because this plugin exists.

If a separate plugin or repository standard owns toolchain/task-runner configuration, defer to that source for syntax, versions, installation, and environment setup.

## Mise adapter

Use when the repository already standardizes on Mise or explicitly adopts it.

```text
mise run bootstrap
mise run doctor
mise run ci:fast
mise run ci:extended
mise run release:check
mise run jobs:local   # only when the repository needs local isolation/parity
```

Template: `../assets/adapters/mise.toml.template`.

## package.json adapter

For Node/Bun repositories, package scripts may expose the contract directly.

```text
pnpm bootstrap
pnpm doctor
pnpm ci:fast
pnpm ci:extended
pnpm release:check
pnpm jobs:local      # optional
```

Equivalent npm/Bun invocations are valid.

Template: `../assets/adapters/package-json.scripts.json.template`.

## Make adapter

Example:

```text
make bootstrap
make doctor
make ci-fast
make ci-extended
make release-check
make jobs-local     # optional
```

Do not add Make only for naming consistency.

## Native script adapter

A repository may use a project-owned CLI or script:

```text
python scripts/ci.py fast
node scripts/quality.mjs fast
pwsh ./tooling/ci.ps1 fast
```

This is conformant when the operations are documented and reproducible.

## Local Docker adapter

Docker is an **optional local adapter** for `jobs:local` or a repository-specific integration command.

Use it when it proves a property that the native host gate does not, for example:

- Linux behavior from a Windows workstation;
- disposable DB/service integration;
- filesystem or network isolation;
- release-environment parity.

Do not make Docker a prerequisite for `ci:fast` unless the product itself genuinely requires container execution.

See `../assets/adapters/docker-jobs-local.md.template`.

## Remote CI adapter

Use a remote adapter when the selected `REMOTE` or `HYBRID` policy requires remote checks, or when an existing remote system is an explicit product or governance constraint. The policy choice does not prescribe GitHub Actions, GitLab CI, CircleCI, Azure Pipelines, or a particular runner.

Remote CI should call the same concrete repository commands used locally. Keep provider configuration to triggers, environment setup, permissions, caching, and artifact handling; do not duplicate lint, test, or build logic there.

## Migration

Legacy commands can remain as aliases:

```text
quality        -> ci:fast
quality:full   -> ci:extended
release-check  -> release:check
```

Prefer a small compatibility layer over unnecessary churn.
