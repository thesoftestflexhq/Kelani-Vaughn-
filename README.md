# Kelani Vaughn API

FastAPI + OpenAI tool-calling chat service for the Kélani Vaughn “The Softest Flex” brand.

## Run locally

```bash
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env   # add your real OPENAI_API_KEY
python -m src.main     # http://localhost:8000/docs
