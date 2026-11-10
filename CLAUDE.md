# Project rules for Claude Code

Multi-Agent Medical AI: research prototype for structured reporting and similar-case retrieval in renal nuclear imaging. See `README.md` for the stack and repository map, and `docs/Project_Proposal_Group3.pdf` for the proposal.

## Tooling

- **graphify:** if `graphify-out/graph.json` exists, read `graphify-out/GRAPH_REPORT.md` and query the graph before broad Glob/Grep searches. After code changes, run `graphify update .`.
- **rtk:** a global hook may rewrite shell commands to `rtk ...`. That is expected; do not undo it.
- **ponytail:** write only the code a task needs.

## Data safety (non-negotiable)

- Never read, print, or upload patient images, reports, or metadata. Never send them to external services.
- Never commit raw data, checkpoints, FAISS indexes, MLflow runs, or logs. They are in `.gitignore`.
- Never run `/graphify` on a data directory. Keep data in ignored folders.
- Splits are patient-level; the retrieval index uses training-split cases only.

## Code

- Python, tests with `pytest`, lint/format with `ruff`.
- Config values go in `configs/`, not hard-coded.
