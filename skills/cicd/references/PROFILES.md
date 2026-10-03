# CI/CD Product Profiles

The CORE contract stays stable. Profiles add only product-specific delivery behavior.

## CORE — all software repos

Expected baseline:

```text
runtime/toolchain declaration
lockfile
bootstrap
doctor
ci:fast
ci:extended when justified
release:check when releasable
secrets outside Git
concise documentation
execution policy selected from repository requirements
```

The operating contract does not prescribe where checks run. Choose `LOCAL`, `REMOTE`, or `HYBRID` separately using `EXECUTION-POLICY.md`.

## WEB / SaaS

Typical extra concerns:

- frontend/backend build;
- environment configuration;
- preview/staging;
- DB migration safety;
- web smoke;
- deployment provider state;
- rollback to prior deployment.

Do not make provider preview checks mandatory if the provider integration is not actually part of the delivery model.

## SERVICE / API

Typical extra concerns:

- API contract/integration tests;
- DB/services;
- container/service package when applicable;
- health endpoint;
- migration and rollback procedure.

## DESKTOP

Typical extra concerns:

- target OS is part of the product contract;
- packaging/installer belongs near release;
- installer smoke;
- artifact checksum;
- distribution channel.

Do not test unsupported OSes for symmetry.

## CLI

Typical extra concerns:

- build/package;
- `--help` / `--version` smoke;
- filesystem behavior;
- target platform matrix only when the CLI claims those platforms.

## LIBRARY / PACKAGE

Typical extra concerns:

- compile/type/export correctness;
- package contents;
- consumer smoke;
- registry publication.

## LOCAL TOOL

Typical extra concerns:

- supported local OS;
- configuration doctor;
- package/installer/archive;
- manual distribution may be entirely valid.

No web deployment is required.

## DOCUMENTATION / STANDARD

Typical extra concerns:

- links;
- schemas;
- examples/fixtures;
- package/archive integrity;
- GitHub Release or artifact handoff.

Do not introduce service infrastructure simply to look like an application repository.

## Profile selection rule

If a repository spans profiles, choose the primary product profile and add only the relevant secondary requirements.

Never combine all profiles into a universal mega-pipeline.
