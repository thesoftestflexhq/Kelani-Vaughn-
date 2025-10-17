.
├── src/
│   ├── main.py              # entry point (uvicorn runner)
│   ├── server.py            # FastAPI app + endpoints
│   ├── llm.py               # OpenAI client + tool-calling loop
│   ├── tools.py             # tool definitions + executors
│   ├── memory.py            # SQLiteMemory (per-session)
│   ├── schemas.py           # Pydantic models
│   ├── prompt.py            # KELANI_VAUGHN_SYSTEM_PROMPT
│   └── config.py            # environment settings loader
├── requirements.txt
├── .env.example             # copy to .env and fill in OPENAI_API_KEY
├── Dockerfile
├── fly.toml                 # (optional) Fly.io deploy config
├── render.yaml              # (optional) Render deploy blueprint
├── .dockerignore
└── web/
    └── index.html           # (optional) tiny browser chat UI
