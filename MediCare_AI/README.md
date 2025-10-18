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