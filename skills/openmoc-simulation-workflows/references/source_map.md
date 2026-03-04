# openmoc source map: Simulation Workflows

Generated from source roots:
- `openmoc`
- `src`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `workflow`
- `run control`
- `eigenvalue`
- `fixed source`
- `restart`
- `convergence`
- `timing`

## Fast source navigation
- `rg -n "computeFlux|computeSource|computeEigenvalue|setFixedSource|setConvergenceThreshold" src/Solver.cpp src/Solver.h`
- `rg -n "store_simulation_state|restore_simulation_state|parse_convergence_data" openmoc/process.py`
- `rg -n "Options|num_azim|azim_spacing|max_iters|tolerance" openmoc/options.py`

## Suggested source entry points
- `openmoc/options.py` | runtime input parser (`Options`) and defaults for run control
- `src/Geometry.cpp` | workflow prerequisites (`initializeFlatSourceRegions()`, `setCmfd(...)`, `setDomainDecomposition(...)`)
- `src/TrackGenerator.cpp` | track generation and timing (`generateTracks()`, `printTimerReport(...)`)
- `src/Solver.cpp` and `src/Solver.h` | mode selection and convergence (`computeFlux`, `computeSource`, `computeEigenvalue`, `setConvergenceThreshold`, `setFixedSourceByFSR`)
- `src/CPUSolver.cpp` and `src/CPULSSolver.cpp` | thread controls and linear-source specialization (`setNumThreads`, `resetFixedSources`)
- `openmoc/process.py` | checkpoint/restart and data extraction (`store_simulation_state`, `restore_simulation_state`, `get_scalar_fluxes`)
- `openmoc/krylov.py` | eigenmode workflow (`IRAMSolver.computeEigenmodes`) and boundary checks
- `src/RunTime.cpp` and `src/Timer.cpp` | low-level runtime accounting
