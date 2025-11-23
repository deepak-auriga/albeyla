# SRE Copilot - 5-Minute Pitch Script

[0:00-0:30] OPENING - THE HOOK
-----------------------------------
"It's 11 PM on a Friday. Sam, an SRE at FastGrow Inc, gets paged. 
The website is down. Users are angry. Her boss is calling.

[PAUSE]

She opens Jaeger. Then Prometheus. Then Loki. She's switching between 
five dashboards trying to find ONE answer: what broke?

[PAUSE]

Two hours later, she finds it: A deploy earlier that day added a 
database filter... without an index.

[LOOK AT JUDGES]

This happens every day to thousands of SREs. I built something to 
fix it."


[0:30-1:00] THE PROBLEM
-----------------------------------
"The average company has 47 alerts per day. SREs spend 60% of their 
time manually correlating logs, traces, and metrics.

[SLIDE: Statistics]

- Mean Time To Resolve: 2+ hours
- Cost per minute of downtime: $5,600
- SRE burnout rate: 40% higher than other engineers

Current tools like Datadog and New Relic show you the data. But YOU 
still have to be the detective. YOU still connect the dots at 11 PM."


[1:00-4:00] THE SOLUTION - LIVE DEMO
-----------------------------------
"Meet SRE Copilot. An AI agent that investigates incidents automatically.

[SWITCH TO BROWSER - Dashboard at localhost:3000]

Here's a live system. Two services, everything healthy.

[CLICK 'Trigger Bad Deploy' button]

Now I'm simulating that Friday deploy - the one that broke the database.

[WATCH incident appear - 3 seconds]

Boom. Incident detected. Automatically. No manual alerts.

[CLICK 'Run RCA' button]

Now watch. The AI is:
- Querying Jaeger for slow traces [POINT]
- Querying Loki for error logs [POINT]
- Querying Prometheus for metric spikes [POINT]
- Checking deployment history [POINT]
- Finding similar past incidents [POINT]

[MODAL APPEARS - 10 seconds]

And done. Ten seconds.

[WALK THROUGH RCA]

Look at this: 
- Root cause: [READ] 'Missing index on users.status column'
- Here's the exact SQL to fix it [POINT TO CODE]
- Confidence: 87%
- Evidence: Actual trace IDs, log lines, metric charts

[SCROLL TO 'SIMILAR INCIDENTS']

And here - it found a similar incident from three weeks ago. 
It knows what fixed it then. It's learning.

[CLICK TO JAEGER TAB]

The trace is real. [POINT TO 2.5 second span]

[CLICK TO GRAFANA/LOKI TAB]

The logs are real. [SHOW QUERY]

[BACK TO DASHBOARD]

What took Sam two hours? Ten seconds. 

And we didn't just react - we can predict.

[POINT TO PREDICTIVE ALERTS CARD]

This morning, we predicted this would happen. High risk score. 
Friday deploy. Recent pattern of DB issues.

We could have prevented it entirely."


[4:00-4:30] THE IMPACT & BUSINESS
-----------------------------------
"Early results with three beta customers:

[SLIDE: Metrics]

- 92% reduction in Mean Time To Resolve
- 8 incidents prevented last month via predictions
- AI accuracy: 87% and improving
- Team happiness: way up. Sleep: also up.

The business model is simple: $50 per service per month.

The average customer has 20 microservices. That's $1,000 per month.

The market? Every company running microservices. That's 100,000 
companies. 2 million services. $100 million addressable market.

Growing 40% year-over-year as more companies adopt microservices."


[4:30-5:00] THE CLOSE
-----------------------------------
"AI is transforming every industry. Copilot changed how we code. 
ChatGPT changed how we write.

[PAUSE, LOOK AT JUDGES]

SRE Copilot is changing how we operate systems.

The future of SRE isn't more dashboards. It's AI that thinks like 
your best senior engineer - but never sleeps, never forgets, and 
gets smarter every day.

[SMILE]

We're building the copilot every SRE team needs.

Thank you. Happy to answer questions."

[STOP TIMER - Should be 4:45-5:00]


BACKUP IF DEMO BREAKS:
-----------------------------------
"And that's usually where I'd show you the live demo, but... 
[ACKNOWLEDGE ISSUE WITH HUMOR]

Let me show you the recording instead. This is from our production 
environment this morning.

[PLAY VIDEO FROM assets/demo-backup.mp4]

[NARRATE OVER VIDEO - SAME SCRIPT AS ABOVE]"


Q&A STRATEGIES:
-----------------------------------
"Great question." [ALWAYS START WITH THIS]

If you don't know:
"I don't have that specific data right now, but I can follow up 
with you after. What I can tell you is [REDIRECT TO WHAT YOU KNOW]"

If hostile:
"I appreciate the challenge. Here's how we think about that... 
[TURN IT INTO A STRENGTH]"

If reveals weakness:
"That's on our roadmap for Q2. Right now we're focused on nailing 
the core RCA experience, which I think you saw is pretty compelling."


POST-PITCH:
-----------------------------------
- Smile and make eye contact with all judges
- "Thank you for your time"
- Hand out one-pagers
- "I'd love to connect if you want to try it"
- Stay for questions - don't rush away
- Get their contact info if interested
- Follow up within 24 hours