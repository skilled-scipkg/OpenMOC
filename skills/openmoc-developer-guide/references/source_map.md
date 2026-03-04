# openmoc source map: Developer Guide

Generated from source roots:
- `openmoc`
- `src`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `architecture`
- `developer`
- `internals`
- `swig`
- `extension`
- `contribution`

## Fast source navigation
- `rg -n "<symbol_or_keyword>" openmoc src`
- `rg -n "initializeFlatSourceRegions|generateTracks|computeEigenvalue|computeSource|computeFlux" src`
- `rg -n "class|def|%include|%template" openmoc/swig src`

## Suggested source entry points
- `src/Geometry.cpp` | geometry lifecycle: `setRootUniverse(...)`, `setDomainDecomposition(...)`, `initializeFlatSourceRegions()`
- `src/TrackGenerator.cpp` | tracking lifecycle: `initializeTracks()`, `generateTracks()`, `segmentize()`, `splitSegments(...)`
- `src/Solver.cpp` | solve lifecycle: `computeFlux(...)`, `computeSource(...)`, `computeEigenvalue(...)`, `setConvergenceThreshold(...)`
- `src/TrackTraversingAlgorithms.cpp` | transport kernels and segmentation passes (`TransportSweep::onTrack`, `SegmentCounter`, `SegmentSplitter`)
- `openmoc/swig/openmoc.i` | C++ API symbols exported to Python
- `openmoc/swig/typemaps.i` and `openmoc/swig/argument_typemaps.i` | Python/C++ conversion contracts
- `setup.py` and `config.py` | extension build integration points used by contributors
- `tests/testing_harness.py` | behavior-check scaffold (`_run_openmoc`, `_get_results`, `_compare_results`)
