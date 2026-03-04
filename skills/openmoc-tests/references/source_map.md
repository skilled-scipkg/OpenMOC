# openmoc source map: Tests

Generated from source roots:
- `openmoc`
- `src`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `regression`
- `harness`
- `cmfd`
- `forward`
- `fixed source`
- `krylov`
- `mpi`

## Fast source navigation
- `rg -n "TestHarness|_run_openmoc|_get_results|_compare_results" tests/testing_harness.py`
- `rg -n "computeEigenvalue|computeSource|computeFlux|setFixedSource" src/Solver.cpp src/Solver.h`
- `rg -n "computeKeff|setLatticeStructure|setGroupStructure" src/Cmfd.cpp src/Cmfd.h`

## Suggested source entry points
- `tests/run_tests.py` | suite orchestration and filtering (`-R/--tests-regex`, MPI mode)
- `tests/testing_harness.py` | standard regression lifecycle (`main`, `_run_openmoc`, `_get_results`, `_compare_results`)
- `tests/test_forward_simple_lattice/test_forward_simple_lattice.py` | baseline forward-eigenvalue behavior check
- `tests/test_fixed_linear_source/test_fixed_linear_source.py` | fixed-source semantics and source setup checks
- `tests/test_krylov_forward/test_krylov_forward.py` | Krylov eigenmode behavior checks
- `tests/test_mpi_2D/test_mpi_2D.py` | MPI decomposition smoke test
- `tests/test_cmfd_restart/test_cmfd_restart.py` | restart/state continuity with CMFD
- `src/Solver.cpp` | solver mode mechanics behind most test failures
- `src/Cmfd.cpp` | CMFD stabilization and update-ratio behavior
