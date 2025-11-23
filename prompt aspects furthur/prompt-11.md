I'm building an SRE Copilot platform. This is Hour 11 of 20.

GOAL: Add vector database (Qdrant) for semantic similarity search of past incidents

CONTEXT:
- Hours 1-10 COMPLETED: Full working MVP with beautiful UI
- Now adding INTELLIGENCE: Find similar past incidents using embeddings
- Use Qdrant (lightweight, Docker-friendly vector DB)
- Embed RCA reports and search by similarity

REQUIREMENTS:

1. Update `platform/docker-compose.yml`:
   - Add qdrant service:
     * image: qdrant/qdrant:latest
     * ports: 6333 (HTTP API), 6334 (gRPC)
     * volumes: qdrant-data:/qdrant/storage
     * network: platform-network
   - Add qdrant-data volume

2. Update `platform/backend/requirements.txt`:
   - Add: qdrant-client==1.7.0
   - Add: sentence-transformers==2.2.2
   - Add: openai==1.3.0 (for embeddings alternative)

3. Create `platform/backend/app/clients/qdrant_client.py`:
   - Class: QdrantManager
   - __init__:
     * Connect to Qdrant at localhost:6333
     * Initialize sentence transformer model: "all-MiniLM-L6-v2"
     * Create collection "incidents" if not exists
       - vector_size: 384 (model dimension)
       - distance: Cosine
   - Method: async embed_text(text: str) -> list[float]:
     * Use sentence transformer to generate embedding
     * Return 384-dim vector
   - Method: async store_incident(incident_id, summary, root_cause, embedding):
     * Store in Qdrant with payload: {incident_id, summary, root_cause, timestamp}
     * Point ID: hash of incident_id
   - Method: async find_similar(query_text: str, limit=5) -> list:
     * Generate embedding for query
     * Search Qdrant collection
     * Return similar incidents with scores

4. Create `platform/backend/app/services/embedding_service.py`:
   - Class: EmbeddingService
   - Method: async embed_incident(incident: Incident, rca: RCAReport):
     * Combine: title + summary + root_cause
     * Generate embedding
     * Store in Qdrant
   - Method: async find_similar_incidents(current_incident) -> list:
     * Query Qdrant with current incident text
     * Filter by score > 0.7 (high similarity)
     * Return list of {incident_id, similarity_score, summary}

5. Update `platform/backend/app/agents/rca_agent.py`:
   - Import EmbeddingService
   - In investigate():
     * After generating RCA, call embedding_service.embed_incident()
     * Before generating RCA, call embedding_service.find_similar_incidents()
     * Add similar incidents to Claude prompt context:
       "Similar past incidents:
        - INC-123 (87% similar): Fixed by adding index
        - INC-456 (82% similar): Rollback resolved it"
   - Improve RCA confidence based on similar incidents

6. Update `platform/backend/app/routes/incidents.py`:
   - Add GET /incidents/{id}/similar:
     * Find similar incidents using Qdrant
     * Return list with similarity scores

7. Update `platform/frontend/src/components/RCAModal.jsx`:
   - Add "Similar Past Incidents" section
   - Show list of similar incidents with:
     * Incident ID (clickable)
     * Similarity score (%)
     * Summary
     * "What worked" (the fix from that incident)
   - Style as cards within the modal

8. Create `platform/backend/app/scripts/backfill_embeddings.py`:
   - Script to embed all existing incidents
   - Useful for seeding demo data
   - python -m app.scripts.backfill_embeddings

9. Create `docs/VECTOR_DB.md`:
   - Explain vector embeddings for non-technical audience
   - How similarity search works
   - Why this improves RCA quality
   - Diagram showing embedding pipeline

OUTPUT FORMAT:
- Complete code for all files
- Explain sentence transformers vs OpenAI embeddings
- Include initialization script for Qdrant
- Show example queries

CONSTRAINTS:
- Qdrant must start with docker compose
- Embeddings must be generated within 2 seconds
- Similar incidents should have >70% similarity
- Handle empty collection gracefully (new system)