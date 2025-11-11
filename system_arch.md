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