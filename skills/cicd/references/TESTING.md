# Test Selection and Gate Cost

Choose the smallest test set that provides confidence for the changed risks. Test count and coverage percentage are not goals by themselves.

## Classes

| Class | Evidence | Typical placement |
|---|---|---|
| `FAST` | Pure logic, parsers, validators, transformations, known regressions. | `ci:fast` when inexpensive and relevant. |
| `INTEGRATION` | Real boundaries such as application/database, API/service, CLI/filesystem, or persistence. | `ci:extended` or a risk-specific gate. |
| `E2E` | A few critical user or operator journeys across the running product. | Targeted extended or release validation. |
| `RELEASE` | Final package contents, install/launch smoke, version, checksum, compatibility, or release-only behavior. | `release:check` for the exact candidate. |

Classify by evidence, risk, service requirements, supported platforms, and measured cost. A test need not move physically between directories when a repository already has a reliable way to select it.

## Select by change

- Documentation or standard: links, schemas, examples, and package/archive checks.
- UI or application code: relevant unit tests, type/lint checks, and a useful build; add a small browser smoke for critical journeys when the product requires it.
- Database or persistence: schema, migration, and integration checks at the affected boundary.
- Infrastructure: configuration validation and targeted environment checks.
- Dependencies: locked installation, relevant tests, build, and a security review proportional to the change.

Do not run every E2E journey, every operating system, database restore, destructive migration, or deployment check on every change without a demonstrated need.

## Quality and coverage

- Give critical business, data, authentication, and payment paths regression tests where appropriate.
- Prefer reliable, high-value end-to-end journeys over a large fragile suite.
- Measure coverage when it helps identify untested code; do not impose a universal percentage without a product-specific reason.
- Do not remove tests merely because a suite is large. Show redundancy, low value, or an alternative source of evidence first.
- Do not weaken assertions, add broad retries, or increase timeouts to disguise a real failure.

## Time signals

These are prompts to investigate, not hard failure thresholds:

```text
ci:fast        0-2 minutes excellent; 2-5 healthy; above 5-8 investigate
ci:extended    under 10 minutes excellent; 10-15 healthy; 15-30 review
release:check  under 15 minutes excellent; 15-30 healthy; 30-45 review
```

Measure actual execution and separate setup or queue time. Do not report a configured timeout as a measured duration.
