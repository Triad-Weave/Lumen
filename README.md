# Lumen

**Permission-aware AI knowledge assistant with source-grounded answers.**

Lumen helps teams find information across their documents without compromising access permissions. It uses retrieval-augmented generation (RAG) to answer questions from relevant document passages and provides citations that open the original sources.

Lumen is part of the [Triad-Weave](https://github.com/Triad-Weave) mentorship initiative, alongside Sentinel and Folio. The three applications are designed to run on a common cloud-native platform.

> **Project status:** In development. The initial architecture is proposed and pending design review. The capabilities and technology stack below describe the intended implementation, not completed functionality.

## Core Features

- **Workspace management:** Support multiple workspaces, folders, and individual folder-level read permissions.
- **Document management:** Upload PDF and Markdown documents individually or in batches of up to 300, track ingestion progress, retry failed documents, and delete documents.
- **Scalable ingestion:** Process documents asynchronously, prevent duplicate chunks, and scale workers according to queue backlog.
- **Permission-aware RAG:** Enforce the user's current access scope inside both keyword and vector retrieval, before evidence reaches the model.
- **Hybrid search:** Combine PostgreSQL full-text and vector search with reranking to improve retrieval relevance.
- **Streaming answers and citations:** Stream grounded answers with sentence-level citations linked to the exact source passages; abstain when authorized evidence is insufficient.
- **Evaluation:** Measure retrieval quality, answer faithfulness, citation accuracy, permission leakage, latency, and model cost using repeatable evaluation suites.

## Architecture

Lumen is designed around two independent processing paths that share a single PostgreSQL search index:

| Path               | Processing flow                                                                                                       |
| ------------------ | --------------------------------------------------------------------------------------------------------------------- |
| Document ingestion | Upload → Object storage → Queue → Parse and chunk → Embed → Index                                                     |
| Question answering | Authenticate → Resolve access scope → Hybrid retrieval → Rerank → Generate and validate → Stream answer and citations |

The application consists of a web frontend, a modular backend API, and independently scalable ingestion workers. Shared infrastructure is managed by the Triad-Weave platform rather than provisioned separately by Lumen.

Authorization is enforced by application and database logic, not by model instructions alone. Access changes must apply to the user's next question without re-indexing documents, and model calls must go through the shared LLM gateway.

For component diagrams, deployment topology, data models, workflows, security controls, and architectural decisions, see the [software architecture](docs/architecture.md).

## Proposed Technology Stack

| Layer                         | Technology                                        |
| ----------------------------- | ------------------------------------------------- |
| Frontend                      | Next.js, React, TypeScript                        |
| Backend and ingestion workers | Python, FastAPI, Pydantic                         |
| Database and search           | PostgreSQL, pgvector, PostgreSQL full-text search |
| Data access and migrations    | SQLAlchemy, Alembic, psycopg                      |
| Document processing           | PyMuPDF and Markdown parsing                      |
| Shared messaging and storage  | RabbitMQ (proposed), S3-compatible object storage |
| Shared deployment and scaling | Kubernetes, Helm, GitOps, KEDA                    |
| AI integration                | Shared LLM gateway                                |
| Observability                 | OpenTelemetry and shared monitoring dashboards    |

Platform-wide technology choices and model selections are subject to team agreement and design review.

## Repository Structure

The following is the proposed repository layout. Application and infrastructure directories will be introduced as implementation progresses.

```text
lumen/
├── apps/
│   ├── web/                 # Next.js frontend
│   └── backend/             # API, ingestion workers, migrations and tests
├── eval/                    # Evaluation datasets, prompts, runner and reports
├── infra/                   # Product deployment configuration
├── docs/
│   ├── architecture.md
│   ├── requirements-traceability.md
│   └── ai-assisted-development-log.md
└── README.md
```

## Documentation

| Document                                                           | Description                                                                                            |
| ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------ |
| [Software Architecture](docs/architecture.md)                      | System design, technology selection, architecture diagrams, ADRs and implementation roadmap            |
| [Requirements Traceability](docs/requirements-traceability.md)     | Functional, non-functional and platform requirements mapped to planned implementation and verification |
| [AI-Assisted Development Log](docs/ai-assisted-development-log.md) | Weekly record of AI-assisted work, engineering decisions and reviewed outputs                          |

## Evaluation and Quality Targets

The evaluation strategy includes a golden set of at least 40 questions with expected source passages and a leak suite of at least 30 adversarial questions tested under two different user scopes.

| Metric                             | Target                       |
| ---------------------------------- | ---------------------------- |
| Permission leaks                   | Zero in every evaluation run |
| Retrieval recall@5                 | At least 80% (baseline)      |
| Answer faithfulness                | At least 90% (baseline)      |
| Median time to first words         | At most 3 seconds            |
| Ingestion of 300 typical documents | At most 15 minutes           |
| Average LLM cost per answer        | At most USD 0.03 (baseline)  |

These are SRS targets, not measured results. Baseline-marked targets will be confirmed with the mentor following the first evaluation run.

## Getting Started

Lumen is currently in the architecture and planning phase. Local development prerequisites, environment configuration, platform bring-up, and application startup commands will be documented once the initial implementation and shared platform are available.

For now, start by reviewing the [architecture proposal](docs/architecture.md) and [requirements traceability matrix](docs/requirements-traceability.md).

## Development Roadmap

| Gate   | Milestone                                                                              |
| ------ | -------------------------------------------------------------------------------------- |
| Day 5  | Architecture and shared platform design review                                         |
| Day 10 | GitOps deployment, document ingestion, and a first scoped answer with citations        |
| Day 15 | Golden-set and leak-suite evaluation baselines on the shared dashboard                 |
| Day 20 | Final demonstration, Must requirements, zero-leak evaluation, and measured improvement |

## Development Guidelines

- Keep shared infrastructure changes in the platform repository.
- Submit changes through reviewed pull requests, with tests and updated documentation for affected core logic.
- Never commit secrets, provider keys, or real personal or company documents. Use public or generated test documents only.
- Keep model access within the shared gateway and document evaluation results and architectural trade-offs.
