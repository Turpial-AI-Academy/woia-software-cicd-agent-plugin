# CI/CD Core Principles

This file is the compact core of the operating standard. Procedures and domain detail live in focused references and are loaded only when needed.

## Shared protocol

```text
DISCOVER -> DECIDE -> IMPLEMENT -> VALIDATE -> REPORT
```

Discover the repository's actual product, constraints, tools, gates, platforms, and delivery path. Decide the smallest strategy that meets its risks. Preserve healthy existing practices. Implement only when authorized. Validate the changed behavior and exact release candidate where relevant. Report evidence and remaining limits.

## Classify requirements

- `CORE`: behavior useful across software repositories, such as audit-first decisions, task-runner neutrality, proportional validation, and candidate-bound release evidence.
- `PROFILE`: controls needed for a product type, such as installer smoke for a desktop app or migrations for a service.
- `REPOSITORY-SPECIFIC`: a local contract, external integration, or governance rule that should not become universal without broader evidence.

Use the narrowest correct class. A requirement from one project does not become a global rule by repetition.

## Select sufficient evidence

- Select bounded reconciliation of a healthy gate map before inventory; deep discovery remains required for new contracts, missing proof, uncertainty, drift or material environment, governance, contract, state, security and delivery risk.
- Load detailed references/templates on their triggers and preserve unrelated valid artifacts and evidence.
- Reuse durable execution proof only with unchanged recorded inputs and required invariants; distinguish reusable, invalidated and fresh proof from assumptions. Amortize stable setup; mutation requires affected reruns and release acceptance still proves its exact candidate.
- Prefer the cheapest check that can catch a relevant failure.
- Keep a fast integration gate; add extended or release checks only when they add evidence.
- Choose tests by risk and cost. Do not require arbitrary coverage percentages.
- Use Docker for a stated isolation, service, or platform-parity purpose.
- Do not change product behavior solely to make a CI gate pass.
- Keep routine validation from dirtying versioned files.

## Select where checks run

Choose `LOCAL`, `REMOTE`, or `HYBRID` from team, product risk, governance, branch rules, infrastructure, external requirements, cost, platforms, and delivery model. Remote execution is not inherently exceptional; local execution is not a universal default. See [EXECUTION-POLICY.md](EXECUTION-POLICY.md).

## Focused procedures

- Repository audit: [AUDIT.md](AUDIT.md)
- Implementation: [IMPLEMENTATION.md](IMPLEMENTATION.md)
- Product-specific checks: [PROFILES.md](PROFILES.md)
- Delivery maturity: [MATURITY.md](MATURITY.md)
- Tests: [TESTING.md](TESTING.md)
- Task adapters: [ADAPTERS.md](ADAPTERS.md)
- Git integration: [GIT-FLOW.md](GIT-FLOW.md)
- Security: [SECURITY.md](SECURITY.md)
- Optimization: [OPTIMIZATION.md](OPTIMIZATION.md)
- Troubleshooting: [TROUBLESHOOTING.md](TROUBLESHOOTING.md)
- Release and recovery: [RELEASE.md](RELEASE.md)
