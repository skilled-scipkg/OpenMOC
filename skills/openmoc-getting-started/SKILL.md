---
name: openmoc-getting-started
description: This skill should be used when users ask about getting started in openmoc; it prioritizes documentation references and then source inspection only for unresolved details.
---

# openmoc: Getting Started

## High-Signal Playbook
### Route conditions
- Route installation/build blockers to `openmoc-build-and-install`.
- Route detailed geometry/material modeling to `openmoc-inputs-and-modeling`.
- Route MPI/CUDA scaling questions to `openmoc-parallel-hpc`.
- Route deep method derivations to `openmoc-theory-and-methods`.

### Triage questions
- Is OpenMOC importable in the target Python environment?
- Is this first run eigenvalue (`computeEigenvalue`) or fixed-source (`computeSource`/`computeFlux`)?
- Are basic runtime knobs set (`-a`, `-s`, `-i`, `-c`, threads)?
- Are we using a known sample input before custom model edits?

### Canonical workflow
1. Verify import and runtime options (`openmoc.options.Options()`).
2. Run a known sample input (`sample-input/simple-lattice`).
3. Inspect convergence (`k_eff`, residual, iteration count).
4. Persist state and basic outputs for verification.
5. Move to custom geometry/material scripts only after baseline run is stable.

### Minimal working example
```bash
python -c "import openmoc; print('openmoc import OK')"
python sample-input/simple-lattice/simple-lattice.py -i 30 -a 8 -s 0.12 -c 1e-5
```

### Pitfalls and fixes
- Running custom scripts before a known sample baseline makes debugging slow.
- Forgetting `initializeFlatSourceRegions()` before tracking causes immediate runtime errors.
- Coarse azimuthal settings can hide expected behavior; refine before diagnosing solver bugs.
- If import fails right after install, use the troubleshooting flow in user docs.

### Convergence and validation checks
- Confirm sample case reaches tolerance without runtime exceptions.
- Check `k_eff` trend is stable across reruns with same settings.
- Verify timing/residual data appears in output logs.
- If results drift unexpectedly, compare against a regression test variant first.

## Scope
- Handle questions about initial setup, quickstarts, and core concepts.
- Keep responses abstract and architectural for large codebases; avoid exhaustive per-function documentation unless requested.

## Primary documentation references
- `README.rst`
- `docs/source/quickinstall.rst`
- `docs/source/usersguide/beginners.rst`
- `docs/source/methods/introduction.rst`
- `docs/source/methods/method_of_characteristics.rst`
- `docs/source/methods/cmfd.rst`

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
- `openmoc/options.py` (`Options` defaults and argument parsing)
- `src/Geometry.cpp` (`setRootUniverse`, `initializeFlatSourceRegions`)
- `src/TrackGenerator.cpp` (`generateTracks` workflow)
- `src/Solver.cpp` (`computeEigenvalue`, `computeSource`, `computeFlux`)
- `openmoc/process.py` (state persistence and quick post-run checks)
- Prefer targeted source search (for example: `rg -n "<symbol_or_keyword>" openmoc src sample-input`).
