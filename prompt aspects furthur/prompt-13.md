## ⏰ HOUR 13: Automated Anomaly Detection

### Prompt for Hour 13
```
I'm building an SRE Copilot platform. This is Hour 13 of 20.

GOAL: Add background agent that monitors metrics and automatically detects anomalies

CONTEXT:
- Hours 11-12 COMPLETED: Vector DB + Knowledge Graph
- Currently: Incidents are manually triggered
- Goal: Auto-detect anomalies in metrics and create incidents
- Use statistical methods (Z-score, moving average)

REQUIREMENTS:

1. Create `platform/backend/app/agents/anomaly_detector.py`:
   - Class: AnomalyDetector
   - __init__:
     * Load PrometheusClient
     * Define baseline window: 1 hour
     * Define alert thresholds
   - Method: async detect_latency_anomaly(service_name):
     * Query Prometheus for P95 latency over last hour
     * Calculate: mean, stddev
     * Current value > mean + (3 * stddev) → anomaly
     * Return: {is_anomaly: bool, current: float, baseline: float, zscore: float}
   - Method: async detect_error_rate_anomaly(service_name):
     * Query error rate metric
     * Use moving average (5min window)
     * Spike > 2x normal → anomaly
   - Method: async detect_all(service_names: list):
     * Check all services for all anomaly types
     * Return list of detected anomalies
   - Method: calculate_zscore(values, current) -> float:
     * Standard Z-score formula
   - Method: calculate_moving_average(values, window=5) -> float:
     * Simple moving average

2. Create `platform/backend/app/services/monitoring_service.py`:
   - Class: MonitoringService
   - Method: async run_monitoring_loop():
     * Infinite loop with 30s sleep
     * Get list of active services from knowledge graph
     * For each service:
       - Run anomaly detection
       - If anomaly detected → create_incident()
       - Store anomaly metadata
   - Method: async create_incident(service, anomaly_data):
     * Create Incident in database
     * severity based on anomaly_data.zscore
     * title: "Anomaly detected: {type}"
     * Trigger RCA agent in background
   - Method: start():
     * Start monitoring loop in background task

3. Update `platform/backend/app/main.py`:
   - Import MonitoringService
   - Add startup event:
     * monitoring_service = MonitoringService()
     * asyncio.create_task(monitoring_service.run_monitoring_loop())
   - Add shutdown event:
     * monitoring_service.stop()

4. Create `platform/backend/app/models.py` updates:
   - Add table: anomalies
     * id, incident_id, anomaly_type (latency/error_rate/custom)
     * detected_at, zscore, threshold
     * baseline_value, current_value
     * metadata (JSON)

5. Create `platform/backend/app/routes/monitoring.py`:
   - APIRouter with prefix /api/monitoring
   - GET /status:
     * Return monitoring service status
     * Last check time, services monitored, anomalies in last hour
   - GET /anomalies:
     * List recent anomalies
     * Filter by service, type, time range
   - POST /baseline/reset/{service}:
     * Reset baseline for a service (after fix)

6. Update `platform/frontend/src/pages/Dashboard.jsx`:
   - Add "Monitoring Status" card
   - Show:
     * ✅ Monitoring: Active
     * 🔍 Services watched: 2
     * 🚨 Anomalies last hour: 0
     * ⏱️ Last check: 15s ago

7. Create `docs/ANOMALY_DETECTION.md`:
   - Explain statistical methods (Z-score, moving average)
   - Why 3-sigma threshold
   - How baselines are calculated
   - False positive handling
   - Tuning sensitivity
   - Comparison to ML-based detection (future)

8. Create test script: `tests/test_anomaly_detection.py`:
   - Generate artificial metric spike
   - Verify anomaly is detected
   - Verify incident is created
   - Verify RCA is triggered

OUTPUT FORMAT:
- Complete code with async patterns
- Statistical formulas with comments
- Background task management
- Error handling (Prometheus down, etc.)

CONSTRAINTS:
- Monitoring loop must not block API
- Must handle service restarts gracefully
- Detection latency < 60 seconds
- No false positives with normal traffic
- Baselines must adapt over time