cd platform
docker compose up -d --build backend

# Test RCA endpoint (will use mock if no API key)
curl -X POST http://localhost:8000/api/incidents/test-123/investigate

# Should return JSON with:
# {summary, root_cause, evidence, suggested_fix, confidence}