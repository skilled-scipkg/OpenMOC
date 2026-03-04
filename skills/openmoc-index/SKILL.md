---
name: openmoc-index
description: This skill should be used when users ask how to use openmoc and the correct generated documentation skill must be selected before going deeper into source code.
---

# openmoc Skills Index

## Route the request
- Classify the request into one generated topic skill listed below.
- Keep responses docs-first; escalate to source only when topic docs and examples are insufficient.
- Prefer workflow-level guidance before symbol-level source inspection.

## Generated topic skills
- `openmoc-api-and-scripting`: API and scripting (language bindings, programmatic interfaces, plotting/post-processing)
- `openmoc-inputs-and-modeling`: Inputs and modeling (materials, geometry, boundaries, lattices, discretization)
- `openmoc-build-and-install`: Build and install (prereqs, compilers, CUDA/MPI build variants, debug setup)
- `openmoc-theory-and-methods`: Theory and methods (track generation, convergence, eigenvalue/Krylov methods)
- `openmoc-getting-started`: Getting started (quickstart orientation)
- `openmoc-parallel-hpc`: Parallel and HPC (MPI/OpenMP/GPU execution and scaling)
- `openmoc-simulation-workflows`: Simulation workflows (run control, convergence, restart/output routines)
- `openmoc-developer-guide`: Developer guide (architecture and extension workflow)
- `openmoc-releasenotes`: Release notes (versioned feature/fix deltas and regression triage)
- `openmoc-usersguide`: Users guide topic coverage
- `openmoc-tests`: Test harness and regression routing
- `openmoc-advanced-topics`: Consolidated low-frequency docs (documentation authoring, license, publications)

## Documentation-first inputs
- `docs`

## Tutorials and examples roots
- `sample-input`
- `profile/models`

## Test roots for behavior checks
- `tests`

## Escalate only when needed
- Start from topic skill primary references.
- If references are insufficient, inspect the selected topic skill doc map (for example `skills/openmoc-inputs-and-modeling/references/doc_map.md`).
- If ambiguity remains, inspect the selected topic skill source map (for example `skills/openmoc-inputs-and-modeling/references/source_map.md`) and follow suggested entry points.
- Use targeted symbol search (for example: `rg -n "<symbol_or_keyword>" openmoc src docs`).

## Source directories for deeper inspection
- `openmoc`
- `src`
