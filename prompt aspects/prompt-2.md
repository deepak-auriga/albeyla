
## ⏰ HOUR 2: Customer Environment & OTEL Collector

### Prompt for Hour 2
```
I'm building an SRE Copilot platform. This is Hour 2 of 10.

GOAL: Create customer-side infrastructure with demo apps and OTEL Collector

CONTEXT:
- Hour 1 COMPLETED: Platform stack is running (Jaeger, Loki, Prometheus at localhost)
- Now building the CUSTOMER side that pushes data to platform
- Customer environment is SEPARATE from platform (simulates remote deployment)
- Using OpenTelemetry for instrumentation

REQUIREMENTS:

1. Create `customer-environment/docker-compose.yml` with:
   - otel-collector (otel/opentelemetry-collector-contrib) - ports 4317, 4318
   - blog-app (Python Flask) - port 8001
   - todo-app (Python Flask) - port 8002
   - Network: customer-network
   - Use host.docker.internal to reach platform services

2. Create `customer-environment/otel-collector-config.yaml`:
   - Receivers: OTLP gRPC and HTTP on 0.0.0.0
   - Processors: batch (10s timeout), attributes (inject tenant.id from env var)
   - Exporters:
     * otlp/traces → host.docker.internal:4318
     * loki → host.docker.internal:3100/loki/api/v1/push
     * prometheusremotewrite → host.docker.internal:9090/api/v1/write
   - Add logging exporter for debugging
   - Environment variables: TENANT_ID, TENANT_API_KEY, PLATFORM_OTLP_ENDPOINT

3. Create `customer-environment/demo-app/blog-app.py`:
   - Python Flask application
   - OpenTelemetry instrumentation (auto + manual)
   - Endpoints:
     * GET /health - returns {"status": "healthy"}
     * GET /api/posts - fast endpoint (50ms)
     * GET /api/posts?filter=active - SLOW endpoint (2.5s) for demo
   - Manual span for database query
   - Log with trace_id correlation
   - Resource attributes: service.name, tenant.id

4. Create `customer-environment/demo-app/todo-app.py`:
   - Similar to blog-app but port 8002
   - service.name: todo-service
   - Endpoint: GET /api/todos

5. Create `customer-environment/demo-app/requirements.txt`:
   - flask, opentelemetry packages

6. Create `customer-environment/demo-app/Dockerfile`:
   - Python 3.11-slim base
   - Install requirements
   - Copy app files

7. Create `customer-environment/README.md`:
   - How to start customer environment
   - How to generate traffic
   - How to verify data in platform

OUTPUT FORMAT:
- Complete file contents with inline comments
- Explain OTEL SDK setup
- Include curl commands to test

CONSTRAINTS:
- Must work with Hour 1 platform running
- Traces must appear in Jaeger within 30 seconds
- Apps must be instrumented, not just logging