---
name: openmoc-advanced-topics
description: This skill should be used when users ask about low-frequency advanced documentation topics in openmoc (documentation authoring, license terms, or publications/citation metadata) and a compact docs-first route is needed.
---

# openmoc: Advanced Topics

## Scope
- Handle low-frequency topics that each map to a small doc set: documentation authoring workflow, license text, and publication/citation references.
- Keep responses docs-first and concise; route simulation/build/API/HPC requests to their dedicated skills.

## Route the request
- Use this skill for:
  - Documentation authoring/process questions (`docs/source/devguide/documentation.rst`)
  - License text/redistribution wording (`docs/source/license.rst`)
  - Citation/publication lookup (`docs/source/publications.rst`)
- Route to core skills when the question is operational:
  - Build/install -> `openmoc-build-and-install`
  - Input modeling -> `openmoc-inputs-and-modeling`
  - API/post-processing -> `openmoc-api-and-scripting`
  - Parallel execution -> `openmoc-parallel-hpc`
  - Methods/convergence theory -> `openmoc-theory-and-methods`

## Primary documentation references
- `docs/source/devguide/documentation.rst`
- `docs/source/license.rst`
- `docs/source/publications.rst`

## Workflow
- Start with the primary references above.
- If details are missing, inspect `references/doc_map.md` for complete inventory.
- Escalate to `references/source_map.md` only when implementation details are explicitly requested.
- Cite exact file paths for any legal/citation/documentation-process claim.

## Source entry points for unresolved issues
- `docs/source/conf.py` (Sphinx config and documentation build context)
- `openmoc/swig/docstring.i` (API docstring source generated from Doxygen)
- `README.rst` and `LICENSE` (root-level project metadata/legal context)
- Prefer targeted search first (for example: `rg -n "<keyword>" docs openmoc src`).
