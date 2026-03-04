# openmoc source map: Parallel and HPC

Generated from source roots:
- `openmoc`
- `src`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `mpi`
- `openmp`
- `cuda`
- `decomposition`
- `scaling`
- `performance`

## Fast source navigation
- `rg -n "setDomainDecomposition|MPI_Comm|MPI" src/Geometry.h src/Geometry.cpp openmoc/swig/mpi4py.i`
- `rg -n "setNumThreads|computeEigenvalue|computeSource" src/CPUSolver.cpp src/Solver.cpp`
- `rg -n "GPUSolver|GPUQuery|threads_per_block|thread_blocks" src/accel/cuda openmoc/options.py`

## Suggested source entry points
- `src/Geometry.h` and `src/Geometry.cpp` | domain decomposition setup (`setDomainDecomposition(...)`) and partition-aware geometry flow
- `openmoc/swig/mpi4py.i` | communicator mapping between Python MPI and C++
- `src/ParallelHashMap.h` | parallel data-structure behavior used in decomposed runs
- `src/CPUSolver.cpp` | OpenMP thread controls (`setNumThreads`) and sweep execution path
- `src/accel/cuda/GPUSolver.h` and `src/accel/cuda/GPUSolver.cu` | GPU transport solve implementation
- `src/accel/cuda/GPUQuery.h` | GPU capability checks used during CUDA setup
- `openmoc/cuda/openmoc_cuda.i` | CUDA wrapper boundary for Python usage
- `tests/test_mpi_2D/2D_lattice.py` and `tests/test_mpi_3D/3D_lattice.py` | known-good MPI decomposition patterns
- `tests/test_mpi_2D/test_mpi_2D.py` and `tests/test_mpi_3D/test_mpi_3D.py` | regression entry points for distributed correctness
