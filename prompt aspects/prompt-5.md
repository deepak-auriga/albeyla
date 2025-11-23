## ⏰ HOUR 5: Database Models & Incidents API

### Prompt for Hour 5
```
I'm building an SRE Copilot platform. This is Hour 5 of 10.

GOAL: Add database persistence for incidents and RCA reports

CONTEXT:
- Hours 1-4 COMPLETED: Stack running, RCA agent working
- Using SQLite for MVP (easy to migrate to Postgres later)
- Need to store: tenants, incidents, RCA reports

REQUIREMENTS:

1. Create `platform/backend/app/models.py`:
   - SQLAlchemy ORM models using declarative_base
   - Table: tenants
     * id (Integer, primary key)
     * tenant_id (String 50, unique, indexed)
     * name, api_key
     * created_at (DateTime, default utcnow)
   - Table: incidents
     * id (Integer, primary key)
     * incident_id (String 50, unique, indexed)
     * tenant_id (String 50, indexed)
     * service_name, severity (critical/high/medium/low)
     * status (open/investigating/resolved)
     * title, description (Text)
     * detected_at, resolved_at (DateTime)
   - Table: rca_reports
     * id (Integer, primary key)
     * incident_id (String 50, indexed)
     * summary, root_cause, evidence, suggested_fix (Text)
     * confidence (Float)
     * created_at (DateTime)

2. Create `platform/backend/app/schemas.py`:
   - Pydantic models for request/response validation
   - IncidentCreate: service_name, severity, title, description
   - IncidentResponse: all fields from model
   - RCAReportResponse: all fields from model
   - Use BaseModel, Field, datetime types

3. Update `platform/backend/app/db.py`:
   - Import Base from models
   - Add: Base.metadata.create_all(bind=engine)
   - This creates tables on startup

4. Create `platform/backend/app/routes/incidents.py`:
   - APIRouter with prefix /api
   - GET /incidents - list all incidents (limit 50, order by detected_at desc)
   - POST /incidents - create test incident (for demo)
     * Generate incident_id: INC-{uuid}
     * tenant_id: demo-tenant-001
     * service_name: blog-service
     * severity: high
     * title: "High latency detected on /api/posts endpoint"
     * Return created incident
   - Use Depends(get_db) for database session

5. Update `platform/backend/app/routes/investigate.py`:
   - After RCA generation, save report to database
   - Create RCAReport instance with incident_id
   - Commit to database

6. Update `platform/backend/app/main.py`:
   - Import and include incidents router
   - app.include_router(incidents.router)

7. Create example SQL in comments:
```sql
   -- Example queries for debugging:
   SELECT * FROM incidents;
   SELECT * FROM rca_reports WHERE incident_id = 'INC-123';
   SELECT i.*, r.confidence FROM incidents i 
   LEFT JOIN rca_reports r ON i.incident_id = r.incident_id;
```

OUTPUT FORMAT:
- Complete file contents
- Explain SQLAlchemy relationships
- Include database access commands
- Show example API calls

CONSTRAINTS:
- Tables must auto-create on first run
- Data must persist across container restarts (volume)
- All string fields have reasonable max lengths
- Use UTC timestamps everywhere