# Company Intelligence FAQ Chatbot
Full‑stack RAG starter repo (React + .NET 8 + Qdrant) that ingests company docs and answers questions with context.

## What you get
- **Backend**: ASP.NET Core 8 Web API
- **Vector DB**: Qdrant (Docker) — 3072‑dim cosine index for `text-embedding-3-large`
- **LLM**: OpenAI Chat Completions (`gpt-4o-mini` by default)
- **Embeddings**: OpenAI `text-embedding-3-large`
- **Frontend**: React + Vite (TypeScript) chat UI + file uploader
- **RAG flow**: ingest → chunk → embed → upsert → search → answer (+ sources)

> This is a production-quality starter: clean structure, .env driven, Swagger, CORS, and a simple ingestion pipeline.

---

## Quickstart (local dev)

### 1) Prereqs
- .NET 8 SDK
- Node 18+
- Docker (for Qdrant)
- An OpenAI API key

### 2) Bring up Qdrant
```bash
docker compose up -d qdrant
```

### 3) Backend
```bash
cd backend/src/CompanyIntelligence.Api
dotnet restore
# set env vars (Windows: setx; mac/linux: export in your shell)
export OPENAI_API_KEY=sk-...             # required
export QDRANT_URL=http://localhost:6333  # default
export QDRANT_COLLECTION=company_knowledge
export OPENAI_MODEL=gpt-4o-mini
export OPENAI_EMBEDDING_MODEL=text-embedding-3-large

dotnet run
# Swagger at http://localhost:5088/swagger
```

### 4) Frontend
```bash
cd frontend
npm i
# (optional) configure API base in .env (default http://localhost:5088)
npm run dev
# open http://localhost:5173
```

---

## RAG API
- `POST /api/ingest` — multipart file upload (`file` field). Parses PDF/TXT, chunks, embeds, and upserts to Qdrant.
- `POST /api/chat` — `{ "question": "What's our refund policy?" }` → returns `{ answer, sources[] }`

Both are documented in Swagger.

---

## Notes
- Collection is auto-created with vector size **3072** (OpenAI `text-embedding-3-large`).
- PDF parsing via **PdfPig**; TXT handled natively. You can extend to DOCX by adding an extractor.
- This starter favors clarity. Add auth (JWT/Auth0), caching (Redis), and reranking later.
- Deleting a collection will delete your stored vectors.

---

## License
MIT
