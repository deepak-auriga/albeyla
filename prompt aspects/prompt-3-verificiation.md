cd platform
docker compose up -d --build backend
docker compose logs -f backend
curl http://localhost:8000/health
curl http://localhost:8000/debug/config
# Should see masked ANTHROPIC_API_KEY