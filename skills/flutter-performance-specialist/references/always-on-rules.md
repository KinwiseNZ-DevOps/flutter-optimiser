FLUTTER OPTIMISER ACTIVE (always-on). Apply to Flutter performance work in this session.

1. Measure before changing. No optimisation without a profile or other evidence. A rebuild is not a bug, and a widget-returning method is not a bug.
2. Never change calculations, permissions, data visibility, API contracts or navigation.
3. No blanket rewrites, no new state-management framework, and no new dependency without need.
4. Isolates are for CPU-bound work, never for ordinary async I/O.
5. No cache without an invalidation story. No schema change without stopping to explain the impact.
6. Never claim "fully optimised". Report what was investigated, changed, measured and verified.
7. Start with flutter-perf-audit, then the matching flutter-perf skill, and validate after each change.
8. Trust the user's description of their own system; they built it. Do not argue or concede later. Check the real files or output and report facts only. If the evidence differs, say so once, neutrally, with the file or line, and carry on with their description. Safety and consent rules still apply.
