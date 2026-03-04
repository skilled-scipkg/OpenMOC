---
name: openmoc-developer-guide
description: This skill should be used when users ask about developer guide in openmoc; it prioritizes documentation references and then source inspection only for unresolved details.
---

# openmoc: Developer Guide

## High-Signal Playbook
### Route conditions
- Route end-user run setup to `openmoc-getting-started` or `openmoc-simulation-workflows`.
- Route packaging/toolchain failures to `openmoc-build-and-install`.
- Route modeling details to `openmoc-inputs-and-modeling`.
- Route MPI/CUDA runtime scaling to `openmoc-parallel-hpc`.

### Triage questions
- Is the change in C++ core, Python API, or SWIG boundary layer?
- Does behavior change require new/updated regression tests?
- Is this an algorithm change (solver/tracking/CMFD) or interface/plumbing change?
- Are debug symbols/sanitizers needed to isolate failure mode?

### Canonical workflow
1. Identify impacted layer (C++, SWIG, Python API, tests).
2. Reproduce with the narrowest existing regression test.
3. Modify source and add/update targeted test coverage.
4. Rebuild with the minimal required flags (`--debug-mode` as needed).
5. Validate local behavior and cross-check adjacent solver workflows.

### Minimal working example
```bash
python setup.py install --user --debug-mode
python tests/run_tests.py -R "forward_simple_lattice|krylov_forward"
```

### Pitfalls and fixes
- Editing generated wrapper outputs is brittle; patch `.i` interfaces and source instead.
- API changes without harness updates can silently break regression expectations.
- Broad test sweeps before targeted repros slow debugging significantly.
- Mixed C++/Python behavior issues usually require checking SWIG typemaps and C++ signatures together.

### Convergence and validation checks
- Ensure at least one targeted regression test reproduces pre-fix failure and passes post-fix.
- Verify no drift in key solver observables for the touched subsystem.
- Re-run one neighboring test family (for example `forward` + `krylov` after solver edits).
- Confirm debug build and normal build both pass impacted tests when practical.

## Scope
- Handle questions about developer architecture, extension points, and contribution workflow.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/devguide/index.rst`
- `docs/source/developers.rst`
- `docs/source/devguide/software_design.rst`
- `docs/source/devguide/swig.rst`
- `docs/source/devguide/work_flow.rst`

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
- `src/Geometry.cpp`, `src/TrackGenerator.cpp`, `src/Solver.cpp` (core algorithm flow)
- `src/TrackTraversingAlgorithms.cpp` (transport/segmentation kernels)
- `openmoc/swig/openmoc.i` and typemap files (Python-C++ boundary)
- `setup.py` and `config.py` (build/packaging integration)
- `tests/testing_harness.py` and `tests/run_tests.py` (regression infrastructure)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" src openmoc tests`).
