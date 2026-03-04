---
name: openmoc-usersguide
description: This skill should be used when users ask about usersguide in openmoc; it prioritizes documentation references and then source inspection only for unresolved details.
---

# openmoc: Usersguide

## High-Signal Playbook
### Route conditions
- Route build/install blockers to `openmoc-build-and-install`.
- Route deep modeling specifics to `openmoc-inputs-and-modeling`.
- Route production run-control and restart flows to `openmoc-simulation-workflows`.
- Route MPI/CUDA deployment and scaling to `openmoc-parallel-hpc`.

### Triage questions
- Is the request about setup, runtime controls, outputs, or troubleshooting?
- Is this a fresh script or an adaptation of an existing sample input?
- Are plotting/post-processing requirements interactive or headless?
- Which outputs are needed for validation (`k_eff`, fluxes, pin powers, stored state)?

### Canonical workflow
1. Start from user guide section matching task (`input`, `running`, `processing`, `troubleshoot`).
2. Run a known sample script with explicit options.
3. Inspect output metrics and convergence.
4. Persist simulation state when further analysis is required.
5. Escalate to source only when docs and examples disagree with behavior.

### Minimal working example
```bash
python sample-input/simple-lattice/simple-lattice.py -i 30 -a 8 -s 0.12 -c 1e-5
python tests/test_forward_simple_lattice/test_forward_simple_lattice.py
```

### Pitfalls and fixes
- Skipping sample-based validation increases time-to-diagnosis on custom models.
- Headless nodes need non-interactive plotting; `plotter.py` handles `Agg` fallback.
- Fixed-source setup order matters (set fixed source after tracks are generated).
- Tolerance/angle/spacing combinations strongly affect runtime and observed convergence.

### Convergence and validation checks
- Confirm residual decreases and final tolerance is met.
- Check `k_eff` stability with modest mesh/angle refinements.
- Validate restored state using `restore_simulation_state(...)` when restart workflow is used.
- Cross-reference behavior against one relevant regression test before large runs.

## Scope
- Handle questions across user-guide operations and practical run usage.
- Keep responses grounded in docs and runnable examples.

## Primary documentation references
- `docs/source/usersguide/index.rst`
- `docs/source/usersguide/beginners.rst`
- `docs/source/usersguide/input.rst`
- `docs/source/usersguide/running.rst`
- `docs/source/usersguide/processing.rst`
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
- `openmoc/options.py` and `openmoc/process.py` (runtime controls and post-processing)
- `openmoc/plotter.py` (rendering and headless behavior)
- `src/Geometry.cpp` / `src/TrackGenerator.cpp` / `src/Solver.cpp` (core user workflow internals)
- `src/boundary_type.h` (boundary enum mapping)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" docs/source/usersguide openmoc src`).
