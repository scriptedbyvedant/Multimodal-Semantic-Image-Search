# Multimodal Semantic Image Search

A production-grade semantic image search system powered by CLIP embeddings, Qdrant vector search, FastAPI, and a Streamlit UI. It supports text-to-image and image-to-image retrieval, plus optional LLM-based query rewriting for improved relevance.

## Highlights
- Multimodal search: text to image and image to image.
- Scalable vector storage with Qdrant.
- Modular backend with clean service boundaries.
- Simple Streamlit UI for demos and exploration.

## Architecture
- HLD: `docs/ARCHITECTURE.md`
- LLD: `docs/LLD.md`

## Quickstart
1) Install dependencies:
```bash
pip install -r requirements.txt
```

2) Create `.env` in repo root:
```
QDRANT_URL=http://localhost:6333
QDRANT_API_KEY=
QDRANT_COLLECTION=semantic-image-search
OPENAI_API_KEY=
OPENAI_MODEL=gpt-4o-mini
CLIP_MODEL_NAME=ViT-B-32
CLIP_CHECKPOINT=laion2b_s34b_b79k
DEVICE=cpu
IMAGES_ROOT=images
```

3) Run the API:
```bash
uvicorn semantic_image_search.backend.main:app --reload
```

4) Run the UI:
```bash
streamlit run semantic_image_search/ui/app.py
```

## Core API
- `POST /ingest` - Index a folder of images.
- `GET /translate` - Rewrite a query with an LLM.
- `GET /search-text` - Text-to-image search.
- `POST /search-image` - Image-to-image search.

Example:
```bash
curl -X POST "http://localhost:8000/ingest?folder_path=images"
```

## Documentation
- Index: `docs/README.md`
- User Guide: `docs/USER_GUIDE.md`
- Developer Guide: `docs/DEVELOPER_GUIDE.md`
- Project Report: `docs/PROJECT_REPORT.md`
- Contributing: `docs/CONTRIBUTING.md`

## Contributing
See `docs/CONTRIBUTING.md` for guidelines.

## License
See `LICENSE`.
