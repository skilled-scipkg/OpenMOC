---
name: openmoc-inputs-and-modeling
description: This skill should be used when users ask about inputs and modeling in openmoc; it prioritizes documentation references and then source inspection only for unresolved details.
---

# openmoc: Inputs and Modeling

## High-Signal Playbook
### Route conditions
- Route install/compiler environment blockers to `openmoc-build-and-install`.
- Route solver-mode selection (eigenvalue vs fixed-source, convergence controls, CMFD run behavior) to `openmoc-simulation-workflows`.
- Route MPI/domain decomposition or GPU execution questions to `openmoc-parallel-hpc`.
- Route detailed API/post-processing requests to `openmoc-api-and-scripting`.

### Triage questions
- Are materials provided manually or loaded via HDF5 (`openmoc.materialize`)? (`docs/source/usersguide/input.rst`)
- Is the model 2D or 3D, and what boundaries are required (vacuum/reflective)? (`docs/source/usersguide/input.rst`)
- Is geometry built with direct `Cell.addSurface(...)` or explicit `Region` trees? (`docs/source/usersguide/input.rst`, `docs/source/methods/constructive_solid_geometry.rst`)
- Are rings/sectors required for pin gradients? (`docs/source/usersguide/input.rst`)
- Is lattice structure uniform or non-uniform? (`docs/source/usersguide/input.rst`)
- Is CMFD attached before track generation? (`docs/source/usersguide/input.rst`)

### Canonical workflow
1. Load or define multi-group cross sections, then confirm group count and domain mapping. (`docs/source/usersguide/input.rst`, `openmoc/materialize.py`)
2. Create surfaces and set boundary conditions.
3. Build cells, fills, and halfspace/region definitions.
4. Add rings/sectors where flux gradients require extra spatial resolution.
5. Compose universes/lattices and register a root universe in `Geometry`.
6. Add optional symmetry/domain decomposition/CMFD structures before tracking.
7. Call `geometry.initializeFlatSourceRegions()` before any track generation.
8. Validate geometry with quick plots or geometry-print style checks.

### Minimal working example
```python
import openmoc

materials = openmoc.materialize.load_from_hdf5('c5g7-mgxs.h5', '../')
zcyl = openmoc.ZCylinder(x=0.0, y=0.0, radius=1.0)
xmin, xmax = openmoc.XPlane(x=-2.0), openmoc.XPlane(x=2.0)
ymin, ymax = openmoc.YPlane(y=-2.0), openmoc.YPlane(y=2.0)
for s in (xmin, xmax, ymin, ymax):
    s.setBoundaryType(openmoc.REFLECTIVE)

fuel = openmoc.Cell(name='fuel'); fuel.setFill(materials['UO2'])
fuel.addSurface(-1, zcyl); fuel.setNumRings(3); fuel.setNumSectors(8)
moderator = openmoc.Cell(name='moderator'); moderator.setFill(materials['Water'])
moderator.addSurface(+1, zcyl)
for hs, s in ((+1, xmin), (-1, xmax), (+1, ymin), (-1, ymax)):
    moderator.addSurface(hs, s)

root = openmoc.Universe(name='root'); root.addCell(fuel); root.addCell(moderator)
geometry = openmoc.Geometry(); geometry.setRootUniverse(root)
geometry.initializeFlatSourceRegions()
```

### Pitfalls and fixes
- Tracks generated before FSR initialization cause failures: call `geometry.initializeFlatSourceRegions()` first. (`docs/source/usersguide/input.rst`, `src/TrackGenerator.cpp`)
- Rings only apply to cells containing `ZCylinder` surfaces. (`docs/source/usersguide/input.rst`)
- Fixed sources must be set after tracks are generated. (`docs/source/usersguide/input.rst`)
- HDF5 precedence rules matter: `transport` over `total`, `nu-scatter matrix` over `scatter matrix`. (`docs/source/usersguide/input.rst`, `openmoc/materialize.py`)
- OpenMOC enforces a minimum total XS (1E-10), so near-void models need interpretation care. (`docs/source/usersguide/input.rst`)

### Convergence and validation checks
- Check model cardinalities after setup: number of FSRs/materials/energy groups (`geometry.getNumFSRs()`, `geometry.getNumMaterials()`).
- Validate boundary assignments and lattice widths before long runs.
- Compare geometry printouts against known references when debugging topology (`tests/test_geometry_print/geometry_true.txt`).
- Run sensitivity sweeps on rings/sectors and track spacing for key observables (`k_eff`, flux shape).

## Scope
- Handle questions about inputs, system setup, models, and physical parameterization.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `docs/source/usersguide/input.rst`
- `docs/source/methods/constructive_solid_geometry.rst`
- `docs/source/usersguide/processing.rst`
- `docs/source/api/material.rst`
- `docs/source/api/geometry.rst`
- `docs/source/api/index.rst`
- `docs/source/devguide/swig.rst`
- `tests/test_geometry_print/geometry_true.txt`

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
- `openmoc/materialize.py` (HDF5 import semantics and precedence)
- `src/Geometry.h` / `src/Geometry.cpp` (model assembly, FSR initialization)
- `src/Material.h` / `src/Material.cpp` (cross-section storage and access)
- `src/Cell.h` / `src/Cell.cpp` and `src/Universe.h` / `src/Universe.cpp` (CSG hierarchy)
- `src/boundary_type.h` (boundary enums and meaning)
- `openmoc/swig/openmoc.i` and `openmoc/swig/typemaps.i` (Python/C++ exposure)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" openmoc src`).
