## ⏰ HOUR 14: Feedback Loop & Self-Learning

### Prompt for Hour 14
```
I'm building an SRE Copilot platform. This is Hour 14 of 20.

GOAL: Add feedback mechanism so system learns from human corrections

CONTEXT:
- Hour 13 COMPLETED: Automated anomaly detection
- Problem: RCA might be wrong sometimes
- Solution: Let users rate RCA quality, use feedback to improve
- Store feedback, retrain embeddings, adjust confidence

REQUIREMENTS:

1. Update `platform/backend/app/models.py`:
   - Add table: rca_feedback
     * id, rca_report_id, incident_id
     * rating (1-5 stars)
     * was_helpful (boolean)
     * correct_root_cause (text, if AI was wrong)
     * correct_fix (text, if suggestion didn't work)
     * feedback_text (optional comments)
     * submitted_by, submitted_at

2. Create `platform/backend/app/services/learning_service.py`:
   - Class: LearningService
   - Method: async record_feedback(rca_id, feedback_data):
     * Store in rca_feedback table
     * If rating < 3 → low quality, analyze why
     * If correct_root_cause provided → update knowledge graph
     * Trigger retraining if enough feedback collected
   - Method: async calculate_rca_quality_score() -> float:
     * Average rating of all RCAs
     * Used to show "AI Accuracy" metric
   - Method: async retrain_embeddings():
     * Get all incidents with feedback
     * For low-rated RCAs: downweight in similarity search
     * For high-rated RCAs: use as positive examples
     * Re-embed with adjusted weights
   - Method: async adjust_confidence_model():
     * Analyze: when was AI confident but wrong?
     * Update confidence calculation in RCA agent
     * Use logistic regression: features = [similar_incidents_count, zscore, service_history]
   - Method: async get_learning_insights():
     * Return: areas where AI struggles
     * Common failure patterns
     * Improvement over time graph

3. Update `platform/backend/app/agents/rca_agent.py`:
   - In _generate_rca():
     * Query past feedback for similar incidents
     * Add to Claude prompt:
       "Past feedback shows:
        - For DB issues, index suggestion was 90% effective
        - For this service, rollback usually doesn't help
        - Users prefer detailed SQL fixes over generic advice"
     * Adjust confidence based on feedback history
   - Method: calculate_confidence_with_feedback(base_confidence, feedback_data):
     * If similar past RCAs were highly rated → increase confidence
     * If similar past RCAs were wrong → decrease confidence

4. Create `platform/backend/app/routes/feedback.py`:
   - APIRouter with prefix /api/feedback
   - POST /rca/{rca_id}:
     * Submit feedback for an RCA
     * Body: {rating, was_helpful, correct_root_cause?, correct_fix?, feedback_text?}
   - GET /insights:
     * Return learning insights (quality score, improvement trend)
   - POST /retrain:
     * Trigger manual retraining (admin only in production)

5. Update `platform/frontend/src/components/RCAModal.jsx`:
   - Add feedback section at bottom
   - Star rating (1-5)
   - "Was this helpful?" Yes/No buttons
   - If No → show form:
     * "What was the actual root cause?"
     * "What fix actually worked?"
     * Text area for additional comments
   - "Submit Feedback" button
   - After submit: Thank you message + "This helps improve AI"

6. Create `platform/frontend/src/pages/Insights.jsx`:
   - New page showing learning metrics
   - Cards:
     * AI Accuracy Score (average rating)
     * Total Incidents Analyzed
     * Feedback Received (%)
     * Improvement Over Time (line chart)
   - Section: "Where AI Excels"
     * Service types with high accuracy
   - Section: "Where AI Struggles"
     * Service types with low accuracy
     * Suggested improvements
   - Section: "Recent Learnings"
     * Timeline of feedback-driven improvements

7. Update `platform/frontend/src/App.jsx`:
   - Add route: /insights → Insights

8. Create `platform/backend/app/scripts/analyze_feedback.py`:
   - Script for admin to analyze feedback
   - Generate report:
     * RCA accuracy by service
     * Common reasons for low ratings
     * Suggested prompt improvements
   - python -m app.scripts.analyze_feedback > report.txt

9. Create `docs/SELF_LEARNING.md`:
   - Explain feedback loop architecture
   - How feedback improves future RCAs
   - Diagram: Feedback → Learning → Better RCA
   - Metrics tracked
   - Privacy considerations (feedback is stored)
   - Explain "AI gets smarter over time" pitch

10. Add to `platform/backend/app/main.py`:
    - Background task: daily retrain
    - Schedule: 2 AM UTC
    - Only if >10 new feedback entries

OUTPUT FORMAT:
- Complete code with async patterns
- Feedback form with validation
- Learning metrics calculations
- Visualization for insights page

CONSTRAINTS:
- Feedback must be optional (don't block demo)
- Retraining must not impact live RCA generation
- Privacy: no PII in feedback
- Insights page must show value (even with little data)