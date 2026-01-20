# Architecture (HLD)

## Overview
This project provides a multimodal semantic image search system with:
- A FastAPI backend for ingestion and search.
- A CLIP-based embedding layer for images and text.
- Qdrant as the vector database.
- An optional LLM query translator to rewrite user queries for better retrieval.
- A Streamlit UI for interactive search.

## Goals
- High-quality semantic retrieval for text-to-image and image-to-image queries.
- Clear modular separation between API, embeddings, vector store, and UI.
- Straightforward deployment with minimal configuration.

## Non-Goals
- Fine-tuning or training models.
- Managing large-scale distributed ingestion pipelines.

## System Context
```mermaid
flowchart LR
  User --> UI[Streamlit UI]
  User --> API[FastAPI API]
  UI --> API
  API --> Emb[Embedding Service (CLIP)]
  API --> Qdrant[(Qdrant Vector DB)]
  API --> FS[(Local Image Storage)]
  API --> LLM[Query Translator (OpenAI)]
```

## Component Diagram
```mermaid
flowchart TB
  subgraph Client
    UI[Streamlit UI]
    CLI[Optional curl/CLI]
  end

  subgraph Backend
    API[FastAPI]
    Ingest[IndexService]
    Search[ImageSearchService]
    Embed[OpenCLIPEmbeddings]
    QMgr[QdrantClientManager]
    QT[QueryTranslator]
    Cfg[Config]
  end

  UI --> API
  CLI --> API

  API --> Ingest
  API --> Search
  API --> QT

  Ingest --> Embed
  Ingest --> QMgr

  Search --> Embed
  Search --> QMgr
  Search --> FS[(Local Storage)]

  QT --> LLM[(OpenAI API)]
  Cfg --> API
  Cfg --> Ingest
  Cfg --> Search
  Cfg --> QT
  QMgr --> Qdrant[(Qdrant)]
```

## Data Flow - Ingestion
```mermaid
sequenceDiagram
  participant U as User
  participant API as FastAPI
  participant IDX as IndexService
  participant EMB as CLIP Embeddings
  participant QD as Qdrant

  U->>API: POST /ingest?folder_path=...
  API->>IDX: index_folder()
  IDX->>EMB: embed_image_paths()
  EMB-->>IDX: vectors
  IDX->>QD: upsert points
  QD-->>IDX: ack
  IDX-->>API: success
  API-->>U: 200 OK
```

## Data Flow - Text Search
```mermaid
sequenceDiagram
  participant U as User
  participant API as FastAPI
  participant QT as QueryTranslator
  participant EMB as CLIP Embeddings
  participant QD as Qdrant

  U->>API: GET /search-text?q=...
  API->>QT: translate_query()
  QT-->>API: rewritten caption
  API->>EMB: embed_text()
  EMB-->>API: vector
  API->>QD: query_points()
  QD-->>API: results
  API-->>U: ranked results
```

## Data Flow - Image Search
```mermaid
sequenceDiagram
  participant U as User
  participant API as FastAPI
  participant EMB as CLIP Embeddings
  participant QD as Qdrant

  U->>API: POST /search-image (file)
  API->>EMB: embed_single_image()
  EMB-->>API: vector
  API->>QD: query_points()
  QD-->>API: results
  API-->>U: ranked results
```

## Deployment View
```mermaid
flowchart LR
  subgraph Host
    API[FastAPI]
    UI[Streamlit]
    FS[(Local FS)]
  end
  Qdrant[(Qdrant)]
  API --> Qdrant
  UI --> API
  API --> FS
```

## Key Decisions
- Qdrant as the vector store for efficient ANN search.
- OpenCLIP embeddings via LangChain for both text and image vectors.
- Query translation with a lightweight LLM to improve semantic alignment.

## Scalability Notes
- Use GPU for embeddings by setting `DEVICE=cuda`.
- Increase Qdrant resources and enable on-disk vectors.
- Add a background task queue for ingestion if indexing large datasets.

## Security & Privacy
- API keys are sourced from environment variables.
- No user authentication implemented by default.
- Uploaded images are stored locally under `data/query_images`.
