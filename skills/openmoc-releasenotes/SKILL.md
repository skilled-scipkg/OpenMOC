---
name: openmoc-releasenotes
description: This skill should be used when users ask about releasenotes in openmoc; it prioritizes documentation references and then source inspection only for unresolved details.
---

# openmoc: Releasenotes

## High-Signal Playbook
### Route conditions
- Route active install/runtime breakages to `openmoc-build-and-install` or `openmoc-simulation-workflows`.
- Route CMFD/solver algorithm interpretation to `openmoc-theory-and-methods`.
- Route MPI/CUDA execution issues to `openmoc-parallel-hpc`.
- Route API-level adaptation work to `openmoc-api-and-scripting`.

### Triage questions
- Which version jump is being evaluated (from/to release)? (`docs/source/releasenotes/notes_0.1.*.rst`)
- Is the concern feature availability, behavior change, bug fix, or regression?
- Which subsystem is implicated (CMFD, TrackGenerator/geometry load, Python 3 compatibility, state I/O, GPU)?
- Do we need a minimal reproduction from `sample-input` or a regression from `tests`?

### Canonical workflow
1. Read `docs/source/releasenotes/index.rst` and relevant note files for the version interval.
2. Extract only behavior-relevant bullets (feature/fix) and tag by subsystem.
3. Map each tagged item to concrete source entry points and one focused regression test.
4. Reproduce with smallest matching test/input before broad test sweeps.
5. Escalate to commit-linked source files if docs wording is ambiguous.

### Minimal working example
```bash
# Identify behavior-relevant changes in notes
rg -n "CMFD|TrackGenerator|Python 3|store_simulation_state|GPUSolver" \
  docs/source/releasenotes/notes_0.1.*.rst

# Run focused regressions for impacted areas
python tests/run_tests.py -R "cmfd|geometry|krylov|runtime"
```

### Pitfalls and fixes
- Release notes in-tree stop at historical 0.1.x; they are not a complete current changelog. (`docs/source/releasenotes/index.rst`)
- Short commit IDs in notes require drilling into linked commits for full context. (`docs/source/releasenotes/notes_0.1.*.rst`)
- Some fixes are narrow (for example, CMFD coefficient logic, geometry string in track files); do not over-generalize impact.
- Python compatibility fixes in notes should be validated against actual import/runtime behavior in current environment.

### Convergence and validation checks
- Pair each claimed change with one targeted regression class (`cmfd`, `geometry`, `krylov`, `runtime`).
- Confirm numerical deltas against `results_true.dat` where available.
- For solver-related notes, verify both `k_eff` trend and iteration count behavior.
- For I/O/API notes, verify round-trip behavior (store/restore state, module imports).

## Scope
- Handle questions about documentation grouped under the 'releasenotes' theme.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/releasenotes/index.rst`
- `docs/source/releasenotes/notes_0.1.4.rst`
- `docs/source/releasenotes/notes_0.1.3.rst`
- `docs/source/releasenotes/notes_0.1.2.rst`
- `docs/source/releasenotes/notes_0.1.1.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `sample-input`
- `profile/models`

## Test references
- `tests`

## Optional deeper inspection
- `openmoc`
- `src`

## Source entry points for unresolved issues
- `src/Cmfd.h` / `src/Cmfd.cpp` (CMFD feature/fix mapping)
- `src/TrackGenerator.h` / `src/TrackGenerator.cpp` (track-file and geometry-string behavior)
- `src/VectorizedSolver.h` / `src/VectorizedSolver.cpp` (vectorized solver regressions)
- `openmoc/process.py` (state export/import behavior)
- `openmoc/opencg_compatible.py` and `openmoc/compatible/opencg_compatible.py` (compatibility-layer changes)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" openmoc src`).
