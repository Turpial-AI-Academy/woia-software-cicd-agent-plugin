# CI/CD Execution Policy

Choose where required validation runs as part of the repository audit. `LOCAL`, `REMOTE`, and `HYBRID` are equally valid policy modes. Select the minimum mode that satisfies the product, team, governance, and delivery requirements.

For a bounded amendment, preserve a healthy documented policy and inspect only affected requirements plus mandatory controls. Reopen the full decision when policy/governance/required checks, runtime/platform/services, security, delivery or availability changes, or when evidence is missing, contradictory or drifted. Reuse durable decisions while their premises remain valid; current required remote/provider/production evidence cannot be inferred from a local result or an old narrative.

## Modes

| Mode | Required checks | Suitable when |
|---|---|---|
| `LOCAL` | Developers or agents run the repository-owned gates before integration. No remote CI status is required. | A small or personal project can follow the procedure; local infrastructure covers its platforms and risk; no policy requires a remote result. |
| `REMOTE` | The remote CI environment runs the checks required for integration or release. Local checks may remain available for feedback. | Governance, regulation, branch rules, isolated credentials/services, or external delivery controls require a centrally recorded result. |
| `HYBRID` | A quick local gate provides early feedback and remote checks enforce the controls that need a shared or controlled environment. | A team benefits from local iteration and requires shared checks, reviews, or controlled release evidence. |

Remote CI can use hosted or self-hosted infrastructure. Choosing `REMOTE` or `HYBRID` does not select a provider by itself.

## Decide from evidence

Inspect these factors before choosing or changing a mode:

- Existing local and remote gates, their reliability, duplication, and ownership.
- Team size, contributor access, review practice, and ability to run required checks consistently.
- Product impact, data sensitivity, threat model, regulated controls, and recovery cost.
- Organization policy, branch protection, required reviews or checks, and audit evidence.
- Available OSes, architectures, services, secrets, networks, and release credentials.
- Delivery model, deployment provider requirements, artifact promotion, and smoke checks.
- Runner availability, queue time, maintenance burden, usage cost, and outage behavior.

Apply this order:

1. Preserve explicit organization, product, legal, and platform requirements.
2. Keep healthy existing controls unless the audit identifies a concrete failure or cost.
3. If required evidence must be centrally recorded or isolated from developer machines, choose `REMOTE` or `HYBRID`.
4. If collaborators need shared enforcement but benefit from local fast feedback, choose `HYBRID`.
5. Choose `LOCAL` when the repository can meet its integration and release requirements with local gates and no remote policy applies.
6. Check that the selected mode has a practical outage, access, and recovery path.

Do not infer the mode from repository size alone. A small regulated service may need `REMOTE`; a larger low-risk documentation repository may work with `LOCAL` or `HYBRID`.

## Keep policy separate from implementation

The execution policy states where required checks run. The task adapter maps logical operations to Mise, package scripts, Make, a native script, or another existing interface. The CI provider adapter invokes those repository-owned operations.

Keep these contracts stable where they apply:

```text
bootstrap
doctor
ci:fast
ci:extended
release:check
```

A remote provider should call the same repository-owned commands used locally. Keep provider YAML or configuration limited to triggers, environment setup, permissions, caching, and artifact handling; do not make it a second implementation of lint, tests, builds, or release checks.

## Branch rules and status checks

- Do not require a remote status check when the selected policy is `LOCAL`.
- For `REMOTE` and `HYBRID`, require only checks that are stable, owned, and necessary to protect the integration path.
- Include the check's runner availability and outage recovery in the policy decision.
- Keep review and force-push protections independent from CI execution when the platform supports that distinction.

## Record and revisit

Document the selected mode, why it fits, which gates are required, where they run, and any provider-specific constraint. Revisit the decision when team size, risk, governance, platform support, or delivery changes. Do not migrate modes merely for consistency with another repository.
