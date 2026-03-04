---
name: openmoc-api-and-scripting
description: This skill should be used when users ask about api and scripting in openmoc; it prioritizes documentation references and then source inspection only for unresolved details.
---

# openmoc: API and Scripting

## High-Signal Playbook
### Route conditions
- Route compiler/install/environment issues to `openmoc-build-and-install`.
- Route geometry/material authoring details to `openmoc-inputs-and-modeling`.
- Route MPI/CUDA scaling and cluster execution to `openmoc-parallel-hpc`.
- Route algorithm derivations and method proofs to `openmoc-theory-and-methods`.

### Triage questions
- Is the task Python orchestration (`openmoc.*` modules) or SWIG/C++ binding behavior? (`docs/source/devguide/swig.rst`, `openmoc/swig/openmoc.i`)
- Is the run mode eigenvalue, fixed-source, or eigenmodes (Krylov)? (`docs/source/usersguide/input.rst`, `openmoc/krylov.py`)
- Which solver backend is intended (`CPUSolver`, `CPULSSolver`, `GPUSolver`)? (`docs/source/devguide/software_design.rst`)
- Which outputs are required (fluxes, sources, fission rates, plots, HDF5/pickle state)? (`docs/source/usersguide/processing.rst`)
- Is execution headless/cluster (plot backend constraints)? (`openmoc/plotter.py`)

### Canonical workflow
1. Parse runtime flags via `openmoc.options.Options()`.
2. Build or import geometry and initialize FSRs.
3. Create a `TrackGenerator`/`TrackGenerator3D`, configure threads/quadrature, and generate tracks.
4. Instantiate solver, set convergence threshold/threading, run selected solve routine.
5. Persist state with `openmoc.process.store_simulation_state(...)` if post-processing is needed.
6. Use `openmoc.process` and `openmoc.plotter` for analysis/visualization.
7. Escalate to SWIG/C++ only if docs/API behavior remain ambiguous.

### Minimal working example
```python
import openmoc
from openmoc.options import Options
import openmoc.process as process

opts = Options()
from geometry import geometry  # pattern used by sample-input scripts
geometry.initializeFlatSourceRegions()

track_generator = openmoc.TrackGenerator(geometry, opts.num_azim, opts.azim_spacing)
track_generator.setNumThreads(opts.num_omp_threads)
track_generator.generateTracks()

solver = openmoc.CPUSolver(track_generator)
solver.setNumThreads(opts.num_omp_threads)
solver.setConvergenceThreshold(opts.tolerance)
solver.computeEigenvalue(opts.max_iters)
process.store_simulation_state(solver, use_hdf5=True, fluxes=True)
```

### Pitfalls and fixes
- `setFixedSourceByCell/FSR` must happen after tracks are generated. (`docs/source/usersguide/input.rst`, `src/Solver.cpp`)
- Plotting tracks/segments can be expensive for large models and may trigger memory issues; reduce problem size/gridsize first. (`docs/source/usersguide/processing.rst`)
- Headless environments need non-interactive plotting; `plotter.py` switches to `Agg` when `DISPLAY` is absent.
- Runtime option names should match `openmoc/options.py` parser (`--num-threads-per-block`, etc.).
- SWIG/API mismatch debugging starts from `openmoc/swig/openmoc.i` and typemap files, not from generated wrappers.

### Convergence and validation checks
- Verify residual trend and timing report from solver output.
- Confirm persisted state round-trips with `restore_simulation_state(...)`.
- Cross-check key behaviors against focused tests (`tests/test_forward_*`, `tests/test_fixed_*`, `tests/test_krylov_*`).
- For script changes, validate both API-level output and numerical deltas (`k_eff`, iterations, flux fields).

## Scope
- Handle questions about language bindings, APIs, and programmatic interfaces.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/devguide/software_design.rst`
- `docs/source/devguide/swig.rst`
- `docs/source/doxygen/api.rst`
- `docs/source/api/index.rst`
- `docs/source/api/solver.rst`
- `docs/source/api/trackgenerator.rst`
- `docs/source/api/trackgenerator3D.rst`
- `docs/source/api/cpusolver.rst`
- `docs/source/api/cpulssolver.rst`
- `docs/source/api/cmfd.rst`
- `docs/source/api/log.rst`
- `docs/source/usersguide/processing.rst`

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
- `openmoc/options.py` (CLI/runtime option parsing)
- `openmoc/process.py` (state I/O, flux/fission-rate extraction)
- `openmoc/plotter.py` (plot behavior and backend handling)
- `openmoc/__init__.py` (module import bootstrap and logging init)
- `openmoc/swig/openmoc.i` and `openmoc/swig/*.i` (binding and typemap contract)
- `src/Solver.h` / `src/Solver.cpp` and `src/TrackGenerator.h` / `src/TrackGenerator.cpp` plus `src/TrackGenerator3D.h` / `src/TrackGenerator3D.cpp` (runtime behavior)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" openmoc src`).
