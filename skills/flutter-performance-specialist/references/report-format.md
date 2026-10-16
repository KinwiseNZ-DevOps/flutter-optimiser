# Final Report Format

Contents: Sections · Rules

Report using these XML-style sections, in this order.

```
<summary>Brief description of the optimisation work completed.</summary>
<findings>Meaningful issues. Label each: confirmed | likely (needs measurement) | acceptable | optional future.</findings>
<changes>File/component changed and the purpose of each change.</changes>
<performance>Measured before/after evidence only. Include metrics actually measured.</performance>
<verification>
  - tests run
  - static analysis results
  - profiling performed
  - device / profile-mode testing performed
  - calculation regression checks (identical source data, before vs after)
</verification>
<not_changed>Significant areas inspected and deliberately left alone, with the reason.</not_changed>
<remaining_issues>Unresolved concerns, tooling limitations, items needing further profiling.</remaining_issues>
```

## Rules

- Do not invent metrics or percentages. If profiling was not possible, say so in `<performance>` and `<remaining_issues>`.
- Do not claim the application is "fully optimised". Performance is workload-, device-, and data-dependent.
- State exactly what was investigated, changed, measured, and verified.
