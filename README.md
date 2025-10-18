# MediCare AI

MediCare AI is a FastAPI backend that provides medical AI assistant capabilities for Cameroon. It leverages LangChain and Google Gemini for chat and image/text analysis, and integrates with Tavily for research searches.

This repository contains the backend service which exposes REST endpoints for health checks, chat, medical text analysis, image text extraction/analysis, and research search.

## Features

- Chat with a medical AI assistant
- Analyze medical text and return structured findings and recommendations
- Extract text from images and optionally analyze the extracted content
- Search and summarize medical research results
- OpenAPI docs available at `/docs`

## Repo layout

backend/
- app/
  - config.py        # application settings and LLM loaders
  - main.py          # FastAPI app + route registration
  - chains/          # domain logic (chat + analysis chains)
  - models/          # request/response pydantic schemas
  - routes/          # API route definitions (health, analysis, research)
  - services/        # integrations (Google Gemini wrapper, Tavily)
- requirements.txt

## Prerequisites

- Python 3.10+ (recommended)
- A Google Gemini API key (set in environment variable `GOOGLE_API_KEY`)
- A Tavily API key (set in environment variable `TAVILY_API_KEY`)

Install dependencies (from the `backend` folder):

```powershell
python -m pip install -r backend\requirements.txt
```

## Configuration

The application uses a `.env` file at the root of the backend (or environment variables). The following environment variables are used by `app.config.Settings`:

- GOOGLE_API_KEY (required) — Google Gemini API key
- TAVILY_API_KEY (required) — Tavily API key
- HOST — host to bind (default 0.0.0.0)
- PORT — port to run on (default 8000)
- CORS_ORIGINS — comma-separated list of allowed origins (default http://localhost:3000)
- GEMINI_MODEL — Gemini model id (default `gemini-2.0-flash-exp`)
- TEMPERATURE — model temperature (default 0.7)
- MAX_TOKENS — max output tokens (default 2048)

Example `.env` (place in `backend/.env`):

```text
GOOGLE_API_KEY=your_google_gemini_api_key
TAVILY_API_KEY=your_tavily_api_key
HOST=0.0.0.0
PORT=8000
CORS_ORIGINS=http://localhost:3000
GEMINI_MODEL=gemini-2.0-flash-exp
TEMPERATURE=0.7
MAX_TOKENS=2048
```

## Running the server (development)

From the workspace root, run:

```powershell
cd backend; uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

This will start the FastAPI server and enable automatic reload on code changes. Open `http://localhost:8000/docs` to explore the API using Swagger UI.

## API Endpoints

All endpoints are prefixed with `/api` except the root and docs.

- GET /api/health — health check (returns status, timestamp, message)
- POST /api/chat — chat with the AI
  - Request: { "message": "...", "language": "en" }
  - Response: { "response": "...", "language": "en", "timestamp": "..." }
- POST /api/analyze-text — analyze medical text
  - Request: { "text": "...", "context": "optional context", "language": "en" }
  - Response: structured analysis (summary, key_findings, recommendations, next_steps, disclaimer, timestamp)
- POST /api/analyze-image — upload image for OCR + analysis
  - multipart form: `file` (image), `language` (opt), `extract_text_only` (bool)
- POST /api/extract-text — upload image and return extracted text only
- POST /api/research — search medical research via Tavily
  - Request: { "query": "...", "max_results": 5, "language": "en" }
  - Response: list of results and a short summary

Use the interactive docs at `/docs` for example requests and response schemas.

## Development notes

- The core prompt logic lives in `app/chains`.
- Gemini client loader functions are in `app/config.py` (`load_google_llm` and `load_google_vision_llm`) and are cached for reuse.
- Services wrappers are in `app/services` (e.g., `gemini_service.py`, `tavily_service.py`).

## Testing

There are no automated tests included by default. For manual testing, use `curl`, `httpie`, Postman, or the Swagger UI (`/docs`). Example using `curl` for health check:

```powershell
curl http://localhost:8000/api/health
```

## Security & Disclaimer

This project integrates third-party LLMs and performs medical-related analysis. Outputs are for informational purposes only and should not be relied on as medical advice. The API responses include disclaimers in analysis endpoints. Always follow privacy and data protection best practices when handling patient data.

## Next steps / Improvements

- Add unit and integration tests for the chains and service wrappers
- Add Dockerfile and docker-compose for local and production deployment
- Add CI workflow for linting and tests

## License

Add your chosen license here (e.g., MIT). If none is provided, treat the repository as proprietary.
