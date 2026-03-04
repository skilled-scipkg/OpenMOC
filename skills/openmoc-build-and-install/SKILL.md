---
name: openmoc-build-and-install
description: This skill should be used when users ask about build and install in openmoc; it prioritizes documentation references and then source inspection only for unresolved details.
---

# openmoc: Build and Install

## High-Signal Playbook
### Route conditions
- Route modeling/geometry authoring to `openmoc-inputs-and-modeling`.
- Route solver setup, convergence, fixed-source, and CMFD runtime choices to `openmoc-simulation-workflows`.
- Route MPI/CUDA runtime scaling and domain decomposition to `openmoc-parallel-hpc`.
- Route Python/SWIG API usage questions to `openmoc-api-and-scripting`.

### Triage questions
- Which platform and package manager are in use (Ubuntu apt, MacPorts, conda)? (`docs/source/usersguide/ubuntu_prerequisites.rst`, `docs/source/usersguide/mac_prerequisites.rst`)
- Is this a standard CPU build, MPI build (`--cc=mpicc`), or CUDA build (`--with-cuda`)? (`docs/source/usersguide/install.rst`, `setup.py`)
- Do you need `single` or `double` precision (`--fp`)? (`docs/source/usersguide/install.rst`)
- Is this a first install with import/linker errors (for example `NameError`)? (`docs/source/usersguide/troubleshoot.rst`)
- Are debugging symbols/sanitizers needed now? (`docs/source/devguide/debugging.rst`, `docs/source/devguide/build_system.rst`)

### Canonical workflow
1. Install prerequisites in documented order (compiler, Python headers, SWIG, NumPy; optional matplotlib/h5py/mpi4py). (`docs/source/usersguide/ubuntu_prerequisites.rst`, `docs/source/usersguide/mac_prerequisites.rst`)
2. Clone source and inspect install flags (`python setup.py install --help`). (`docs/source/usersguide/install.rst`)
3. Build/install with `--user` first; add `--cc`, `--fp`, `--with-cuda`, `--debug-mode` only as needed. (`docs/source/usersguide/install.rst`)
4. Validate import and run a short sample case from `sample-input`. (`docs/source/quickinstall.rst`)
5. If import/link failures occur, apply troubleshooting (including two consecutive installs for known first-install issues). (`docs/source/usersguide/troubleshoot.rst`)
6. If still blocked, inspect build wiring in `setup.py`/`config.py` and SWIG interface files.

### Minimal working example
```bash
git clone https://github.com/mit-crpg/OpenMOC.git
cd OpenMOC
python setup.py install --help
python setup.py install --user --fp=double
# Optional CUDA debug build:
# python setup.py install --user --with-cuda --fp=single --debug-mode
python -c "import openmoc; print('openmoc import OK')"
python sample-input/simple-lattice/simple-lattice.py -i 5 -a 8 -s 0.12
```

### Pitfalls and fixes
- `--user` is a literal flag, not a username placeholder. (`docs/source/usersguide/install.rst`, `docs/source/quickinstall.rst`)
- `NameError: ... is not defined` right after install: run the install command twice in sequence. (`docs/source/usersguide/troubleshoot.rst`)
- Missing `swig` aborts build from `setup.py`; install SWIG first. (`setup.py`, `docs/source/usersguide/*_prerequisites.rst`)
- Loader/library issues: confirm `LD_LIBRARY_PATH` or explicit `library_directories` in `config.py`. (`docs/source/devguide/build_system.rst`, `config.py`)
- In `config.py`, do not prepend `l`/`lib` to `shared_libraries`. (`docs/source/devguide/build_system.rst`)

### Convergence and validation checks
- `import openmoc` succeeds in the target Python environment.
- A short sample run produces iterations/timing output without import/runtime errors (`sample-input/simple-lattice/simple-lattice.py`).
- Build flags in use match intent (`python setup.py install --help`, `setup.py` option parsing).
- For MPI or CUDA builds, run focused regression checks before broad runs (`tests/run_tests.py`, `tests/test_mpi_*`).

## Scope
- Handle questions about build, installation, compilation, and environment setup.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `README.rst`
- `docs/source/usersguide/install.rst`
- `docs/source/quickinstall.rst`
- `docs/source/usersguide/ubuntu_prerequisites.rst`
- `docs/source/usersguide/mac_prerequisites.rst`
- `docs/source/devguide/build_system.rst`
- `docs/source/devguide/debugging.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for the complete topic document list.
- Use tutorials/examples as executable usage patterns when available.
- Use tests as behavior or regression references when available.
- If ambiguity remains after docs, inspect `references/source_map.md` and start with the ranked source entry points.
- Cite exact documentation file paths in responses.

## Tutorials and examples
- `sample-input`
- `profile/models`

## Test references
- `tests`

## Optional deeper inspection
- `openmoc`
- `src`

## Source entry points for unresolved issues
- `setup.py` (install flags, compiler/linker selection, SWIG invocation)
- `config.py` (compiler flags, include/library directories, CUDA/MPI wiring)
- `CMakeLists.txt` (ctest wiring used by regression harness)
- `openmoc/swig/openmoc.i` (main SWIG interface)
- `openmoc/cuda/openmoc_cuda.i` (CUDA SWIG interface)
- `openmoc/__init__.py` (module import/log initialization)
- `src/accel/cuda/GPUSolver.cu` and `src/accel/cuda/GPUQuery.h` (CUDA backend touchpoints)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" setup.py config.py openmoc src`).
