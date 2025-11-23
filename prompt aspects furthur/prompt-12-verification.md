cd platform
docker compose up -d --build neo4j backend

# Check Neo4j Browser
open http://localhost:7474
# Login: neo4j/password123

# Initialize graph with demo data
docker exec -it platform-backend python -m app.scripts.init_graph

# View in browser
MATCH (n) RETURN n LIMIT 25

# Test API
curl http://localhost:8000/api/graph/services
curl http://localhost:8000/api/graph/services/blog-service/incidents

# Open service map
open http://localhost:3000/service-map
```