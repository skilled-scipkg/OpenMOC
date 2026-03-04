# openmoc source map: Inputs and Modeling

Generated from source roots:
- `openmoc`
- `src`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `geometry`
- `material`
- `boundary`
- `cell`
- `universe`
- `lattice`
- `hdf5`

## Fast source navigation
- `rg -n "setNumEnergyGroups|setSigmaT|setSigmaS|setSigmaF|setNuSigmaF|setChi" src/Material.h src/Material.cpp`
- `rg -n "setRootUniverse|initializeFlatSourceRegions|setCmfd|setDomainDecomposition" src/Geometry.h src/Geometry.cpp`
- `rg -n "setNumRings|setNumSectors|addSurface|addCell" src/Cell.h src/Cell.cpp src/Universe.h src/Universe.cpp`

## Suggested source entry points
- `openmoc/materialize.py` | HDF5 import semantics (`load_from_hdf5(...)`) and XS precedence behavior
- `src/Material.h` and `src/Material.cpp` | multigroup XS storage/queries (`setSigma*`, `getSigma*`, `buildFissionMatrix`)
- `src/Geometry.h` and `src/Geometry.cpp` | model assembly and FSR creation (`setRootUniverse`, `setCmfd`, `initializeFlatSourceRegions`)
- `src/Cell.h` and `src/Cell.cpp` | CSG and discretization controls (`addSurface`, `setNumRings`, `setNumSectors`)
- `src/Universe.h` and `src/Universe.cpp` | hierarchy composition (`addCell`)
- `src/Surface.h` and `src/Surface.cpp` | boundary-condition implementation (`setBoundaryType`)
- `src/boundary_type.h` | boundary enums used by Python/C++ APIs
- `openmoc/swig/openmoc.i` and `openmoc/swig/typemaps.i` | input-model object exposure to Python
