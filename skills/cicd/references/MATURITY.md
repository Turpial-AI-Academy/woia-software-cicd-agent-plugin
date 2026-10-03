# Delivery Maturity Signals

Maturity describes the delivery needs that have emerged. It does not prescribe `LOCAL`, `REMOTE`, or `HYBRID`; select that policy separately from actual constraints in [EXECUTION-POLICY.md](EXECUTION-POLICY.md).

## Early prototype

Start with environment diagnosis, a fast local gate, a clear manual delivery step, and a recovery plan appropriate to the prototype's impact. Add infrastructure only when it solves a current problem.

## First usable release

Maintain reproducible setup, a fast gate, release-candidate validation, and product-specific smoke and rollback steps. Add extended validation where risk justifies it. Choose remote checks when team, governance, or delivery requirements call for shared enforcement.

## Production service or product

Add only the controls needed for production operation, such as migration safety, monitoring, controlled environments, recovery automation, scheduled security work, or artifact provenance. Reassess access, ownership, and failure recovery.

## Regulated or centrally governed delivery

Use documented approvals, ownership, policy checks, audit evidence, provenance, or supply-chain controls when governance requires them. Select a remote execution environment when required evidence must be centrally recorded or isolated. Keep the control set tied to explicit obligations.

## Review signals

Review delivery data periodically when it can guide a decision:

- Median and high-percentile gate duration.
- Queue time compared with actual execution time.
- Repeated or flaky failures and duplicated work.
- Dependency setup, build, container, or service startup cost.
- Time to recover from a failed deployment and avoidable release rework.

Use measurements to choose an improvement. Avoid turning metrics into targets that encourage unnecessary tests, automation, or paperwork.
