# openmoc source map: Build and Install

Generated from source roots:
- `openmoc`
- `src`
- build system roots: `setup.py`, `config.py`, `CMakeLists.txt`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `install`
- `build`
- `swig`
- `compiler`
- `cuda`
- `mpi`
- `debug`
- `linker`

## Fast source navigation
- `rg -n "<flag_or_symbol>" setup.py config.py openmoc src`
- `rg -n "initialize_options|finalize_options|customize_compiler|customize_linker|build_extensions" setup.py`
- `rg -n "compiler_flags|linker_flags|shared_libraries|library_directories|include_directories" config.py`

## Suggested source entry points
- `setup.py` | compile/install flags in `CustomInstall.initialize_options(...)` and `CustomInstall.finalize_options(...)` (`--cc`, `--fp`, `--with-cuda`, `--debug-mode`)
- `setup.py` | build pipeline hooks in `customize_compiler(...)`, `customize_linker(...)`, and `custom_build_ext.build_extensions(...)`
- `config.py` | default build configuration (`cc`, `fp`, `with_cuda`, `debug_mode`, `with_ccache`)
- `config.py` | binary-link behavior (`shared_libraries`, `library_directories`, `include_directories`, `compiler_flags`, `linker_flags`)
- `openmoc/__init__.py` | import/bootstrap checks after installation
- `openmoc/swig/openmoc.i` and `openmoc/cuda/openmoc_cuda.i` | SWIG wrapper boundary when compilation succeeds but module import fails
- `src/accel/cuda/GPUSolver.cu` and `src/accel/cuda/GPUQuery.h` | CUDA backend touchpoints for GPU-enabled builds
- `tests/run_tests.py` | quick post-build regression entry point
