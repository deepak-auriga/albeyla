## ⏰ HOUR 3: FastAPI Backend Skeleton

### Prompt for Hour 3
```
I'm building an SRE Copilot platform. This is Hour 3 of 10.

GOAL: Create FastAPI backend with configuration management and health endpoint

CONTEXT:
- Hours 1-2 COMPLETED: Observability stack + customer apps sending telemetry
- Now building the backend API that will query observability data
- Using SQLite for simplicity (not Postgres)
- Configuration pattern for easy debugging

REQUIREMENTS:

1. Create `platform/backend/requirements.txt`:
   - fastapi, uvicorn[standard]
   - sqlalchemy, aiosqlite
   - pydantic, pydantic-settings
   - httpx (for API calls later)
   - redis, anthropic
   - python-dotenv

2. Create `platform/backend/app/context.py`:
   - Pydantic BaseSettings class for configuration
   - Fields:
     * DB_URL (default: sqlite:////data/sre_copilot.db)
     * REDIS_URL, JAEGER_QUERY_URL, PROMETHEUS_URL, LOKI_URL
     * ANTHROPIC_API_KEY, ENV (dev/staging/prod)
   - Method: debug_info() - returns config with masked secrets
   - Function: get_context() with @lru_cache() for singleton
   - Load from .env file

3. Create `platform/backend/app/main.py`:
   - FastAPI app instance
   - CORS middleware (allow http://localhost:3000)
   - Startup event: load context, print debug_info()
   - Routes:
     * GET /health - returns {"status": "ok", "service": "sre-copilot-api"}
     * GET /debug/config - returns ctx.debug_info()
   - Include inline comments explaining each section

4. Create `platform/backend/app/db.py`:
   - SQLAlchemy setup for SQLite
   - Create engine with DB_URL from context
   - SessionLocal factory
   - get_db() dependency for FastAPI
   - Don't create tables yet (we'll add models in Hour 5)

5. Create `platform/backend/Dockerfile`:
   - FROM python:3.11-slim
   - WORKDIR /app
   - Copy and install requirements
   - Copy app/ directory
   - EXPOSE 8000
   - CMD: uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload

6. Update `platform/docker-compose.yml`:
   - Add backend service:
     * build: ./backend
     * ports: 8000:8000
     * environment: DB_URL, ANTHROPIC_API_KEY (from env)
     * volumes: ./backend/app:/app/app (hot reload), backend-data:/data
     * depends_on: redis
   - Add backend-data volume

7. Create `platform/backend/README.md`:
   - How to run backend locally
   - How to test endpoints
   - Environment variables needed

OUTPUT FORMAT:
- Complete file contents
- Explain Pydantic Settings pattern
- Explain singleton pattern with lru_cache
- Include test commands

CONSTRAINTS:
- Backend must start with: docker compose up -d --build backend
- /health must return 200 within 10 seconds
- Hot reload must work (change code, see logs update)