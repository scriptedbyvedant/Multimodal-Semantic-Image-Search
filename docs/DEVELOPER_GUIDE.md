# Developer Guide

## Repo Layout
```
semantic_image_search/
  backend/       # API, embeddings, retrieval, ingestion
  ui/            # Streamlit UI
  notebooks/     # Experiments
```

## Local Development
1) Create a virtual environment and install dependencies.
2) Run Qdrant locally or point to a remote instance.
3) Export required environment variables or add a `.env` file.

## Environment Variables
- `QDRANT_URL` (required)
- `QDRANT_API_KEY` (optional)
- `QDRANT_COLLECTION` (default `semantic-image-search`)
- `OPENAI_API_KEY` (optional for query translation)
- `OPENAI_MODEL` (default `gpt-4o-mini`)
- `CLIP_MODEL_NAME` (default `ViT-B-32`)
- `CLIP_CHECKPOINT` (default `laion2b_s34b_b79k`)
- `VECTOR_SIZE` (default 512)
- `DEVICE` (default `cpu`)
- `IMAGES_ROOT`, `QUERY_IMAGE_ROOT`, `RETRIEVED_ROOT`

## Running Services
- Backend: `uvicorn semantic_image_search.backend.main:app --reload`
- UI: `streamlit run semantic_image_search/ui/app.py`

## Adding a New Embedding Model
1) Extend `EmbeddingLoader` in `semantic_image_search/backend/embeddings.py`.
2) Add env vars for model name/checkpoint.
3) Validate vector size and update `VECTOR_SIZE` accordingly.

## Adding a New Endpoint
1) Add a function in `semantic_image_search/backend/main.py`.
2) Implement the service layer in `backend/` as needed.
3) Update docs and add tests.

## Logging
- Uses structured logging in `semantic_image_search/backend/logger`.
- Keep logs consistent with `log.info` / `log.error` and include context.

## Testing (Suggested)
- Unit tests for embedding and query translation wrappers.
- Integration tests for Qdrant indexing and search.
- API contract tests for all endpoints.

## Release Checklist
- Update README and docs.
- Validate `.env` defaults.
- Smoke test ingestion and search.
