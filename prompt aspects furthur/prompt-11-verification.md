cd platform
docker compose up -d --build qdrant backend

# Check Qdrant UI
open http://localhost:6333/dashboard

# Test embedding
curl -X POST http://localhost:8000/api/incidents/test-123/investigate

# Check similar incidents
curl http://localhost:8000/api/incidents/test-123/similar

# Verify in frontend
open http://localhost:3000
# Run RCA, should see "Similar Past Incidents" section