Demo Scenario
Simulated Incident Flow
T-0: Deploy broken code (DB query without index)
T+2min:

Data Ingestion Agent detects high DB latency in metrics
Embeddings show deviation from baseline

T+4min:

Incident Detection Agent creates alert: "DB_LATENCY_SPIKE"
Dashboard shows red indicator
Slack notification sent

T+5min:

RCA Agent auto-triggers:

Fetches DB slow query logs
Correlates with deployment timeline
Identifies recent code change
Searches knowledge graph (finds similar past incident)
Generates RCA: "New filter query missing index on user_id column"



T+7min:

Suggested Fix: "Add index on users.user_id" or "Rollback deployment v1.2.3"
Manual/Auto-remediation option

T+10min:

Predictive Agent flags: "API response times degrading, predict outage in 20 mins"