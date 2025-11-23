## ⏰ HOURS 6-7: React Frontend

### Prompt for Hours 6-7
```
I'm building an SRE Copilot platform. This is Hours 6-7 of 10.

GOAL: Build beautiful React dashboard with teal/orange theme

CONTEXT:
- Hours 1-5 COMPLETED: Full backend working with incidents API
- Now building frontend that judges will see
- Design MUST impress: off-white bg, teal (#00BFA6) + orange (#FF7A59)
- 2xl rounded corners, hover effects, professional polish

REQUIREMENTS:

1. Create `platform/frontend/package.json`:
   - name: sre-copilot-frontend
   - dependencies: react, react-dom, react-router-dom
   - devDependencies: @vitejs/plugin-react, vite
   - scripts: dev, build, preview

2. Create `platform/frontend/vite.config.js`:
   - Import react plugin
   - Server: host 0.0.0.0, port 3000

3. Create `platform/frontend/index.html`:
   - Basic HTML5 template
   - div#root
   - Script: /src/main.jsx

4. Create `platform/frontend/src/styles/theme.css`:
   - CSS variables:
     * --bg: #F7F6F3 (off-white)
     * --teal: #00BFA6
     * --orange: #FF7A59
     * --dark: #2C3E50
     * --gray: #7F8C8D
     * --card-radius: 16px
   - Global styles: body, buttons, cards
   - .btn, .btn-teal, .btn-orange with hover effects
   - Hover: transform scale(1.02), box-shadow increase
   - .card with border-radius, shadow

5. Create `platform/frontend/src/main.jsx`:
   - React.StrictMode
   - Render App component

6. Create `platform/frontend/src/App.jsx`:
   - BrowserRouter with Routes
   - Route / → Dashboard
   - Route /incidents → Incidents (future)
   - Import Header component
   - Import theme.css

7. Create `platform/frontend/src/components/Header.jsx`:
   - Header bar with logo "🤖 SRE Copilot"
   - Nav links: Dashboard, Incidents, Insights
   - Search box on right
   - User avatar circle with initials "SA"
   - Styled with white bg, shadow

8. Create `platform/frontend/src/pages/Dashboard.jsx`:
   - Main dashboard component
   - State: incidents (useState), selectedIncident, rcaReport, loading
   - useEffect: fetchIncidents() on mount
   - fetchIncidents: fetch http://localhost:8000/api/incidents
   - runRCA(incidentId): POST to /incidents/{id}/investigate
   - UI Structure:
     * Page title "🤖 SRE Copilot Dashboard"
     * System Health card with 3 metrics (Error Rate, P95 Latency, CPU)
     * "🔥 Trigger Bad Deploy" button (calls POST /demo/trigger-incident)
     * Active Incidents section
     * Map incidents to IncidentCard components
     * If no incidents: empty state message
   - Pass onRunRCA handler to IncidentCard

9. Create `platform/frontend/src/components/IncidentCard.jsx`:
   - Props: incident, onRunRCA, loading
   - Display:
     * Severity badge (colored by level)
     * Incident ID
     * Title (large, bold)
     * Service name
     * Detected timestamp
     * Two buttons: "🔍 Run RCA" (teal), "📊 Open" (orange)
   - Card has left border colored by severity
   - Hover effect on card

10. Create `platform/frontend/src/components/RCAModal.jsx`:
    - Props: report, onClose
    - Full-screen overlay with backdrop
    - Modal card (max-width 800px, centered)
    - Sections:
      * Header with title and close button
      * Confidence score badge (large, teal)
      * Summary paragraph
      * Root Cause (yellow highlight box)
      * Evidence (gray box with trace IDs, logs, metrics)
      * Suggested Fix (code block, dark theme)
      * Action buttons: "✅ Apply Fix", "🔄 Rollback"
    - Click backdrop or X to close

11. Create `platform/frontend/Dockerfile`:
    - FROM node:18-alpine
    - WORKDIR /app
    - COPY package*.json, npm install
    - COPY . .
    - EXPOSE 3000
    - CMD npm run dev

12. Update `platform/docker-compose.yml`:
    - Add frontend service:
      * build: ./frontend
      * ports: 3000:3000
      * environment: VITE_API_URL=http://localhost:8000
      * volumes: ./frontend/src:/app/src (hot reload)
      * depends_on: backend

OUTPUT FORMAT:
- Complete file contents for all 12 items
- Inline CSS or CSS-in-JS for styling
- Accessible design (ARIA labels, semantic HTML)
- Mobile-responsive (bonus if time)

CONSTRAINTS:
- Colors MUST be exact: #F7F6F3, #00BFA6, #FF7A59
- Buttons MUST have hover effects
- Cards MUST have 16px border radius
- Must work with existing backend APIs
- Loading states must show during RCA