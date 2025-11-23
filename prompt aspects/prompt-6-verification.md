cd platform
docker compose up -d --build frontend

# Open browser
open http://localhost:3000

# Should see:
# - Beautiful off-white dashboard
# - Teal and orange buttons
# - Empty incidents list (or create one first)

# Test flow:
# 1. Click "Trigger Bad Deploy"
# 2. Incident appears
# 3. Click "Run RCA"
# 4. Modal shows with report
```
