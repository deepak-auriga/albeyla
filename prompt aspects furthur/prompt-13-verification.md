cd platform
docker compose up -d --build backend

# Check monitoring status
curl http://localhost:8000/api/monitoring/status

# Generate artificial spike in demo app
cd customer-environment/demo-app
for i in {1..50}; do 
  curl "http://localhost:8001/api/posts?filter=active" &
done

# Wait 60 seconds, check for auto-created incident
curl http://localhost:8000/api/incidents
# Should see new incident with "Anomaly detected" title

# Check anomalies
curl http://localhost:8000/api/monitoring/anomalies

# Verify in UI
open http://localhost:3000
# Should see monitoring status card + new incident
```