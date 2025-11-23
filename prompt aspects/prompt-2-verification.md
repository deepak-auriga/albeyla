cd customer-environment
docker compose up -d
docker compose ps
curl http://localhost:8001/health
curl http://localhost:8001/api/posts
curl "http://localhost:8001/api/posts?filter=active"
docker logs customer-otel-collector | grep "Traces"
# Then check Jaeger UI at localhost:16686 for "blog-service"