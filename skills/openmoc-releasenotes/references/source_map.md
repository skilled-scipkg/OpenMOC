# openmoc source map: Releasenotes

Generated from source roots:
- `openmoc`
- `src`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `release`
- `cmfd`
- `track file`
- `geometry string`
- `compatibility`
- `state`

## Fast source navigation
- `rg -n "CMFD|diffusion|computeKeff|computeLarsensEDCFactor" src/Cmfd.cpp src/Cmfd.h`
- `rg -n "getTestFilename|readSegmentsFromFile|dumpSegmentsToFile" src/TrackGenerator.cpp src/TrackGenerator.h`
- `rg -n "store_simulation_state|restore_simulation_state" openmoc/process.py`
- `rg -n "compatible|opencg" openmoc/opencg_compatible.py openmoc/compatible/opencg_compatible.py`

## Suggested source entry points
- `src/Cmfd.h` and `src/Cmfd.cpp` | CMFD behavior touched by historical fixes (`computeKeff`, diffusion/correction-factor routines)
- `src/TrackGenerator.h` and `src/TrackGenerator.cpp` | track-file naming/loading flow (`getTestFilename`, `readSegmentsFromFile`, `dumpSegmentsToFile`)
- `src/VectorizedSolver.h` and `src/VectorizedSolver.cpp` | vectorized solver behavior referenced by performance/bug notes
- `openmoc/process.py` | state persistence behavior cited by release notes (`store_simulation_state`, `restore_simulation_state`)
- `openmoc/opencg_compatible.py` and `openmoc/compatible/opencg_compatible.py` | compatibility-layer behavior for migration/regression triage
- `tests/run_tests.py` plus targeted tests in `tests/test_cmfd_*`, `tests/test_forward_*`, and `tests/test_krylov_*` | release-note claim validation
