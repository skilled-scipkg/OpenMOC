# openmoc source map: Theory and Methods

Generated from source roots:
- `openmoc`
- `src`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `track generation`
- `source iteration`
- `eigenvalue`
- `krylov`
- `transport sweep`
- `convergence`

## Fast source navigation
- `rg -n "generateTracks|segmentize|splitSegments|countSegments" src/TrackGenerator.cpp src/TrackGenerator.h`
- `rg -n "computeFlux|computeSource|computeEigenvalue|setConvergenceThreshold" src/Solver.cpp src/Solver.h`
- `rg -n "IRAMSolver|computeEigenmodes|VACUUM" openmoc/krylov.py`
- `rg -n "TransportSweep::onTrack|SegmentCounter|SegmentSplitter" src/TrackTraversingAlgorithms.cpp`

## Suggested source entry points
- `src/TrackGenerator.h` and `src/TrackGenerator.cpp` | 2D track/segment algorithm flow (`generateTracks`, `segmentize`, `splitSegments`)
- `src/TrackGenerator3D.h` and `src/TrackGenerator3D.cpp` | 3D tracking and segmentation behavior
- `src/TrackTraversingAlgorithms.h` and `src/TrackTraversingAlgorithms.cpp` | transport sweep kernels and segment traversal algorithms
- `src/Track.h` and `src/Track.cpp` | track-level segment containers and operations
- `src/Solver.h` and `src/Solver.cpp` | residual definitions and solve loop behavior (`computeFlux`, `computeSource`, `computeEigenvalue`)
- `openmoc/krylov.py` | IRAM operator setup and boundary-condition requirements
- `src/Cmfd.cpp` | acceleration-theory coupling and update-ratio behavior
