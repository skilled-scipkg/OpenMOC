---
name: openmoc-tests
description: This skill should be used when users ask about tests in openmoc; it prioritizes documentation references and then source inspection only for unresolved details.
---

# openmoc: Tests

## High-Signal Playbook
### Route conditions
- Route install/build failures to `openmoc-build-and-install`.
- Route runtime workflow configuration to `openmoc-simulation-workflows`.
- Route MPI/CUDA execution issues to `openmoc-parallel-hpc`.
- Route method-level interpretation to `openmoc-theory-and-methods`.

### Triage questions
- Is this a numerical regression, crash, import issue, or performance concern?
- Which class of tests reproduces it (`forward`, `fixed`, `cmfd`, `krylov`, `mpi`)?
- Do you need a single-test repro or filtered suite run?
- Is reference output outdated or is behavior truly changed?

### Canonical workflow
1. Reproduce with one smallest failing test first.
2. Re-run with focused regex filter in `tests/run_tests.py`.
3. Inspect harness comparison output from `tests/testing_harness.py`.
4. Map failure class to solver/geometry/CMFD source entry points.
5. Validate fix by rerunning the narrow class before broad suite.

### Minimal working example
```bash
python tests/test_forward_simple_lattice/test_forward_simple_lattice.py
python tests/run_tests.py -R "forward_simple_lattice|krylov_forward"
```

### Pitfalls and fixes
- Running full suite too early hides the first actionable failure.
- Comparing results across changed build modes (precision/MPI/CUDA) can yield false positives.
- Reference updates should only follow confirmed intentional behavior changes.
- MPI tests require matching MPI-enabled build/runtime environment.

### Convergence and validation checks
- Confirm failing test reproduces consistently before patching.
- Validate key observables (`k_eff`, iterations, selected flux/current outputs).
- Re-run nearest related tests (`cmfd`, `krylov`, `fixed`, `mpi`) after changes.
- Keep one serial baseline test green when evaluating parallel fixes.

## Scope
- Handle questions about test harnesses, regression strategy, and failure triage.
- Keep responses focused on reproducible diagnosis and validation.

## Primary documentation references
- `tests/readme.rst`
- `docs/source/devguide/work_flow.rst`
- `docs/source/usersguide/troubleshoot.rst`

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
- `tests/run_tests.py` (suite filtering and execution switches)
- `tests/testing_harness.py` (`_run_openmoc`, `_get_results`, `_compare_results`)
- `src/Solver.cpp` (core solve routines behind most regressions)
- `src/Cmfd.cpp` (CMFD-specific numerical behavior)
- `src/TrackGenerator.cpp` (track/segment generation issues)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" tests src openmoc`).
