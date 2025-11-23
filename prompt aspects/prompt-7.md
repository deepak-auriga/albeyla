## ⏰ HOUR 8: Demo Script & Incident Trigger

### Prompt for Hour 8
```
I'm building an SRE Copilot platform. This is Hour 8 of 10.

GOAL: Add demo script to generate realistic incidents for impressive presentations

CONTEXT:
- Hours 1-7 COMPLETED: Full stack working with beautiful UI
- Need way to trigger demo incidents reliably
- Should generate traces, logs, metrics that correlate

REQUIREMENTS:

1. Create `customer-environment/demo-app/trigger_incident.py`:
   - Python script that generates demo workload
   - Uses OpenTelemetry SDK to create spans
   - Function: simulate_slow_query()
     * Create span "db_query_users"
     * sleep(2.5) to simulate slow query
     * Log error with trace_id: "SLOW_DB_QUERY: Missing index..."
     * Set span attribute error=True
   - Function: simulate_normal_query()
     * sleep(0.05) - 50ms, normal
   - Function: run_demo_workload()
     * Generate 15 requests total
     * 5 slow (incident), 10 normal (baseline)
     * Print progress
   - Resource attributes: deploy.version=v4521, deploy.commit=abc123

2. Create `platform/backend/app/routes/demo.py`:
   - APIRouter with prefix /api/demo
   - POST /trigger-incident:
     * Create new Incident in database
     * incident_id: INC-{random}
     * tenant_id: demo-tenant-001
     * service_name: blog-service
     * severity: high
     * title: "High latency detected on /api/posts endpoint"
     * description: "P95 latency increased from 200ms to 2500ms after deploy v4521"
     * Return incident data
   - Add comment: "In production, would trigger background job"

3. Update `platform/backend/app/main.py`:
   - Import and include demo router

4. Update `platform/frontend/src/pages/Dashboard.jsx`:
   - Add triggerDemoIncident() function:
     * fetch POST http://localhost:8000/api/demo/trigger-incident
     * alert success message
     * refresh incidents list
   - Add button above incidents list:
     * "🔥 Trigger Bad Deploy (Demo)"
     * Orange button, large (18px font, 16px padding)
     * onClick: triggerDemoIncident()

5. Create `docs/DEMO_SCRIPT.md`:
   - Complete step-by-step demo guide
   - 5-minute presentation flow:
     * Step 1: Show healthy system (30s)
     * Step 2: Trigger bad deploy (1m)
     * Step 3: Watch AI investigate (2m)
     * Step 4: Show correlated data (1.5m)
     * Step 5: Optional - show raw data in Jaeger/Loki
   - Include "what to say" script for presenter
   - Troubleshooting section
   - Backup plans if something breaks

6. Add demo instructions to `customer-environment/README.md`

OUTPUT FORMAT:
- Complete file contents
- Make script executable (#!/usr/bin/env python3)
- Include usage examples
- Demo script must be foolproof

CONSTRAINTS:
- Script must generate data that appears in Jaeger within 30s
- Frontend button must work without manual terminal commands
- Demo must be repeatable (can run multiple times)
- Should take exactly 5 minutes for full demo