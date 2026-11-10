# Graph Report - medical-vlm  (2026-11-10)

## Corpus Check
- Corpus is ~1,059 words - fits in a single context window. You may not need a graph.

## Summary
- 49 nodes · 72 edges · 8 communities (6 shown, 2 thin omitted)
- Extraction: 76% EXTRACTED · 24% INFERRED · 0% AMBIGUOUS · INFERRED: 17 edges (avg confidence: 0.87)
- Token cost: 82,075 input · 0 output

## Community Hubs (Navigation)
- Project Scope & Proposal
- Requirements & Agentic Design
- Retrieval & Data Safety
- Report Generation & Fine-tuning
- Orchestration & Interface
- Agentic Dev Tooling
- Storage Layer

## God Nodes (most connected - your core abstractions)
1. `CSE 491 Project Proposal (Group 3)` - 11 edges
2. `Similar-case retrieval` - 7 edges
3. `Data safety` - 6 edges
4. `LangGraph orchestrator` - 6 edges
5. `LingShu` - 6 edges
6. `Agentic workflow` - 6 edges
7. `Multi-Agent Medical AI` - 5 edges
8. `Structured draft report generation` - 5 edges
9. `Report generation` - 4 edges
10. `LingShu (7B / 32B)` - 4 edges

## Surprising Connections (you probably didn't know these)
- `LingShu (7B / 32B)` --semantically_similar_to--> `LingShu`  [INFERRED] [semantically similar]
  README.md → docs/Project_Proposal_Group3.pdf
- `Held-out test set` --semantically_similar_to--> `Patient-level splits / training-only retrieval index`  [INFERRED] [semantically similar]
  docs/Project_Proposal_Group3.pdf → README.md
- `Data safety` --implements--> `Patient data protection (NFR-1)`  [INFERRED]
  CLAUDE.md → docs/Project_Proposal_Group3.pdf
- `Multi-Agent Medical AI` --references--> `Decision support, not autonomous diagnosis`  [INFERRED]
  README.md → docs/Project_Proposal_Group3.pdf
- `LangGraph orchestrator` --implements--> `Agentic workflow`  [INFERRED]
  README.md → docs/Project_Proposal_Group3.pdf

## Hyperedges (group relationships)
- **Orchestrator routes report generation, validation, and retrieval** — readme_langgraph_orchestrator, readme_report_generation, readme_output_validation, readme_similar_case_retrieval, readme_gradio_interface [EXTRACTED 1.00]
- **Agentic dev tooling (graphify, rtk, ponytail)** — readme_graphify, readme_rtk, readme_ponytail, claude_tooling [EXTRACTED 1.00]
- **Evaluation metrics suite** — docs_project_proposal_group3_report_metrics, docs_project_proposal_group3_retrieval_metrics, docs_project_proposal_group3_system_metrics, docs_project_proposal_group3_held_out_test_set [EXTRACTED 1.00]

## Communities (8 total, 2 thin omitted)

### Community 0 - "Project Scope & Proposal"
Cohesion: 0.31
Nodes (9): Project rules for Claude Code, Held-out test set, HuatuoGPT-Vision, LingShu, CSE 491 Project Proposal (Group 3), Renal nuclear imaging dataset (static and pinhole, ~2000 patients), Risks and mitigation, Work plan WP1-WP7 (+1 more)

### Community 1 - "Requirements & Agentic Design"
Cohesion: 0.22
Nodes (7): Agentic workflow, Non-functional requirements NFR-1..NFR-4, Observe, Align, and Enhance, Output validation, Retrieval metrics (Precision@5, MRR), Similar-case retrieval, System metrics (task completion rate, end-to-end latency)

### Community 2 - "Retrieval & Data Safety"
Cohesion: 0.31
Nodes (6): BiomedCLIP baseline, FAISS, Find similar cases workflow, graphify, Local MLflow, Similar-case retrieval

### Community 3 - "Report Generation & Fine-tuning"
Cohesion: 0.22
Nodes (8): MediVLM, Report metrics (ROUGE-L, BERTScore, RaTEScore), Structured draft report generation, Generate report workflow, LingShu (7B / 32B), LLaMA-Factory, LoRA / QLoRA fine-tuning, Report generation

### Community 4 - "Orchestration & Interface"
Cohesion: 0.40
Nodes (6): Functional requirements FR-1..FR-7, Full assessment workflow, Gradio interface, LangGraph orchestrator, Output validation, Repository map

### Community 5 - "Agentic Dev Tooling"
Cohesion: 0.67
Nodes (3): Tooling, ponytail plugin, rtk (Rust Token Killer)

## Knowledge Gaps
- **10 isolated node(s):** `LLaMA-Factory`, `BiomedCLIP baseline`, `Local MLflow`, `Storage (Parquet, SQLite, file-based)`, `rtk (Rust Token Killer)` (+5 more)
  These have ≤1 connection - possible missing edges or undocumented components. (Counts symbols only; 11 node(s) total have ≤1 connection when file, concept and rationale nodes are included.)
- **2 thin communities (<3 nodes) omitted from report** — run `graphify query` to explore isolated nodes.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `Similar-case retrieval` connect `Retrieval & Data Safety` to `Requirements & Agentic Design`, `Report Generation & Fine-tuning`, `Orchestration & Interface`?**
  _High betweenness centrality (0.266) - this node is a cross-community bridge._
- **Are the 2 inferred relationships involving `Similar-case retrieval` (e.g. with `Find similar cases workflow` and `Similar-case retrieval`) actually correct?**
  _`Similar-case retrieval` has 2 INFERRED edges - model-reasoned connections that need verification._
- **What connects `LLaMA-Factory`, `BiomedCLIP baseline`, `Local MLflow` to the rest of the system?**
  _10 weakly-connected nodes found - possible documentation gaps or missing edges._
- **Why does `CSE 491 Project Proposal (Group 3)` connect `Project Scope & Proposal` to `Requirements & Agentic Design`, `Report Generation & Fine-tuning`, `Orchestration & Interface`?**
  _High betweenness centrality (0.247) - this node is a cross-community bridge._
- **Are the 2 inferred relationships involving `Data safety` (e.g. with `Patient data protection (NFR-1)` and `Research safeguards`) actually correct?**
  _`Data safety` has 2 INFERRED edges - model-reasoned connections that need verification._
- **Why does `Data safety` connect `Retrieval & Data Safety` to `Requirements & Agentic Design`?**
  _High betweenness centrality (0.169) - this node is a cross-community bridge._
- **Are the 2 inferred relationships involving `LingShu` (e.g. with `HuatuoGPT-Vision` and `LingShu (7B / 32B)`) actually correct?**
  _`LingShu` has 2 INFERRED edges - model-reasoned connections that need verification._