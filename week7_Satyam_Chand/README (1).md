# Document Question Answering System (RAG)

A Retrieval-Augmented Generation pipeline for answering questions over your own
PDFs / text files, grounded in retrieved content rather than the model's memory alone.

## Architecture

```
documents/ (PDF/.txt)
      │
      ▼
document_loader.py   → raw text per page/file
      │
      ▼
chunker.py            → overlapping text chunks
      │
      ▼
embedder.py            → vector embeddings (sentence-transformers)
      │
      ▼
vector_store.py        → FAISS index (persisted to disk)
      │
      ▼  (query time)
embedder.py             → embed the user's question
      │
      ▼
vector_store.py         → retrieve top-k similar chunks
      │
      ▼
rag_pipeline.py         → Groq generates an answer
```

## Tuning / experiments (from the project brief)

All in `config.py`:
- `CHUNK_SIZE` / `CHUNK_OVERLAP` — try different chunking strategies
- `EMBEDDING_MODEL` — swap `all-MiniLM-L6-v2` for `all-mpnet-base-v2` (higher quality, slower)
- `TOP_K` — more retrieved chunks = more context, but more noise too
- `GENERATION_MODEL` — swap generation models

Ideas for further improvement, not yet implemented here:
- Hybrid search (keyword/BM25 + vector) instead of pure vector similarity
- A re-ranking step (e.g. cross-encoder) after initial retrieval
- Swapping FAISS for a hosted vector DB (Pinecone, Weaviate, Chroma) if scaling beyond one machine

