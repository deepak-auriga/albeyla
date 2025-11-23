## ⏰ HOUR 12: Knowledge Graph with Neo4j

### Prompt for Hour 12
```
I'm building an SRE Copilot platform. This is Hour 12 of 20.

GOAL: Add Neo4j knowledge graph to map relationships between services, incidents, and fixes

CONTEXT:
- Hour 11 COMPLETED: Vector DB for similarity search
- Now adding GRAPH DATABASE to understand:
  * Which services call which (dependencies)
  * Which deployments caused which incidents
  * Which fixes work for which root causes
- Neo4j visualizations will impress judges

REQUIREMENTS:

1. Update `platform/docker-compose.yml`:
   - Add neo4j service:
     * image: neo4j:5.13-community
     * ports: 7474 (browser), 7687 (bolt)
     * environment:
       - NEO4J_AUTH=neo4j/password123
       - NEO4J_PLUGINS=["apoc"]
     * volumes: neo4j-data:/data
   - Add neo4j-data volume

2. Update `platform/backend/requirements.txt`:
   - Add: neo4j==5.14.0

3. Create `platform/backend/app/clients/neo4j_client.py`:
   - Class: KnowledgeGraphClient
   - __init__:
     * Connect to Neo4j bolt://neo4j:7687
     * Auth: neo4j/password123
   - Method: async create_service(name, version):
     * CREATE (s:Service {name: $name, version: $version})
   - Method: async create_incident(incident_id, service_name, severity):
     * CREATE (i:Incident {id: $id, service: $service})
     * MATCH (s:Service {name: $service})
     * CREATE (i)-[:OCCURRED_IN]->(s)
   - Method: async link_incident_to_deploy(incident_id, deploy_version):
     * MATCH (i:Incident {id: $incident_id})
     * MATCH (d:Deploy {version: $version})
     * CREATE (i)-[:CAUSED_BY]->(d)
   - Method: async create_fix(fix_description, incident_id):
     * CREATE (f:Fix {description: $desc})
     * MATCH (i:Incident {id: $incident_id})
     * CREATE (f)-[:RESOLVES]->(i)
   - Method: async get_service_incidents(service_name) -> list:
     * MATCH (i:Incident)-[:OCCURRED_IN]->(s:Service {name: $name})
     * RETURN i ORDER BY i.timestamp DESC LIMIT 10
   - Method: async get_fix_patterns(root_cause_type) -> list:
     * MATCH (f:Fix)-[:RESOLVES]->(i:Incident {type: $type})
     * RETURN f, count(*) as usage_count
     * ORDER BY usage_count DESC

4. Create `platform/backend/app/services/knowledge_graph_service.py`:
   - Class: KnowledgeGraphService
   - Method: async build_from_traces(traces):
     * Extract service dependencies from trace spans
     * Create Service nodes
     * Create CALLS relationships
   - Method: async record_incident_resolution(incident, rca, fix_applied):
     * Create Incident node
     * Link to Service
     * Link to Deploy (if known)
     * Create Fix node
     * Create relationships
   - Method: async get_insights_for_service(service_name):
     * Query graph for patterns
     * Return: common failures, effective fixes, risky deployments

5. Update `platform/backend/app/agents/rca_agent.py`:
   - Import KnowledgeGraphService
   - In investigate():
     * Query graph for similar incident patterns
     * Add to Claude prompt:
       "Knowledge graph shows:
        - This service has 3 similar incidents (all DB-related)
        - Adding index fixed 2/3 cases
        - Rollback fixed 1/3 cases"
     * After RCA, store in graph

6. Create `platform/backend/app/routes/graph.py`:
   - APIRouter with prefix /api/graph
   - GET /services:
     * Return all services from graph
     * Include incident counts
   - GET /services/{name}/dependencies:
     * Return service dependency map
     * Format for frontend visualization
   - GET /services/{name}/incidents:
     * Return incident history with patterns
   - GET /fix-patterns/{root_cause_type}:
     * Return proven fixes for this type

7. Create `platform/frontend/src/pages/ServiceMap.jsx`:
   - New page showing service topology
   - Use D3.js or React Flow for visualization
   - Nodes: Services (size = incident count)
   - Edges: CALLS relationships
   - Click service → show incidents
   - Color code by health (red = many incidents)

8. Update `platform/frontend/src/App.jsx`:
   - Add route: /service-map → ServiceMap

9. Update `platform/frontend/src/components/Header.jsx`:
   - Add nav link to Service Map

10. Create `docs/KNOWLEDGE_GRAPH.md`:
    - Explain graph database concepts
    - Show example Cypher queries
    - Diagram of node/relationship types
    - How it improves RCA over time

11. Create `platform/backend/app/scripts/init_graph.py`:
    - Script to populate graph with demo data
    - Create sample services, incidents, fixes
    - python -m app.scripts.init_graph

OUTPUT FORMAT:
- Complete code for all components
- Neo4j Cypher queries with comments
- D3.js or React Flow for service map
- Explain graph vs relational vs vector DB

CONSTRAINTS:
- Neo4j must start cleanly
- Graph queries must complete <500ms
- Frontend visualization must be interactive
- Graph should show value in demo (not empty)