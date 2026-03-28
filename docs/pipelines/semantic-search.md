# Semantic Search (Embeddings)

## Library Search Pipeline

```
On trip creation/update:
  → Generate text embedding of trip title + destinations + vibes + budget
  → Store in PostgreSQL (pgvector extension) or dedicated vector column
  → Index for fast cosine similarity search

On library search:
  → Embed search query with same model
  → Vector similarity search against all public trips
  → Combine with keyword filters (destination, budget, duration)
  → Return ranked results
```

## Model

Use Gemini `text-embedding-004` model (free tier: 1500 req/day). Each embedding is 768 dimensions.

## Storage

PostgreSQL supports the `pgvector` extension. Embeddings stored alongside trip data. No external vector DB needed.
