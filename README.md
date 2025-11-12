# SRE Copilot - Project Context Document
*AI-Powered Incident Prediction and Automated Root Cause Analysis*

## 📋 Project Overview

**Hackathon Duration:** 5 Days  
**Winner:** Karan Rupani (Reference Implementation)  
**Shared By:** Nishant Gauttam  
**Project Type:** AI-Agentic Multi-Agent System

---

## 🎯 Problem Statement

### The Challenge
SRE teams face critical operational challenges:
- **Alert Fatigue:** Hundreds of alerts daily, many false positives
- **Manual RCA:** Time-consuming debugging (2+ hours per incident)
- **Reactive Approach:** Information scattered across tools
- **Context Switching:** Multiple dashboards, logs, services

### Real-World Scenario (Sam's Story)
- **Time:** 11 PM incident detected
- **Issue:** Recent deployment added query filter without database index
- **Resolution Time:** 2 hours of manual investigation
- **Problem:** Juggling dashboards, syncing logs across services
- **Impact:** Critical system performance degradation

---

## 💡 Solution Architecture

### Core Philosophy
Build an **autonomous AI-agentic system** that:
- Prevents incidents before they occur
- Automates root cause analysis
- Learns from historical patterns
- Provides actionable insights
- Beautiful, real-time UI/UX

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Frontend Dashboard                        │
│  (React + TailwindCSS + Real-time WebSocket Updates)       │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│                   Orchestration Layer                        │
│        (FastAPI Backend + Agent Coordinator)                 │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌──────────────┬──────────────┬──────────────┬───────────────┐
│   Data       │   Incident   │     RCA      │  Predictive   │
│  Ingestion   │   Detection  │    Agent     │    Agent      │
│   Agent      │    Agent     │              │               │
└──────────────┴──────────────┴──────────────┴───────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│              Knowledge Base & Vector Store                   │
│     (ChromaDB/Qdrant + Time-series Embeddings)              │
└─────────────────────────────────────────────────────────────┘
                            ↓
┌─────────────────────────────────────────────────────────────┐
│           Data Sources & Monitoring Stack                    │
│  OpenTelemetry Collector → SigNoz → Custom Parsers          │
└─────────────────────────────────────────────────────────────┘
```

---

## 🤖 AI Agent System Design

### 1. Data Ingestion Agent
**Responsibilities:**
- Poll SigNoz API every 5 minutes
- Parse logs, traces, metrics from OpenTelemetry
- Generate embeddings for each data point
- Store in vector database with timestamps
- **Health check every 30 minutes** (key requirement)
- Update system topology map dynamically

**Autonomy Features:**
- Self-triggers on schedule
- Auto-scales parsing based on data volume
- Detects core incidents from parsed data
- Updates embeddings over time for time-series data

**Data Processing:**
- Logs parsing and structuring
- Trace analysis and request counting
- Timeout detection
- Error pattern recognition
- Embedding generation for anomaly detection

### 2. Incident Detection Agent
**Responsibilities:**
- Monitor incoming metrics/logs continuously
- Calculate baseline deviations using embeddings
- Cluster related anomalies
- Suppress false positives intelligently
- Create incident tickets with priority
- Trigger RCA Agent on critical incidents

**Autonomy Features:**
- Continuously running background process
- Self-adjusting thresholds based on learning
- Auto-prioritization based on impact
- Correlation of alerts using AI-based clustering

**Detection Methods:**
- Embedding similarity analysis
- Time-series pattern analysis
- Context enrichment
- Historical baseline comparison

### 3. RCA Agent (Root Cause Analysis)
**Responsibilities:**
- Spin up per-incident autonomous investigation
- **Multi-step reasoning chain:**
  1. Fetch and filter relevant logs (time window analysis)
  2. Query recent deployments and code changes
  3. Analyze metric correlations with anomalies
  4. Check infrastructure changes
  5. Search knowledge graph for similar past incidents
  6. Identify probable failure points
  7. Generate comprehensive RCA report with confidence scores
  8. Suggest fixes (rollback, infra tuning, config update)

**Autonomy Features:**
- Per-incident agent instance
- Parallel investigation threads
- Self-terminating on resolution
- Learning from resolution outcomes

**Integration Points:**
- Git repository analysis
- Infrastructure change logs
- Historical incident database
- Deployment tracking systems

### 4. Predictive Insights Agent
**Responsibilities:**
- Analyze historical patterns and trends
- Detect slow degradations before customer impact
- Predict resource exhaustion scenarios
- Flag drift from baseline behaviors
- Generate proactive alerts
- Continuously learn from incidents and usage patterns

**Autonomy Features:**
- Runs every 15 minutes
- Self-learning from outcomes
- Adapts prediction models over time
- Proactive issue flagging

**Prediction Capabilities:**
- Memory leak trend detection
- Disk space exhaustion forecasting
- Performance degradation patterns
- Capacity planning insights

---

## 🛠️ Technology Stack

### Backend Infrastructure
- **Framework:** FastAPI (async, WebSocket support)
- **AI Orchestration:** LangChain + CrewAI
- **LLM:** Claude 3.5 Sonnet (Anthropic API)
- **Vector Database:** ChromaDB or Qdrant
- **Time-series DB:** TimescaleDB or SigNoz built-in
- **Graph Database:** NetworkX (in-memory) or Neo4j
- **Agent Communication:** Message queue or event bus

### Frontend Stack
- **Framework:** React + Vite
- **Styling:** TailwindCSS + shadcn/ui
- **Charts:** Recharts or D3.js
- **Real-time:** Socket.io-client or WebSocket
- **State Management:** Zustand or Redux Toolkit
- **UI Components:** Custom dashboard components

### Monitoring & Observability
- **Collector:** OpenTelemetry Collector (core component)
- **Backend:** SigNoz (logs, traces, metrics storage)
- **Custom Parser:** Python service wrapping OTel data
- **Instrumentation:** Auto-instrumentation for sample apps

### AI/ML Components
- **Embeddings:** OpenAI Ada-002 or Claude embeddings
- **Anomaly Detection:** Isolation Forest + Custom threshold logic
- **Pattern Matching:** Cosine similarity on embeddings
- **Time-series Analysis:** Statistical models + ML

### DevOps
- **Containerization:** Docker + Docker Compose
- **Sample Apps:** Node.js/Python microservices (intentional bugs)
- **CI/CD Simulation:** GitHub webhooks for deployment tracking
- **Deployment:** Local development, cloud-ready architecture

---

## 📅 5-Day Sprint Plan

### Day 1: Foundation & Data Pipeline 🚀
**Goal:** Set up monitoring infrastructure and basic data ingestion

**Morning (4 hours):**
- [ ] Set up project repository structure
- [ ] Deploy SigNoz using Docker Compose
- [ ] Configure OpenTelemetry Collector
- [ ] Create 2-3 sample microservices with intentional issues

**Afternoon (4 hours):**
- [ ] Build Data Ingestion Agent (AI-powered)
  - Connect to SigNoz API
  - Parse logs, traces, metrics
  - Structure data for embedding generation
  - Set up 30-minute health check scheduler

**Evening (2 hours):**
- [ ] Set up vector database (ChromaDB/Qdrant)
- [ ] Create embedding pipeline for logs/metrics
- [ ] Test end-to-end data flow

**Deliverables:**
- Running SigNoz instance
- OpenTelemetry data flowing
- Basic data ingestion agent operational

---

### Day 2: AI Agent Foundation & Incident Detection 🤖
**Goal:** Build autonomous agent framework and incident detection

**Morning (4 hours):**
- [ ] Agent Framework Setup
  - Implement LangChain/CrewAI multi-agent orchestrator
  - Create base agent class with memory
  - Set up Claude API integration
  - Implement agent communication protocol

**Afternoon (4 hours):**
- [ ] Build Incident Detection Agent
  - Anomaly detection using embeddings similarity
  - Time-series pattern analysis
  - Alert correlation engine
  - False positive suppression logic
  - Auto-trigger on threshold breaches

**Evening (2 hours):**
- [ ] Test incident detection with synthetic issues
- [ ] Fine-tune detection thresholds
- [ ] Validate agent autonomy

**Deliverables:**
- Multi-agent framework operational
- Incident detection with <20% false positives

---

### Day 3: RCA Agent & Knowledge Graph 🔍
**Goal:** Autonomous root cause analysis and learning system

**Morning (5 hours):**
- [ ] Build RCA Agent (Most Critical Component)
  - Log correlation analyzer
  - Recent deployment tracker
  - Metric-anomaly mapper
  - Code change analyzer (Git integration)
  - Infrastructure change detector
  - Multi-step reasoning chain

**Afternoon (4 hours):**
- [ ] Knowledge Graph & Memory System
  - Build incident-to-fix relationship graph
  - Store historical RCA patterns
  - Implement similarity search for past incidents
  - Create learning feedback loop
  - Self-learning capability implementation

**Evening (1 hour):**
- [ ] Integration testing with Day 1-2 components
- [ ] End-to-end incident flow validation

**Deliverables:**
- Functional RCA agent with 80%+ accuracy
- Knowledge graph with historical patterns

---

### Day 4: Predictive Agent & Dashboard 📊
**Goal:** Proactive monitoring and beautiful UI

**Morning (3 hours):**
- [ ] Build Predictive Insights Agent
  - Trend analysis engine
  - Degradation pattern detector
  - Resource exhaustion predictor
  - Baseline drift calculator

**Afternoon (5 hours):**
- [ ] Build Frontend Dashboard (React + Vite)
  - **System Overview Page:**
    - Real-time topology map (all servers)
    - Health status of all services
    - Live metrics feed
  - **Incidents Dashboard:**
    - Active incidents list
    - RCA status tracker
    - Timeline visualization
  - **Analytics & Insights:**
    - Prediction alerts
    - Historical trends
    - Agent learning progress
  - **Agent Activity Feed:**
    - Real-time agent actions
    - Investigation status

**Evening (2 hours):**
- [ ] WebSocket integration for real-time updates
- [ ] Responsive design polish
- [ ] Beautiful UI/UX refinement

**Deliverables:**
- Complete predictive system
- Production-ready dashboard

---

### Day 5: Integration, Polish & Demo 🎯
**Goal:** Complete system integration and demo preparation

**Morning (3 hours):**
- [ ] Build Chat Interface
  - NLP query parser
  - Integration with all agents
  - Conversational context management
  - Sample queries: "Why is latency high?", "What changed before CPU spike?"

**Afternoon (4 hours):**
- [ ] Slack Bot Integration
  - Alert webhook listener
  - Bot command handlers
  - RCA report formatter
  - Interactive actions (trigger RCA, query metrics)

**Late Afternoon (2 hours):**
- [ ] End-to-end testing scenarios
- [ ] Performance optimization
- [ ] Error handling & logging
- [ ] Documentation

**Evening (1 hour):**
- [ ] Demo preparation and rehearsal
- [ ] Create demo video
- [ ] Final polish

**Deliverables:**
- Complete integrated system
- Demo-ready application

---

## 🎨 Dashboard Requirements

### Must-Have Features
1. **Unified Server View:**
   - Complete dashboard showing all servers where agent is installed
   - Real-time health indicators
   - System topology visualization

2. **Post-Agent Installation Data:**
   - OpenTelemetry data integration
   - Logs, traces, requests, timeouts
   - Core incident metrics

3. **Embedding-Based Analysis:**
   - Parse all telemetry data
   - Detect incidents through embedding values
   - Update embeddings over time for time-series data
   - Build vast knowledge base

4. **Automated Health Checks:**
   - Every 30 minutes log parsing
   - Continuous health monitoring
   - Proactive issue detection

5. **Beautiful UI:**
   - Modern, clean design
   - Real-time updates
   - Intuitive navigation
   - Responsive layout

---

## 🎯 Key Features Breakdown

### Data Integration
- Seamless connection with observability tools
- Real-time streaming and historical ingestion
- Support for logs, metrics, traces, events
- OpenTelemetry as primary collector

### System Understanding
- Automatically map system architecture
- Learn service topology and dependencies over time
- Continuously update with deployment changes
- Track scaling events and infrastructure shifts

### Alert Management
- Consolidate alerts and reduce noise
- Correlate related incidents using AI-based clustering
- Context enrichment for each alert
- Embeddings and time-series analysis
- Suppress false positives by understanding baseline behaviors
- Prioritize high-impact incidents with context

### Autonomous RCA Agent (Per Incident)
- Fetches and filters relevant logs
- Maps recent code deployments and infra changes
- Correlates metrics with anomalies
- Identifies probable failure points
- Summarizes RCA with context and historical patterns
- Suggests fixes (rollback, infra tuning, config update)

### Automated Diagnosis
- Ingest error alerts from variety of systems
- Deep fetch for detailed analysis
- Perform incident analysis
- Generate detailed RCA with suggested solutions
- Use historical patterns, log correlations, anomaly detection
- Recommend probable fixes

### Predictive Insights
- Predict before issues escalate
- Continuously learn from incidents
- Analyze usage patterns and drift from baseline
- Proactively flag brewing issues
- Detect slow degradations before customer impact

### Self-Learning Capability
- Learn from past RCA incidents
- Build context from historical data
- Improve accuracy over time
- Adapt to new patterns

### Knowledge Graph & Memory
- Build internal knowledge graph of system behavior
- Track past incidents, fixes, architectural decisions
- Act as dynamic system memory for the team
- Enable pattern recognition across incidents

### Natural Language Interaction
- Chat interface for live queries
- Query live metrics and past incidents
- Perform RCA through conversation
- Example queries:
  - "Why is latency high in service-X?"
  - "What changed in the system before this CPU spike?"
  - "Was this issue seen in last month's deployment?"

---

## 📱 User Interfaces

### Web Interface - Unified View
- Real-time incidents and their status
- System topology & health visualization
- RCA reports and fix history
- Learning insights and upcoming risk areas
- Agent activity feed
- Historical analytics

### Chat Interface / Slack Integration
- Talk to agent directly
- Trigger on alert messages
- Onboarded as Slack bot or CLI tool
- Natural language queries
- Interactive RCA exploration

---

## 🎬 Demo Scenario

### Simulated Critical Alert Flow

**T-0 (Setup):**
- Deploy broken code: DB query without index
- System running normally

**T+2 minutes:**
- Data Ingestion Agent detects high DB latency in metrics
- Embeddings show significant deviation from baseline
- Alert generated internally

**T+4 minutes:**
- Incident Detection Agent creates alert: "DB_LATENCY_SPIKE"
- Dashboard shows red critical indicator
- Slack notification sent: "🔴 High CPU Alert - Service A"
- Incident #INC-1234 created

**T+5 minutes:**
- RCA Agent auto-triggers and starts investigation
- Dashboard shows: "SRE Copilot - Incident detection system has started"
- Multi-step analysis begins:
  1. Fetches DB slow query logs
  2. Correlates with deployment timeline
  3. Identifies recent code change (deployment v1.2.3)
  4. Searches knowledge graph (finds similar past incident)
  5. Analyzes query patterns

**T+7 minutes:**
- RCA Complete - Root Cause Identified:
  - "A recent deployment added a new column without the right index"
  - "Filter query on user_id column missing index"
  - "Causing load on DB and application server"
- Suggested Fixes:
  1. "Add index: CREATE INDEX idx_users_user_id ON users(user_id)"
  2. "Rollback deployment v1.2.3"
- Confidence Score: 92%

**T+10 minutes:**
- Predictive Agent flags additional issue:
  - "API response times degrading by 15%"
  - "Predict outage in 20 minutes if not addressed"
- Proactive alert sent to team

**T+12 minutes:**
- Manual/Auto-remediation option presented
- Fix applied (index created or rollback executed)
- System returns to normal

**T+15 minutes:**
- Knowledge graph updated with new incident pattern
- Learning feedback loop completed
- Future similar incidents can be detected faster

---

## ✅ MVP Features (Must Have)

### Core Agents
- ✅ Data Ingestion with 30-minute health checks
- ✅ Incident Detection with anomaly clustering
- ✅ RCA Agent with multi-step reasoning
- ✅ Basic Predictive Insights

### Dashboard
- ✅ Real-time system topology
- ✅ Incident list with status tracking
- ✅ RCA report viewer
- ✅ Agent activity feed
- ✅ Beautiful, modern UI

### Intelligence
- ✅ Embedding-based pattern matching
- ✅ Historical incident learning
- ✅ Automated fix suggestions
- ✅ Time-series anomaly detection

### Integration
- ✅ OpenTelemetry + SigNoz
- ✅ Slack alerts
- ✅ Basic chat interface
- ✅ Real-time WebSocket updates

---

## 🎁 Bonus Features (If Time Permits)

- Advanced graph visualization (D3.js force graph)
- Auto-remediation execution engine
- Multi-cloud support (AWS, GCP, Azure)
- Custom alert rule builder
- Mobile-responsive design
- Export RCA reports (PDF)
- Integration with PagerDuty, OpsGenie
- Custom metric dashboards
- Team collaboration features

---

## 📊 Success Metrics

### Functional Requirements
- All 4 agents autonomous and communicating
- Agent-to-agent coordination working
- Self-triggering capabilities operational

### Intelligence Requirements
- Detect 3+ incident types correctly
- <20% false positive rate
- 80%+ RCA accuracy

### Learning Requirements
- Show improved RCA over multiple incidents
- Knowledge graph growing with patterns
- Embedding accuracy improving

### UX Requirements
- Dashboard loads <2 seconds
- Real-time updates working smoothly
- Beautiful, intuitive interface

### Demo Requirements
- Complete end-to-end incident flow in 5 minutes
- Showcase all agents working together
- Demonstrate learning capability

---

## 💡 Critical Success Factors

### Technical
1. **Day 1-2 Priority:** Data flow is foundation - everything depends on this
2. **Day 3 Focus:** RCA agent is the star - allocate most complex logic here
3. **Day 4 Balance:** Make UI impressive but functional (use component library)
4. **Day 5 Polish:** Rehearse demo multiple times
5. **Throughout:** Commit working code frequently, use feature branches

### Architectural
- Keep agents truly autonomous (self-triggering)
- Ensure real-time data flow (no polling delays >5 min)
- Build knowledge graph from day 1
- Make embeddings central to detection logic

### AI/ML
- Use Claude API effectively for reasoning
- Implement proper embedding strategies
- Balance autonomy with reliability
- Learn from every incident

### UX/UI
- Prioritize real-time visibility
- Show agent "thinking" process
- Make RCA reports actionable
- Beautiful but functional design

---

## 🚀 Next Steps

Ready to begin implementation. Context preserved and documented.

**Current Status:** Planning Complete ✅  
**Next Phase:** Start Day 1 Development

**Decision Points:**
1. Choose vector database (ChromaDB vs Qdrant)
2. Choose frontend component library (shadcn/ui recommended)
3. Define agent communication protocol
4. Set up development environment

---

## 📝 Notes & Reminders

- **OpenTelemetry Wrapper:** Create custom parser for enhanced data extraction
- **Embedding Strategy:** Update embeddings continuously for time-series learning
- **Health Checks:** 30-minute automated log parsing is mandatory
- **Knowledge Base:** Must be vast and continuously growing
- **UI Priority:** Beautiful design is as important as functionality
- **Agent Autonomy:** Each agent must be truly autonomous, not just scheduled tasks

---

**Document Version:** 1.0  
**Last Updated:** Start of Hackathon  
**Status:** Ready for Implementation 🚀
