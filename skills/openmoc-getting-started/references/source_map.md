# openmoc source map: Getting Started

Generated from source roots:
- `openmoc`
- `src`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `quickstart`
- `first run`
- `cmfd`
- `tracks`
- `solver`
- `options`

## Fast source navigation
- `rg -n "Options|num_azim|azim_spacing|max_iters|tolerance" openmoc/options.py`
- `rg -n "initializeFlatSourceRegions|generateTracks|computeEigenvalue|computeSource" openmoc src`
- `rg -n "store_simulation_state|restore_simulation_state" openmoc/process.py`

## Suggested source entry points
- `openmoc/options.py` | runtime defaults and CLI parsing in `Options`
- `src/Geometry.cpp` | first-run geometry setup and FSR initialization (`setRootUniverse(...)`, `initializeFlatSourceRegions()`)
- `src/TrackGenerator.cpp` | track creation flow (`setNumThreads(...)`, `generateTracks()`)
- `src/Solver.cpp` | solver mode entry points (`computeFlux(...)`, `computeSource(...)`, `computeEigenvalue(...)`)
- `src/Cmfd.cpp` | CMFD setup knobs (`setLatticeStructure(...)`, `setGroupStructure(...)`, `setSORRelaxationFactor(...)`)
- `openmoc/process.py` | state persistence and convergence parsing (`store_simulation_state(...)`, `restore_simulation_state(...)`, `parse_convergence_data(...)`)
- `sample-input/simple-lattice/simple-lattice.py` | canonical minimal runnable simulation
- `sample-input/krylov/adjoint-eigenmodes.py` | first extension from standard solve to eigenmodes
