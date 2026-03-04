---
name: openmoc-simulation-workflows
description: This skill should be used when users ask about simulation workflows in openmoc; it prioritizes documentation references and then source inspection only for unresolved details.
---

# openmoc: Simulation Workflows

## High-Signal Playbook
### Route conditions
- Route install/compiler/toolchain failures to `openmoc-build-and-install`.
- Route geometry/material authoring to `openmoc-inputs-and-modeling`.
- Route MPI/CUDA scaling to `openmoc-parallel-hpc`.
- Route detailed API and plotting questions to `openmoc-api-and-scripting`.

### Triage questions
- Is this run eigenvalue, fixed-source, or Krylov eigenmodes? (`docs/source/usersguide/input.rst`, `openmoc/krylov.py`)
- Are tracks/FSRs already initialized in the expected order? (`src/Geometry.cpp`, `src/TrackGenerator.cpp`)
- Is CMFD enabled and configured before tracking? (`docs/source/usersguide/input.rst`, `src/Cmfd.cpp`)
- Is restart/post-processing required (HDF5/pickle state)? (`openmoc/process.py`)
- Are runtime options and thread counts set from `openmoc.options.Options()`? (`openmoc/options.py`)

### Canonical workflow
1. Parse runtime options from `openmoc.options.Options()`.
2. Build/import geometry and call `geometry.initializeFlatSourceRegions()`.
3. Create a `TrackGenerator` (or `TrackGenerator3D`), set threads, generate tracks.
4. Create solver (`CPUSolver`/`CPULSSolver`/`GPUSolver`) and set convergence settings.
5. Run `computeEigenvalue(...)`, `computeSource(...)`, or `computeFlux(...)` based on goal.
6. Persist and inspect outputs via `openmoc.process.store_simulation_state(...)`.
7. Validate numerical behavior before scaling complexity.

### Minimal working example
```bash
python sample-input/simple-lattice/simple-lattice.py -i 30 -a 8 -s 0.12 -c 1e-5
```
```python
import openmoc
import openmoc.process as process
from geometry import geometry

opts = openmoc.options.Options()
geometry.initializeFlatSourceRegions()

track_generator = openmoc.TrackGenerator(geometry, opts.num_azim, opts.azim_spacing)
track_generator.setNumThreads(opts.num_omp_threads)
track_generator.generateTracks()

solver = openmoc.CPUSolver(track_generator)
solver.setConvergenceThreshold(opts.tolerance)
solver.computeEigenvalue(opts.max_iters)
process.store_simulation_state(solver, fluxes=True, use_hdf5=True)
```

### Pitfalls and fixes
- Generating tracks before `initializeFlatSourceRegions()` causes failures and invalid setup.
- `setFixedSourceByCell/FSR` should be applied after tracks are generated.
- `computeFlux(...)` with default `only_fixed_source=True` is not equivalent to full source iteration.
- CMFD setup mistakes (mesh/group structure) can produce unstable updates; start from tested cases.
- Restart/state analysis should use matching file format and version (`store_simulation_state` vs `restore_simulation_state`).

### Convergence and validation checks
- Confirm source residual drops below requested tolerance.
- Check `k_eff` and iteration trend stability under small discretization changes.
- Validate saved state reloads cleanly with `restore_simulation_state(...)`.
- Cross-check representative workflows with `tests/test_forward_*`, `tests/test_fixed_*`, and `tests/test_cmfd_*`.

## Scope
- Handle questions about simulation setup, execution flow, and runtime controls.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/usersguide/running.rst`
- `docs/source/usersguide/input.rst`
- `docs/source/usersguide/processing.rst`
- `docs/source/devguide/work_flow.rst`
- `docs/source/methods/eigenvalue_calculations.rst`

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
- `openmoc/options.py` (`Options` run-control parsing)
- `src/Geometry.cpp` (`initializeFlatSourceRegions`, `setCmfd`, `setDomainDecomposition`)
- `src/TrackGenerator.cpp` (`generateTracks`, segment/timer behavior)
- `src/Solver.cpp` (`computeFlux`, `computeSource`, `computeEigenvalue`, fixed-source setters)
- `openmoc/process.py` (`store_simulation_state`, `restore_simulation_state`, convergence parsing)
- `openmoc/krylov.py` (`IRAMSolver.computeEigenmodes`, boundary checks)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" openmoc src tests`).
