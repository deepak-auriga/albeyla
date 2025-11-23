## ⏰ HOUR 9: Integration Testing

### Prompt for Hour 9
```
I'm building an SRE Copilot platform. This is Hour 9 of 10.

GOAL: Create automated tests to verify everything works end-to-end

CONTEXT:
- Hours 1-8 COMPLETED: Full application with demo capability
- Need tests for hackathon judges to verify
- Tests should be simple bash scripts

REQUIREMENTS:

1. Create `tests/verify_platform.sh`:
   - Bash script with set -e (exit on error)
   - Function: test_endpoint(name, url, expected_code)
     * curl with -s -o /dev/null -w "%{http_code}"
     * Print colored output (green ✓ or red ✗)
   - Sleep 30s for services to start
   - Test each endpoint:
     * Jaeger UI (localhost:16686) → 200
     * Prometheus (localhost:9090) → 200
     * Loki (localhost:3100/ready) → 200
     * Grafana (localhost:3001) → 302 (redirect)
     * Backend health (localhost:8000/health) → 200
     * Frontend (localhost:3000) → 200
   - Print summary with access URLs
   - Exit 0 if all pass, 1 if any fail

2. Create `tests/verify_integration.sh`:
   - Bash script for full flow test
   - Step 1: Create incident via API
     * curl POST localhost:8000/api/incidents
     * Parse incident_id from JSON (use grep/cut)
   - Step 2: Run RCA
     * curl POST localhost:8000/api/incidents/{id}/investigate
     * Check response contains "root_cause"
   - Step 3: Verify incident in list
     * curl GET localhost:8000/api/incidents
     * grep for incident_id
   - Print step-by-step progress
   - Use colors for readability

3. Create `tests/README.md`:
   - How to run tests
   - What each test verifies
   - Manual testing checklist:
     * Platform services
     * Customer environment
     * End-to-end flow
     * UI/UX checks
   - Debugging section with common issues

4. Make scripts executable:
   - chmod +x tests/*.sh

OUTPUT FORMAT:
- Complete bash scripts with comments
- Use ANSI color codes for output
- Handle errors gracefully
- Include usage examples

CONSTRAINTS:
- Scripts must work on macOS and Linux
- Must not require additional dependencies (just curl, grep)
- Should complete in under 2 minutes
- Clear error messages if something fails