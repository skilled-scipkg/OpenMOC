# openmoc source map: Advanced Topics

Generated from source roots:
- `openmoc`
- `src`

Use this map only after exhausting the topic docs in `doc_map.md`.

## Topic query tokens
- `documentation`
- `license`
- `citation`
- `publications`
- `docstring`
- `sphinx`

## Fast source navigation
- `rg -n "<keyword>" docs openmoc src README.rst LICENSE`
- `rg -n "extensions|intersphinx|html_theme" docs/source/conf.py`

## Suggested source entry points
- `docs/source/conf.py` | Sphinx build behavior and documentation pipeline settings
- `openmoc/swig/docstring.i` | SWIG docstring bridge generated from Doxygen comments
- `openmoc/__init__.py` | package metadata/bootstrap text shown to users
- `README.rst` | project-level usage/citation context
- `LICENSE` | canonical redistribution/legal text
- For license/publication wording, docs in `doc_map.md` are authoritative and source escalation is usually unnecessary.
