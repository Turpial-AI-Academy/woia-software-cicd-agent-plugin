# CI/CD Optimization Procedure

Optimize evidence production, not just wall-clock time.

## 1. Measure first

Collect where available:

```text
median ci:fast duration
p95 ci:fast duration
bootstrap duration
build duration
test duration
Docker/service startup duration
remote queue duration (when remote checks are selected)
ci:extended duration
release:check duration
flaky failure frequency
```

Separate local execution time from Docker/service startup. When remote checks are selected, separate queue time from execution time.

## 2. Build a duplication map

For every gate, list what it runs.

Look for:

- unit tests executed by `quality`, then by `jobs:local`, then by release;
- `doctor` executed separately and inside another gate;
- docs validation run by two scripts;
- build repeated in multiple jobs without need;
- conformance suites that overlap full test suites.

Remove duplication before optimizing individual commands.

## 3. Remove irrelevant work

Examples:

- docs-only changes should not start DB integration;
- Windows-only product does not need Linux/macOS matrices;
- a local desktop app does not need web deployment checks;
- Docker should not wrap purely static checks.

## 4. Use proportional selection

Add simple path/risk selection when it is reliable and saves meaningful time.

Avoid complicated selectors whose maintenance cost is higher than the work saved.

## 5. Improve bootstrap

Check:

- lockfile install mode;
- unnecessary browser/package downloads;
- duplicate setup across sequential jobs;
- unnecessary dev dependencies for docs-only jobs;
- repeated tool installation.

## 6. Cache after correctness

Cache dependency downloads or build inputs only after establishing deterministic clean builds.

Cache keys should include the inputs that make the cached content valid.

## 7. Parallelize independent work

Parallelize only when jobs are genuinely independent and setup duplication does not erase the gain.

Good candidates:

- platform-specific release packaging;
- independent integration domains;
- long test partitions.

## 8. Shard last

Shard only suites that are demonstrably expensive.

Before adding shards, measure 1/2/4/etc. workers and account for setup overhead.

Avoid eight shards for a suite that would finish quickly after duplicate work is removed.

## 9. Reduce flakiness

Flakiness creates hidden pipeline cost through retries and human distrust.

Track and prioritize:

- tests with repeated intermittent failure;
- shared state;
- network dependency;
- timing assumptions;
- nondeterministic fixtures.

## 10. Optimize the local integration gate

Prefer one stable local aggregate, `ci:fast`, over many commands that developers or agents must remember manually.

Keep `ci:fast` small; move expensive evidence to `ci:extended`, `jobs:local`, or `release:check`.

## 11. Review execution policy

Measure the cost and evidence of the selected `LOCAL`, `REMOTE`, or `HYBRID` policy:

- host-native execution time;
- remote queue and execution time when remote checks are selected;
- Docker/service startup and image pull/build cost;
- platform and credential requirements;
- runner maintenance and outage cost.

Change the policy only when the repository's team, risk, governance, infrastructure, or delivery needs justify it.

## 12. Expected report

For each optimization state:

```text
Before duration:
After duration:
Work removed:
Work moved to extended/release:
Coverage/evidence retained:
New complexity introduced:
Remaining bottleneck:
```

Do not claim improvement without measurement when measurement is possible.
