# Project Report

## Executive Summary
This project implements a multimodal semantic image search engine using CLIP embeddings, Qdrant vector search, and a FastAPI backend with a Streamlit UI. It supports both text-to-image and image-to-image retrieval and provides an optional LLM query translator for improved semantic alignment.

## Objectives
- Enable semantic search over local image folders.
- Provide a simple API and UI for ingestion and retrieval.
- Maintain modular design for future extension.

## Scope
Included:
- Indexing images from local folders.
- Text-based and image-based retrieval.
- Optional LLM query rewriting.

Out of scope:
- Model training or fine-tuning.
- Large-scale distributed ingestion.
- Authentication and authorization.

## Architecture Overview
- Backend: FastAPI
- Embeddings: OpenCLIP via LangChain
- Vector DB: Qdrant
- UI: Streamlit

Refer to `docs/ARCHITECTURE.md` for details and diagrams.

## Key Features
- Folder-based ingestion with automatic category inference.
- Semantic search using cosine similarity.
- Optional query translation with an LLM.
- Save retrieved images locally for inspection.

## Dependencies
- Python 3.11+
- Qdrant (local or managed)
- OpenAI API (optional for query translation)

## Risks and Mitigations
- Vector DB unavailability: handle errors, surface logs.
- LLM latency/timeouts: request timeouts and optional bypass.
- Embedding performance: GPU support via `DEVICE=cuda`.

## Evaluation Plan
Suggested metrics for evaluation:
- Recall@K for curated test sets.
- Mean reciprocal rank (MRR) for text queries.
- Latency measurements for end-to-end requests.

## Future Enhancements
- Batch ingestion queue with job tracking.
- Multi-vector or hybrid retrieval (CLIP + text captioning).
- Multi-tenant support and auth.
- Model selection and evaluation dashboards.
