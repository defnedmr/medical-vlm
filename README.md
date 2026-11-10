# Multi-Agent Medical AI

<p align="center">
  <strong>Research prototype for structured reporting and similar-case retrieval in renal nuclear imaging.</strong>
</p>

<p align="center">
  <img alt="Project status" src="https://img.shields.io/badge/status-in%20design-2563EB?style=flat-square">
  <img alt="Runtime" src="https://img.shields.io/badge/runtime-local%20only-0F766E?style=flat-square">
  <img alt="Model family" src="https://img.shields.io/badge/model-LingShu-7C3AED?style=flat-square">
  <img alt="License" src="https://img.shields.io/badge/use-research%20prototype-F59E0B?style=flat-square">
</p>

> [!IMPORTANT]
> This system is for research and preliminary decision support only. It does not provide autonomous diagnosis and every draft must be reviewed by a qualified physician.

## What it will do

The prototype will accept static and/or pinhole renal images, then provide one of three local workflows:

| Workflow | Result |
| --- | --- |
| **Generate report** | Structured draft with findings, impression, and uncertainty notes |
| **Find similar cases** | Top-k historical training cases with reports and similarity scores |
| **Full assessment** | Generated report, validation flags, retrieved cases, and agent trace |

## System flow

```text
Input images + selected task
            │
            ▼
    LangGraph orchestrator
       ├── report generation
       ├── output validation
       └── similar-case retrieval
            │
            ▼
Structured draft + flags + supporting cases
            │
            ▼
       Gradio interface
```

## Planned stack

| Layer | Technology |
| --- | --- |
| Medical VLM | LingShu 7B for experiments; LingShu 32B when feasible |
| Fine-tuning | LoRA / QLoRA, PyTorch, Transformers, PEFT, LLaMA-Factory |
| Retrieval | LingShu vision embeddings or BiomedCLIP baseline with FAISS |
| Orchestration | LangGraph state graph |
| Interface | Gradio |
| Experiment tracking | Local MLflow |
| Storage | Parquet metadata, SQLite logs, file-based images/checkpoints/indexes |
| Quality | pytest and ruff |

## Repository map

```text
.
├── configs/                 # Versioned data, model, training, retrieval, and app settings
├── docs/                    # Architecture, experiment protocol, and project documentation
├── src/
│   ├── agent/               # LangGraph task routing and state
│   ├── app/                 # Gradio demonstration interface
│   ├── data/
│   │   ├── preparation/     # Anonymisation, quality checks, manifests, and splits
│   │   └── schemas/         # Data contracts
│   ├── evaluation/          # Report, retrieval, and end-to-end metrics
│   ├── inference/           # Multi-image report generation
│   ├── retrieval/           # Embedding, FAISS indexing, and ranking
│   ├── training/            # Fine-tuning launchers
│   │   └── launchers/
│   └── validation/          # Report schema and unsupported-finding checks
└── tests/
    ├── evaluation/
    ├── integration/
    └── unit/
```

## Development setup (agentic tooling)

We develop with [Claude Code](https://claude.com/claude-code) and three token-efficiency tools. Program installs live on each machine, so **every team member runs the steps below once**. The shared parts (plugin settings, project rules, knowledge graph) come with the repository.

| Tool | Purpose | Installed | Shared via repo |
| --- | --- | --- | --- |
| [graphify](https://github.com/safishamsi/graphify) | Knowledge graph of code + docs so Claude queries a graph instead of re-reading files | Per machine | `graphify-out/graph.json`, `GRAPH_REPORT.md` |
| [rtk](https://github.com/rtk-ai/rtk) (Rust Token Killer) | Compresses shell command output (git, tests, ls…) before it reaches the model | Per machine (global hook) | — |
| [ponytail](https://github.com/DietrichGebert/ponytail) | Plugin that keeps generated code minimal | Per machine | `.claude/settings.json` (optional, auto-prompts install) |

**Prerequisites:** Claude Code, Python 3.10+, Git.

### 1. graphify

```bash
pip install graphifyy
graphify install
```

The PyPI name is `graphifyy` (two y's); the command is `graphify`. On Windows, if `graphify` is not found, add your Python `Scripts` folder to `PATH` or use `pipx install graphifyy`.

Optional — keep the graph updated on every commit and merge `graph.json` without conflicts:

```bash
graphify hook install
```

### 2. rtk

```bash
# Windows
winget install rtk-ai.rtk
# macOS / Linux
brew install rtk
```

Verify you have the right `rtk` (there is an unrelated crate with the same name), then register the Claude Code hook:

```bash
rtk gain                      # must print the savings dashboard, not "unknown command"
rtk init -g --auto-patch      # adds a global PreToolUse hook to ~/.claude/settings.json
```

The hook only rewrites Bash tool calls. Do **not** add the rtk hook to the project's `.claude/settings.json`: it would break Bash for teammates without rtk.

### 3. ponytail

Inside Claude Code:

```text
/plugin marketplace add DietrichGebert/ponytail
/plugin install ponytail@ponytail
```

When the repository contains `.claude/settings.json` with the block below, Claude Code offers to install the plugin when you trust the folder:

```json
{
  "extraKnownMarketplaces": {
    "ponytail": { "source": { "source": "github", "repo": "DietrichGebert/ponytail" } }
  },
  "enabledPlugins": { "ponytail@ponytail": true }
}
```

Only install from `DietrichGebert/ponytail`. Useful commands: `/ponytail [lite|full|ultra|off]`, `/ponytail-review`, `/ponytail-audit`, `/ponytail-gain`.

### 4. Restart and check

Fully restart Claude Code, then:

- `/graphify .` builds the graph on the first run. If `graphify-out/graph.json` is already in the repo, use it and run `graphify update .` after code changes (no LLM needed).
- `rtk gain` and `/ponytail-gain` show the savings.

> [!WARNING]
> graphify sends non-code files (docs, PDFs, images) to the model to extract concepts. It skips paths in `.gitignore` / `.graphifyignore`, so keep all patient data, images, and reports in ignored folders and **never** run `/graphify` on a data directory.

## Research safeguards

- All images, reports, model checkpoints, and retrieval indexes remain on access-restricted local storage.
- No patient data is sent to external runtime services.
- Raw data, checkpoints, indexes, and run artifacts must never be committed to Git.
- Dataset splits are patient-level; the retrieval index is built from training-split cases only.
