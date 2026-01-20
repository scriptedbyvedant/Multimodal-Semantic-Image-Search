# User Guide

## Prerequisites
- Python 3.11+
- Qdrant running locally or remotely

## Setup
1) Create and activate a virtual environment.
2) Install dependencies:
```bash
pip install -r requirements.txt
```

3) Configure environment variables in `.env` at repo root:
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

## Run Backend API
```bash
uvicorn semantic_image_search.backend.main:app --reload
```

## Run Streamlit UI
```bash
streamlit run semantic_image_search/ui/app.py
```

## Ingest Images
- Place images under `images/` with subfolders as categories.
- Call the ingestion endpoint:
```bash
curl -X POST "http://localhost:8000/ingest?folder_path=images"
```

## Search by Text
```bash
curl "http://localhost:8000/search-text?q=red%20sports%20car&k=5"
```

## Search by Image
```bash
curl -X POST "http://localhost:8000/search-image" \
  -F "file=@/path/to/query.jpg" \
  -F "k=5"
```

## Save Results Locally
Add `save_results=true` to store results under `data/retrieved/`:
```bash
curl "http://localhost:8000/search-text?q=cat&k=5&save_results=true"
```
