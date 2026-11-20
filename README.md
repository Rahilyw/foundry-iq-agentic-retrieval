# Foundry IQ — Agentic Retrieval & Knowledge Graphs

![Python](https://img.shields.io/badge/Python-3.11+-3776AB?logo=python&logoColor=white)
![Azure AI Search](https://img.shields.io/badge/Azure_AI_Search-Agentic_Retrieval-0078D4?logo=microsoft-azure&logoColor=white)
![Foundry IQ](https://img.shields.io/badge/Foundry_IQ-Knowledge_Bases-5C2D91?logo=microsoft&logoColor=white)
![GQL](https://img.shields.io/badge/Graph-GQL-00B4D8)

A hands-on Foundry IQ project that goes beyond classic chunk RAG: build a multi-index knowledge base with citations, compare **agentic retrieval** vs **microsoft-retriever-1**, then answer relationship questions with an **ontology-backed entity graph** and theme questions with a **topic graph**.

Built at **Microsoft Ignite 2026** (LAB673: Foundry IQ — Hands-on with knowledge graphs and agentic retrieval).

---

## The Problem It Solves

Vector search over PDF chunks is a strong baseline, but it struggles when you need:

- **Citations** that follow answers back to source documents across multiple indexes
- **Relationship answers** (“who supplies what to whom?”) that require explicit edges, not nearest neighbors
- **Theme routing** across a large corpus when the right documents cluster by topic, not keyword overlap
- A fair comparison of **standard agentic retrieval** vs a hosted retriever model on the same knowledge base

This project walks those patterns on a synthetic business corpus (**Cocoarynth**) with restored search indexes, OWL ontologies, and side-by-side notebooks.

---

## How It Works

```
┌─────────────────────────────────────────────────────────────────┐
│                        Foundry IQ                                │
│                                                                  │
│  Knowledge base ──▶ citations back to source documents           │
│                                                                  │
│  ┌──────────────────┐  ┌──────────────────┐  ┌────────────────┐ │
│  │ Chunk indexes    │  │ Entity graph     │  │ Topic graph    │ │
│  │ (PDF + embeddings│  │ OWL ontology +   │  │ labeled themes │ │
│  │  by doc group)   │  │ GQL + provenance │  │ + centroids    │ │
│  └────────┬─────────┘  └────────┬─────────┘  └───────┬────────┘ │
│           │                     │                     │          │
│           └──────────┬──────────┴──────────┬──────────┘          │
│                      ▼                     ▼                     │
│           Agentic retrieval      microsoft-retriever-1           │
│           (compare quality / latency side by side)               │
└─────────────────────────────────────────────────────────────────┘
```

### 1 — Knowledge base + retriever comparison
Restore chunk indexes, wire a Foundry IQ knowledge base, follow citations to sources, and measure standard agentic retrieval against **microsoft-retriever-1**.

### 2 — Entity graphs
Load an ontology-constrained business graph, run native **GQL**, inspect assertion provenance, and retrieve answers that follow explicit relationships across records.

### 3 — Topic graphs
Explore labeled topic groups, centroid-nearest chunks, hierarchy GQL, and topic-aware routing vs diversified vector search — then investigate a customer complaint with topics + PDF evidence.

A local **graph explorer** (`src/explorer/app.py`) visualizes ontology, entities, and topics that the Azure portal does not surface yet.

---

## What I Built / Completed

| Area | Outcome |
|------|---------|
| Knowledge base | Multi-index Foundry IQ KB over Cocoarynth document groups |
| Retrieval | Side-by-side agentic retrieval vs microsoft-retriever-1 |
| Entity graph | Ontology + GQL queries with graph provenance |
| Topic graph | Theme hierarchy, centroid routing, complaint investigation |
| Explorer | Local web UI for ontology / entities / topics |
| Infra | Bicep + scripts to deploy / restore indexes into your subscription |

---

## Project Structure

```
.
├── src/
│   ├── notebooks/
│   │   ├── 01-knowledge-base-and-mr1.ipynb
│   │   ├── 02-entities-and-graphs.ipynb
│   │   └── 03-topics.ipynb
│   └── explorer/app.py              # Local graph / topic explorer
├── data/
│   ├── cocoarynth/                  # Synthetic corpus assets
│   ├── graph-data/                  # OWL + graph/topic index snapshots
│   └── index-data/                  # Document-group chunk snapshots
├── infra/                           # Bicep, restore, and setup scripts
├── scripts/                         # Export / validate helpers
├── deploy_yourself.md               # Deploy to your own Azure subscription
└── azure.yaml
```

---

## Setup

### Prerequisites
- Azure subscription with access to Foundry IQ / Azure AI Search features used in the lab
- Python 3.11+ and the ability to create a local venv
- Azure CLI / `azd` for self-paced deploy (see `deploy_yourself.md`)

### Quick path

1. Clone this repo
2. Follow [deploy_yourself.md](deploy_yourself.md) to provision Azure resources and restore index snapshots
3. Copy `.env.sample` → `.env` and fill values from deployment output
4. Use the repo `.venv` as the notebook kernel and run:

| Notebook | Focus |
|----------|--------|
| `src/notebooks/01-knowledge-base-and-mr1.ipynb` | KB, citations, retriever comparison |
| `src/notebooks/02-entities-and-graphs.ipynb` | Entity graph, GQL, provenance |
| `src/notebooks/03-topics.ipynb` | Topic graph and complaint investigation |

### Graph explorer

```bash
python src/explorer/app.py
# open http://localhost:8765
```

---

## Tech Stack

| Component | Technology |
|-----------|------------|
| Knowledge layer | Microsoft Foundry IQ |
| Search | Azure AI Search (agentic retrieval, entity graph, topic graph) |
| Retriever model | microsoft-retriever-1 |
| Graph | OWL ontology + GQL |
| SDK | `azure-search-documents` |
| Infra | Bicep, Azure Developer CLI (`azd`) |
| Exploration | Python notebooks + local FastAPI/static explorer |

---

## Attribution

Lab content, Cocoarynth corpus, and index snapshots originate from Microsoft’s Ignite 2026 LAB673 materials, licensed under MIT (see `LICENSE` / `LICENSE-DOCS`). This repository is my portfolio build of that session — completed notebooks, explorer usage, and documentation rewritten for personal presentation.

---

*Built at Microsoft Ignite 2026 — LAB673: Foundry IQ: Hands-on with knowledge graphs and agentic retrieval*
