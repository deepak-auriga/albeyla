# Manual trigger
cd customer-environment/demo-app
python trigger_incident.py

# Check Jaeger for traces
open http://localhost:16686
# Search service: blog-service, look for slow traces

# UI trigger
open http://localhost:3000
# Click "Trigger Bad Deploy" button
# Should create incident immediately
```
