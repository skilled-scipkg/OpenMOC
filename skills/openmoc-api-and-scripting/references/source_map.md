# openmoc source map: API and Scripting

Generated from source roots:
- `openmoc`
- `src`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `api`
- `bindings`
- `swig`
- `solver`
- `trackgenerator`
- `post-processing`
- `plotting`

## Fast source navigation
- `rg -n "store_simulation_state|restore_simulation_state|get_scalar_fluxes|parse_convergence_data" openmoc/process.py`
- `rg -n "computeEigenvalue|computeSource|computeFlux|setFixedSource" src/Solver.h src/Solver.cpp`
- `rg -n "generateTracks|retrieveTrackCoords|retrieveSegmentCoords|setNumThreads" src/TrackGenerator.h src/TrackGenerator.cpp`
- `rg -n "%include|%template|typemap" openmoc/swig/*.i`

## Suggested source entry points
- `openmoc/options.py` | runtime argument parsing and default values in `Options`
- `openmoc/process.py` | Python API for results/state (`store_simulation_state`, `restore_simulation_state`, `get_scalar_fluxes`)
- `openmoc/plotter.py` | plotting API (`plot_tracks`, `plot_segments`, `plot_materials`, `plot_cells`)
- `openmoc/krylov.py` | eigenmode API (`IRAMSolver`, `computeEigenmodes`) and boundary checks
- `openmoc/swig/openmoc.i` | exported C++ symbol list for Python bindings
- `openmoc/swig/typemaps.i` and `openmoc/swig/argument_typemaps.i` | Python/C++ type conversion behavior
- `src/Solver.h` and `src/Solver.cpp` | solver API behavior (`computeFlux`, `computeSource`, `computeEigenvalue`, fixed-source setters)
- `src/TrackGenerator.h` and `src/TrackGenerator.cpp` | 2D tracking API behavior (`generateTracks`, segment retrieval)
- `src/TrackGenerator3D.h` and `src/TrackGenerator3D.cpp` | 3D tracking API behavior
- `src/Universe.h` and `src/Universe.cpp` | hierarchy API backing `Cell`/`Universe` exposure in Python
