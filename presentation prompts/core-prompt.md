# Build production images locally
cd platform
docker build -f backend/Dockerfile.prod -t sre-copilot-backend:prod ./backend
docker build -f frontend/Dockerfile.prod -t sre-copilot-frontend:prod ./frontend

# Test production compose
docker compose -f docker-compose.prod.yml up -d

# Deploy to Fly.io
flyctl auth login
flyctl launch --name sre-copilot
flyctl secrets set ANTHROPIC_API_KEY="your-key"
flyctl deploy

# Check deployment
flyctl status
flyctl logs

# Test production URL
curl https://sre-copilot.fly.dev/health
open https://sre-copilot.fly.dev

# Run health check
./scripts/health_check.sh
# Should exit 0 if healthy
```

---

## ⏰ HOUR 20: Final Polish & Pitch Rehearsal

### Prompt for Hour 20
```
I'm building an SRE Copilot platform. This is Hour 20 of 20 - FINAL HOUR.

GOAL: Perfect the pitch, create backup plans, final testing

CONTEXT:
- Hours 1-19 COMPLETED: Production-ready platform deployed
- This is it - make sure demo is flawless
- Prepare for every possible scenario

REQUIREMENTS:

1. Create `docs/PITCH_FINAL.md`:
   - Complete pitch script with timings
   - Every word you'll say
   - Transitions between slides and demo
   - Backup lines if something breaks
   - Q&A preparation:
     * Expected questions with answers
     * Technical deep-dive answers
     * Business model defense
     * Competitive landscape
   
   - Opening (30 seconds):
     * "Imagine it's 11 PM on Friday..."
     * Hook with Sam's story
     * "I built something to fix this"
   
   - Problem (60 seconds):
     * Statistics on SRE burnout
     * Cost of downtime ($5,600/minute for e-commerce)
     * "Current tools don't help, they just show data"
   
   - Solution Demo (180 seconds):
     * "Let me show you SRE Copilot"
     * Live demo with narration
     * Hit all key features
     * End with impact metrics
   
   - Market & Business (45 seconds):
     * TAM: Every company with microservices
     * Business model: $50/service/month
     * Traction: 3 beta customers
   
   - Closing (15 seconds):
     * "AI is transforming SRE work"
     * "We're building the copilot every team needs"
     * "Thank you - questions?"

2. Create `tests/final_integration_test.sh`:
   - Comprehensive pre-demo test
   - Steps (all must pass):
     1. Platform health check
     2. Database has demo data
     3. All services responding
     4. Frontend loads in <2s
     5. Can create incident
     6. Can run RCA
     7. RCA completes in <15s
     8. Results display correctly
     9. Slack notification sent (if configured)
     10. No console errors
   - Output: Detailed pass/fail report
   - Exit 0 only if ALL tests pass
   - Takes <90 seconds to run

3. Create `DEMO_CHECKLIST.txt`:
   - Physical checklist to print

   30 MINUTES BEFORE:
[ ] Laptop charged (100%)
[ ] Backup laptop ready
[ ] Internet connection tested
[ ] Hotspot ready (backup)
[ ] Demo environment reset
[ ] Run final_integration_test.sh
[ ] All browser tabs open
[ ] Presentation slides loaded
[ ] Water bottle filled
[ ] Deep breath
10 MINUTES BEFORE:
[ ] Close all other apps
[ ] Disable notifications
[ ] Turn off Slack/email
[ ] Set phone to airplane mode
[ ] Final test: trigger one incident
[ ] Verify RCA works
[ ] Clear browser cache
[ ] Zoom to 110% (readability)
DURING DEMO:
[ ] Speak slowly and clearly
[ ] Show enthusiasm
[ ] Make eye contact with judges
[ ] If error occurs → go to backup plan
[ ] Time yourself (5 minutes max)
BACKUP PLANS:
[ ] Screenshots ready (assets/demo-screenshots/)
[ ] Video recording of demo (backup.mp4)
[ ] Offline mode works
[ ] Can explain without showing if needed


4. Create demo video: `assets/demo-backup.mp4`:
   - Record perfect demo run
   - Include narration
   - 4 minutes long
   - Use if live demo fails
   - Upload to YouTube (unlisted)
   - Have URL ready

5. Create `docs/Q&A_PREPARATION.md`:
   - Anticipated questions with answers:
   
   **Technical Questions:**
   Q: "How does the AI generate RCA?"
   A: "We use Claude API with a carefully crafted prompt. We pass in correlated traces, logs, metrics, plus context from similar past incidents. The prompt asks Claude to identify root cause, provide evidence, and suggest fixes. We then parse the structured response."
   
   Q: "What if the AI is wrong?"
   A: "Great question. We have a feedback loop - users can rate RCA quality and provide corrections. This feedback improves future RCAs through our self-learning system. Currently running at 87% accuracy."
   
   Q: "How do you handle multi-tenancy?"
   A: "Each tenant has a unique ID attached to all their telemetry. Data is logically separated in our databases, and we use row-level security. In production, we'd have separate database instances for enterprise customers."
   
   Q: "What about security?"
   A: "All data encrypted in transit (TLS) and at rest. API key authentication. Rate limiting to prevent abuse. We don't store sensitive data - just metadata. Planning SOC2 compliance for enterprise."
   
 Q: "How does it scale?"
   A: "Current architecture handles 1000 spans/second per tenant. For scale, we'd use Kafka for ingestion buffering, Cassandra for Jaeger backend, and Kubernetes for auto-scaling. Each component scales independently. We've load-tested to 10K concurrent users."
   
   **Business Questions:**
   Q: "What's your competitive advantage vs Datadog?"
   A: "Datadog shows dashboards - you still manually correlate. We autonomously investigate. Think GitHub Copilot for operations. Also, we're 1/10th the cost for the RCA feature specifically."
   
   Q: "How do you make money?"
   A: "$50 per service per month. Average customer has 20 services = $1000/month. Enterprise tier at $5000/month with SSO, compliance, dedicated support."
   
   Q: "What's the market size?"
   A: "Every company with microservices needs this. 100K companies worldwide with 20+ services. That's 2M services × $50 = $100M TAM. Growing 40% YoY as more companies adopt microservices."
   
   Q: "Why would someone switch from their current tools?"
   A: "They don't switch - we complement. Keep Datadog for metrics, add us for intelligent investigation. Our value prop is time savings: 2 hours → 10 minutes per incident. ROI positive after first incident."
   
   Q: "What's your go-to-market strategy?"
   A: "Bottom-up: Free tier for individual developers. Viral through developer communities (DevOps Reddit, HackerNews). Once engineers love it, they bring us to their teams. Enterprise sales later."
   
   **Product Questions:**
   Q: "What features are planned?"
   A: "Next quarter: Auto-remediation - AI applies fixes automatically. Then: Slack/PagerDuty native integration. Then: Custom playbooks - teach AI your company's procedures. Long-term: Open source the agent framework."
   
   Q: "How accurate is the predictive alerting?"
   A: "Currently 78% precision on incident prediction. We optimize for low false positives - better to miss an incident than cry wolf. Accuracy improves as the system learns each customer's baseline."
   
   Q: "Can it work with our existing tools?"
   A: "Yes - we're OpenTelemetry native. Works with any tool that supports OTLP: Datadog, New Relic, Elastic. We also have direct integrations for Jaeger, Prometheus, Loki."

6. Create `scripts/demo_offline_mode.sh`:
   - Set up demo to work without internet
   - Mock external API calls
   - Use cached LLM responses
   - Pre-load all assets
   - Usage: ./scripts/demo_offline_mode.sh enable
   - For venues with bad WiFi

7. Update `README.md` with executive summary:
   - Add at top:
```markdown
Executive SummaryProblem: SREs spend 2+ hours manually correlating logs, traces, and metrics during incidents.Solution: AI copilot that automatically investigates incidents in 10 seconds.Impact: 92% reduction in MTTR. 8 incidents prevented this month via predictive alerts.Technology: OpenTelemetry + Vector DB + Knowledge Graph + Claude AIBusiness: $50/service/month. $100M TAM. 3 beta customers.Demo: https://sre-copilot.fly.dev (login: demo@example.com / password: demo123)Hackathon Build: 20 hours. 8,000 lines of code. 40+ files. Production-ready.

8. Create `assets/one-pager.pdf`:
   - Single-page overview for judges
   - Include:
     * Product screenshot
     * Key metrics (MTTR reduction, accuracy)
     * Architecture diagram
     * Business model
     * Team info
     * Contact details
   - Print 10 copies to hand out

9. Create `docs/TECHNICAL_DEEP_DIVE.md`:
   - For technical judges who want details
   - Sections:
     * Architecture (detailed diagrams)
     * Technology choices (with rationale)
     * Data flow (step-by-step)
     * ML models (algorithms used)
     * Performance benchmarks
     * Security model
     * Scalability analysis
     * Code quality metrics
   - 10-15 pages
   - Include code snippets

10. Create `JUDGES_README.md`:
    - Specifically for judges to evaluate
    - Quick start: One command to run
    - Evaluation criteria with evidence:
      * Innovation: [explain unique approach]
      * Technical complexity: [list technologies]
      * Completeness: [list features]
      * UI/UX quality: [screenshots]
      * Business viability: [market analysis]
      * Demo quality: [video link]
    - How to test each feature
    - Expected outcomes
    - Contact for questions

11. Create `scripts/generate_pitch_slides.py`:
    - Auto-generate slides from template
    - Pull live data from system:
      * Current metrics
      * Recent incidents
      * Learning progress
    - Export as PDF
    - Usage: python scripts/generate_pitch_slides.py

12. Create practice recording: `PITCH_PRACTICE.md`:
    - Record yourself 10 times
    - Track:
      * Time (target: 4:30-5:00)
      * Clarity (1-10)
      * Energy (1-10)
      * Smooth transitions (Y/N)
      * Technical issues (count)
    - Improve each iteration
    - Final recording for review

13. Create `docs/WHAT_I_LEARNED.md`:
    - Reflection document
    - Sections:
      * Technical challenges overcome
      * Tools/frameworks learned
      * Design decisions (good and bad)
      * What I'd do differently
      * Proudest achievement
      * Next steps
    - Useful for post-hackathon writeup

14. Create emergency contacts file:
    - `EMERGENCY_CONTACTS.txt`:


15. Final polish tasks:
    - Spell-check all documentation
    - Test on fresh machine (friend's laptop)
    - Verify all links work
    - Check typos in UI
    - Ensure consistent branding
    - Add favicon to frontend
    - Test in multiple browsers
    - Mobile responsive final check
    - Screenshot dark mode (if supported)
    - Update version numbers

OUTPUT FORMAT:
- Complete pitch script (word-for-word)
- Comprehensive Q&A document
- Test scripts with detailed output
- Backup plans for every failure mode
- Judge evaluation materials

CONSTRAINTS:
- Pitch must be exactly 5 minutes
- Must be memorable (judges see 50+ pitches)
- Must be clear (non-technical judges)
- Must show value immediately
- Must handle technical issues gracefully