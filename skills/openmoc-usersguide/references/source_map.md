# openmoc source map: Usersguide

Generated from source roots:
- `openmoc`
- `src`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `users guide`
- `options`
- `geometry`
- `tracks`
- `solver`
- `post-processing`
- `plotting`

## Fast source navigation
- `rg -n "Options|num_azim|azim_spacing|max_iters|tolerance" openmoc/options.py`
- `rg -n "initializeFlatSourceRegions|generateTracks|computeEigenvalue|computeSource|computeFlux" openmoc src`
- `rg -n "plot_tracks|plot_segments|plot_materials|plot_cells|plot_flat_source_regions" openmoc/plotter.py`

## Suggested source entry points
- `openmoc/options.py` | command-line/runtime controls (`Options`)
- `openmoc/materialize.py` | HDF5 material import contract (`load_from_hdf5(...)`)
- `src/Geometry.cpp` | user-facing geometry assembly path (`setRootUniverse`, `setCmfd`, `initializeFlatSourceRegions`)
- `src/TrackGenerator.cpp` | track generation and reporting (`generateTracks`, `printTimerReport`)
- `src/Solver.cpp` | solver run modes and convergence updates (`computeEigenvalue`, `computeSource`, `computeFlux`)
- `openmoc/process.py` | persisted outputs and restart (`store_simulation_state`, `restore_simulation_state`, `parse_convergence_data`)
- `openmoc/plotter.py` | plotting behavior and headless backend fallback (`matplotlib.use('Agg')`)
- `src/boundary_type.h` | boundary constants mapped from docs/API usage
