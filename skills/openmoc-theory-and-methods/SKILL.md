---
name: openmoc-theory-and-methods
description: This skill should be used when users ask about theory and methods in openmoc; it prioritizes documentation references and then source inspection only for unresolved details.
---

# openmoc: Theory and Methods

## High-Signal Playbook
### Route conditions
- Route install/toolchain questions to `openmoc-build-and-install`.
- Route geometry/material construction to `openmoc-inputs-and-modeling`.
- Route operational run-control and restart usage to `openmoc-simulation-workflows`.
- Route Python binding/module usage to `openmoc-api-and-scripting`.

### Triage questions
- Is the question about track generation, source iteration, convergence criteria, or Krylov eigenmodes?
- Is the run using reflective/vacuum boundaries (critical for some methods)? (`docs/source/methods/track_generation.rst`, `openmoc/krylov.py`)
- Is this 2D vs 3D transport and what discretization is chosen (angles/spacing)?
- Is CMFD acceleration involved and how are coarse groups/relaxation set? (`docs/source/usersguide/input.rst`)
- Is behavior mismatch numerical (tolerances/discretization) or implementation-level (solver routine semantics)?

### Canonical workflow
1. Map the user’s question to a methods chapter (`track_generation`, `eigenvalue_calculations`, `krylov`).
2. Translate equations/algorithms into concrete API controls (track spacing, azimuthal angles, `computeEigenvalue`, `computeSource`, `computeEigenmodes`).
3. Verify boundary-condition assumptions (especially vacuum requirement for IRAM).
4. Choose residual metric and tolerance consistent with objective.
5. Reproduce using a minimal sample input before deep optimization.
6. If docs are insufficient, inspect corresponding track/solver source implementation.

### Minimal working example
```python
import openmoc
from geometry import geometry

opts = openmoc.options.Options()
geometry.initializeFlatSourceRegions()

track_generator = openmoc.TrackGenerator(geometry, opts.num_azim, opts.azim_spacing)
track_generator.setNumThreads(opts.num_omp_threads)
track_generator.generateTracks()

cpu_solver = openmoc.CPUSolver(track_generator)
iram_solver = openmoc.krylov.IRAMSolver(cpu_solver)
iram_solver.computeEigenmodes(num_modes=2, solver_mode=openmoc.FORWARD)
print(iram_solver._eigenvalues)
```

### Pitfalls and fixes
- IRAM requires all boundaries to be `VACUUM`; otherwise `IRAMSolver` emits an error. (`openmoc/krylov.py`)
- `computeFlux(...)` omits scattering/fission updates and can show strong ray effects for low angle counts. (`docs/source/usersguide/input.rst`)
- `computeSource(...)` includes source updates but is documented as not thoroughly tested; validate carefully. (`docs/source/usersguide/input.rst`)
- FSRs must be initialized before tracking. (`docs/source/usersguide/input.rst`, `src/TrackGenerator.cpp`)
- Low azimuthal resolution or coarse spacing can mask method expectations; refine before diagnosing algorithmic bugs.

### Convergence and validation checks
- Use documented source-convergence tolerance ranges as a starting point (typically `1e-6` to `1e-4`). (`docs/source/methods/eigenvalue_calculations.rst`)
- Confirm residual type (`SCALAR_FLUX`, `TOTAL_SOURCE`, `FISSION_SOURCE`) matches objective. (`docs/source/usersguide/input.rst`, `src/Solver.h`)
- Check stability of `k_eff` and flux shape under angle/spacing refinement.
- For Krylov, compare dominant mode behavior against standard power iteration trend on the same geometry.

## Scope
- Handle questions about theoretical background and algorithmic methods.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/methods/index.rst`
- `docs/source/methods/track_generation.rst`
- `docs/source/methods/eigenvalue_calculations.rst`
- `docs/source/methods/krylov.rst`
- `docs/source/index.rst`

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
- `openmoc/krylov.py` (IRAM operator construction and boundary checks)
- `src/Solver.h` / `src/Solver.cpp` (residual types, fixed-source/eigenvalue routines)
- `src/TrackGenerator.h` / `src/TrackGenerator.cpp` (track generation flow and constraints)
- `src/TrackGenerator3D.h` / `src/TrackGenerator3D.cpp` (3D extensions and boundary handling)
- `src/TrackTraversingAlgorithms.h` / `src/TrackTraversingAlgorithms.cpp` (ray traversal details)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" openmoc src`).
