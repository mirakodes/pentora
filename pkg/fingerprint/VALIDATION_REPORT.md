## Performance Benchmarks

This section summarizes benchmark performance for the fingerprint resolver and validation runner. Use it to spot regressions and to communicate scaling behavior.

### How To Compare (Baseline vs Current)

- Baseline (multi-sample):
  - `go test -bench=. -benchmem ./pkg/fingerprint -run ^$ -benchtime=1s -count=6 > pkg/fingerprint/testdata/baseline.txt`
- Current and compare with benchstat:
  - `go test -bench=. -benchmem ./pkg/fingerprint -run ^$ -benchtime=1s -count=6 | benchstat pkg/fingerprint/testdata/baseline.txt -`

### Current Observations (developer notes)

- Resolver micro-benchmarks are sub-microsecond in common paths; telemetry adds expected overhead.
- Validation runner allocations dominate; opportunities include preallocation and reducing transient structures.
- Large dataset benches (~1k and 10k cases) indicate near-linear scaling in time and memory.

### Benchmarks of Interest

- `BenchmarkResolverSingleMatch`
- `BenchmarkResolverMultipleRules`
- `BenchmarkResolverNoMatch`
- `BenchmarkResolverVersionExtraction`
- `BenchmarkResolverWithAntiPatterns`
- `BenchmarkResolverWithTelemetry`
- `BenchmarkResolverConcurrent`
- `BenchmarkValidationRunner`
- `BenchmarkValidationMetricsCalculation`
- `BenchmarkRulePreparation`
- `BenchmarkValidationRunnerLargeDataset`
- `BenchmarkValidationRunnerLargeDataset10k`

### Recommendations

- Always run with `-benchmem` and report allocations (`b.ReportAllocs()`).
- Use `-count>=6` to ensure benchstat can compute confidence intervals.
- Update baseline only after intentional and validated changes.

