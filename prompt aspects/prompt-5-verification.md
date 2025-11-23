cd platform
docker compose up -d --build backend

# Create incident
curl -X POST http://localhost:8000/api/incidents

# List incidents
curl http://localhost:8000/api/incidents

# Run RCA (saves to database)
INCIDENT_ID="INC-xxxxx"  # Use ID from previous command
curl -X POST http://localhost:8000/api/incidents/$INCIDENT_ID/investigate

# Access database
docker exec -it platform-backend sqlite3 /data/sre_copilot.db
.tables
SELECT * FROM incidents;
.quit
```