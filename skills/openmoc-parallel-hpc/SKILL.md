---
name: openmoc-parallel-hpc
description: This skill should be used when users ask about parallel and hpc in openmoc; it prioritizes documentation references and then source inspection only for unresolved details.
---

# openmoc: Parallel and HPC

## High-Signal Playbook
### Route conditions
- Route build flags, compiler selection, and CUDA/MPI install blockers to `openmoc-build-and-install`.
- Route geometry decomposition setup (cells/lattices/boundaries) to `openmoc-inputs-and-modeling`.
- Route solver-mode/convergence logic (fixed/eigen/CMFD restart) to `openmoc-simulation-workflows`.
- Route API-level scripting or post-processing details to `openmoc-api-and-scripting`.

### Triage questions
- Is this MPI domain decomposition, OpenMP threading, CUDA acceleration, or a hybrid run?
- Was OpenMOC built with `--cc=mpicc` and (if needed) `--with-cuda`? (`setup.py`, `config.py`)
- Does `setDomainDecomposition(nx, ny, nz, MPI.COMM_WORLD)` match launched rank count? (`docs/source/usersguide/input.rst`, `tests/test_mpi_*`)
- Is the case 2D (`TrackGenerator`) or 3D (`TrackGenerator3D`, segmentation mode)? (`tests/test_mpi_2D/*`, `tests/test_mpi_3D/*`)
- For CUDA runs, are thread block sizes warp-aligned? (`docs/source/usersguide/running.rst`)

### Canonical workflow
1. Build OpenMOC for target parallel mode (MPI and/or CUDA).
2. Ensure runtime deps are present (`mpi4py`, CUDA runtime where needed).
3. Set domain decomposition in geometry before tracking.
4. Initialize FSRs, generate tracks, and configure solver threading.
5. Launch with `mpirun` using rank count equal to decomposition product.
6. Compare against serial baseline or known regression outputs.
7. If mismatch persists, inspect MPI/CUDA binding and backend source entry points.

### Minimal working example
```python
from mpi4py import MPI
import openmoc
from geometry import geometry

geometry.setDomainDecomposition(3, 2, 1, MPI.COMM_WORLD)
geometry.initializeFlatSourceRegions()

track_generator = openmoc.TrackGenerator(geometry, 8, 0.12)
track_generator.setNumThreads(2)
track_generator.generateTracks()

solver = openmoc.CPUSolver(track_generator)
solver.setNumThreads(2)
solver.setConvergenceThreshold(1e-5)
solver.computeEigenvalue(50)
```
```bash
mpirun -n 6 --oversubscribe python run_script.py
```

### Pitfalls and fixes
- Domain decomposition mismatch (`nx*ny*nz != ranks`) yields invalid execution patterns; align both.
- Fresh installs that hit `NameError` may need two install passes. (`docs/source/usersguide/troubleshoot.rst`)
- MPI runs need `mpi4py` installed and MPI-enabled build path.
- 3D MPI tests rely on `TrackGenerator3D` plus segment-formation settings; mirror tested patterns first. (`tests/test_mpi_3D/3D_lattice.py`)
- CUDA threads-per-block should be warp-aligned; OpenMOC rounds up to nearest warp multiple. (`docs/source/usersguide/running.rst`)

### Convergence and validation checks
- Use known MPI smoke tests (`tests/test_mpi_2D/test_mpi_2D.py`, `tests/test_mpi_3D/test_mpi_3D.py`).
- Compare `k_eff`/iterations against non-MPI reference runs for the same model.
- Validate CMFD+MPI combinations with dedicated tests (`tests/test_mpi_*_CMFD`).
- Inspect timing and per-rank behavior before scaling out.

## Scope
- Handle questions about MPI/OpenMP/GPU execution, scaling, and batch systems.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/usersguide/running.rst`
- `docs/source/usersguide/input.rst`
- `docs/source/usersguide/troubleshoot.rst`
- `docs/source/methods/parallelization.rst`
- `docs/source/api/gpusolver.rst`

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
- `tests/test_mpi_2D/2D_lattice.py` and `tests/test_mpi_3D/3D_lattice.py` (known-good decomposition patterns)
- `openmoc/swig/mpi4py.i` (MPI communicator typemaps)
- `src/ParallelHashMap.h` (parallel data structure touchpoint)
- `src/accel/cuda/GPUSolver.h` / `src/accel/cuda/GPUSolver.cu` (CUDA solver backend)
- `src/accel/cuda/GPUQuery.h` and `openmoc/cuda/openmoc_cuda.i` (GPU capability/wrapper layer)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" openmoc src tests`).
