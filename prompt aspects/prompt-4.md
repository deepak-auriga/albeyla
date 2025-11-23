
## ⏰ HOUR 4: RCA Agent with Claude API

### Prompt for Hour 4
```
I'm building an SRE Copilot platform. This is Hour 4 of 10.

GOAL: Build AI agent that queries observability data and generates RCA reports

CONTEXT:
- Hours 1-3 COMPLETED: Observability stack, demo apps, backend skeleton
- Now adding the INTELLIGENCE layer
- Will query Jaeger, Loki, Prometheus APIs
- Use Claude API (Anthropic) for generating RCA

REQUIREMENTS:

1. Create `platform/backend/app/clients/jaeger_client.py`:
   - Class: JaegerClient
   - Method: async get_traces(service_name, start_time, limit=10)
     * Query Jaeger API: GET {JAEGER_URL}/api/traces
     * Params: service, start, limit
     * Return parsed trace data
   - Use httpx.AsyncClient
   - Handle errors gracefully

2. Create `platform/backend/app/clients/loki_client.py`:
   - Class: LokiClient
   - Method: async query_logs(service_name, query_filter="error", limit=100)
     * Query Loki: GET {LOKI_URL}/loki/api/v1/query_range
     * LogQL query: {service_name="X"} |= "filter"
     * Return log lines with timestamps

3. Create `platform/backend/app/clients/prometheus_client.py`:
   - Class: PrometheusClient
   - Method: async query_metric(query, time=None)
     * Query Prometheus: GET {PROMETHEUS_URL}/api/v1/query
     * Example query: "rate(http_requests_total[5m])"
     * Return metric values

4. Create `platform/backend/app/agents/rca_agent.py`:
   - Class: RCAAgent
   - __init__: initialize Anthropic client, create observability clients
   - Method: async investigate(incident_data: dict) -> dict:
     * Step 1: Fetch traces from Jaeger (service, time range)
     * Step 2: Fetch logs from Loki (service, error filter)
     * Step 3: Fetch metrics from Prometheus (if needed)
     * Step 4: Build context string with all data
     * Step 5: Call Claude API with prompt
     * Step 6: Parse response into structured RCA report
   - Method: _build_context(traces, logs, metrics) -> str:
     * Format data for LLM prompt
   - Method: async _generate_rca(context) -> dict:
     * Claude API call
     * Prompt: "You are an expert SRE. Analyze this incident data..."
     * Return: {summary, root_cause, evidence, suggested_fix, confidence}
   - If ANTHROPIC_API_KEY is "dummy", return mock RCA

5. Create `platform/backend/app/routes/investigate.py`:
   - APIRouter with prefix /api
   - POST /incidents/{incident_id}/investigate:
     * Create RCAAgent instance
     * Build incident_data dict (service_name, start_time)
     * Call agent.investigate()
     * Return RCA report JSON

6. Update `platform/backend/app/main.py`:
   - Import and include investigate router
   - app.include_router(investigate.router)

OUTPUT FORMAT:
- Complete file contents for all 6 files
- Detailed comments explaining async patterns
- Claude API prompt engineering tips
- Mock response structure for testing without API key

CONSTRAINTS:
- Must handle missing ANTHROPIC_API_KEY gracefully (mock mode)
- Async/await throughout for performance
- Error handling on all external API calls
- RCA generation should take 5-15 seconds max