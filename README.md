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

## Research safeguards

- All images, reports, model checkpoints, and retrieval indexes remain on access-restricted local storage.
- No patient data is sent to external runtime services.
- Raw data, checkpoints, indexes, and run artifacts must never be committed to Git.
- Dataset splits are patient-level; the retrieval index is built from training-split cases only.
