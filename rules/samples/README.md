# Sample Rules

Rules in this directory are **demonstration/tutorial entities** used by the desktop Sampler.

- These rules are seeded into the desktop SQLite database with `source: "sample"` provenance
- They are **not** included in `rulesentry-data` (the production data crate uses `rules/core/` only)
- Each rule should have `origin.source_type: "sample"` in its JSON

Production rules that double as good demos live in `rules/core/` — this directory is reserved for rules that exist purely for educational purposes.
