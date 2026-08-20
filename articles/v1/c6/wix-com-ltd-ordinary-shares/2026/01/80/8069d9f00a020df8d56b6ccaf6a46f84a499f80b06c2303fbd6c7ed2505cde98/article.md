---
schema_version: "1.0.0"
document_id: "8069d9f00a020df8d56b6ccaf6a46f84a499f80b06c2303fbd6c7ed2505cde98"
company_key: "wix-com-ltd-ordinary-shares"
company: "Wix.com Ltd."
source_id: "wix-com-ltd-ordinary-shares-rss-e1d3c855eeb0"
canonical_url: "https://www.wix.engineering/post/when-ai-becomes-your-on-call-teammate-inside-wix-s-airbot-that-saves-675-engineering-hours-a-month"
published_at: "2026-01-15T07:30:31+00:00"
first_seen_at: "2026-07-20T23:18:01.039713+00:00"
fetched_at: "2026-07-28T21:58:17.349618+00:00"
content_hash: "sha256:17ffc1168a1096a77b75d7648d261afec142c636f3522f2a1747a8b8ff1bf941"
---

# When AI Becomes Your On-Call Teammate: Inside Wix's AirBot That Saves 675 Engineering Hours a Month

##


**1. Introduction: The Challenge**


Understanding the engineering need for AirBot starts with recognizing the scale of the ecosystem it operates within. Wix operates a massive cloud-based development platform serving


**250 million** users. This generates over


**4 billion** HTTP transactions daily, feeding a data lake that currently holds


**7 petabytes** of data.


To manage this volume, Wix Data Engineering maintains over


**3,500 Apache Airflow pipelines (DAGs)** . These pipelines handle everything from ETL processes to Machine Learning operations. At this magnitude, even a 99.9% reliability rate guarantees daily failures.


Previously, handling these failures required a reactive, manual workflow. Engineers acted as "human error parsers," jumping between Airflow, Spark, and Kubernetes logs to locate root causes. This created high cognitive load and increased the Mean Time to Understand (MTTU).


##


**2. The Problem: Why Traditional Alerting Struggled at Scale**


Standard automation breaks down in our heterogeneous stack. Airflow orchestrates Trino, Spark, Kubernetes, and internal services, making hard-coded, tool-specific integrations brittle and unscalable.


We identified four painful steps in the manual process:


-


**The Siren (🚨 Alert):** Disruption via generic alerts (Airflow Alerting /


[Opsgenie](https://www.atlassian.com/software/opsgenie/migration) ).


-


**The Hunt (🔍 Manual Investigation):** Navigating UIs to find the specific task instance.


-


**The Deep Dive (📜 Log Diving):** Parsing megabytes of distributed logs to find a single error line.


-


**The Synthesis (🤔 Cognitive Load):** Mapping the error back to recent code changes.


**The Price of Friction** This workflow wasn't just tedious; it exacted a heavy cost across three critical dimensions. First, it created


**Operational Latency** , where the Mean Time To Recovery (MTTR) was spent primarily on locating bugs rather than fixing them, causing engineers to lose hours navigating tools and delaying critical data SLAs.


This incurred a significant


**Opportunity Cost** , as highly paid engineers were forced to act as "human routers," spending valuable cycles context-switching to perform repetitive investigation tasks instead of shipping new features. Above all, there was a


**Human Cost** ; the mental effort of connecting dots across different systems was exhausting, turning on-call shifts into a source of concern rather than just a routine part of the job.


##


**3. The Solution: AirBot**


AirBot is an


**AI-powered slack agent** designed specifically for the Wix ecosystem. However, the concepts behind it are universal. To solve the combinatorial complexity of our stack, we leveraged Large Language Models (LLMs) to transform alerts from static notifications into active investigations.


Unlike a passive bot that simply reposts error logs, we designed AirBot with three core capabilities that any SRE agent should possess:


##


**4. Architecture & Design: A Blueprint for Building Your Own**


Moving beyond simple API wrapping, AirBot utilizes a microservices architecture prioritizing security and modularity. If you are looking to build a similar tool for your organization, these are the architectural patterns we found essential.


###


**4.1 Connectivity: Security First**


Securing internal tooling within Wix’s fortified network presents a dilemma: how to allow a cloud-hosted bot access to internal Airflow clusters without compromising the security perimeter?


Traditional Slack apps rely on


[HTTP Requests](https://docs.slack.dev/apis/events-api/) , where Slack sends POST requests to a public URL. This forces the organization to "punch holes" in the firewall and expose endpoints to the public internet.


**The Fix:** We solved this by using slack


****[Socket Mode](https://docs.slack.dev/apis/events-api/using-socket-mode/) . Instead of waiting for inbound traffic, the bot initiates a secure,


*outbound* WebSocket connection to Slack. This architecture offers three critical advantages for internal tools:


-


**Zero-Trust Security:** Because the connection is outbound, there is


**no need** to open inbound firewall ports. The bot sits safely inside the private network, completely invisible to the public internet yet fully interactive.


-


**Performance:** Unlike webhooks that require a new HTTP handshake for every request, Socket Mode maintains a persistent, stateful connection.


-


**Developer Experience:** This architecture simplifies local development, allowing engineers to receive events locally without complex tunneling tools like


ngrok


.


The system is built on the


[Slack Bolt Python](https://docs.slack.dev/tools/bolt-python/getting-started/) framework for connection management and wrapped in


[FastAPI](https://fastapi.tiangolo.com/) .


###


**4.2 The Intelligence Layer: Multi-Platform MCP & Logic**


We utilize


**Model Context Protocol (MCP)** to give the bot visibility into our infrastructure. A critical decision was building a


**Custom Airflow Logs MCP** rather than using the standard Airflow API.


**Key Integration Strategies:**


-


**Granular Security:** Avoiding "God Mode" API user (


[REST Airflow API](https://airflow.apache.org/docs/apache-airflow/stable/stable-rest-api-ref.html) ). Our MCP uses IAM roles to access S3 buckets directly, respecting least-privilege principles.


-


**Semantic Search:** Implementation of tools to access relevant errors without loading massive log files into the LLM context.


*Credit to* ***Avidan Cohen*** *and* ***Gal Salomon*** *for architecting this secure retrieval layer.*


**Integration Points:** To make the bot "agentic," we gave it access to specific tools via MCP’s:


-


[GitHu](https://docs.github.com/en/rest?apiVersion=2022-11-28) **b:** Performs static analysis on failing code and auto-generates Pull Requests.


-


**Trino & Spark:** Executes diagnostic SQL and analyzes internal metrics.


-


**OpenMetadata:** Retrieves tables & columns schemas & descriptions for business context.


-


**DDS (Data Discovery Service):** Retrieves table lineage for data dependencies.


-


**Ownership Tag:** Routes alerts to the specific team owning the data asset, not just the pipeline maintainer.


###


4.3 The Reasoning Engine


We deployed a


**Chain of Thought** architecture using


[LangChain](https://docs.langchain.com/oss/python/langchain/overview) for the Main Automated Alert Processing Flow.


**The Logic Flow:**


1.


**Classification Chain:** Identifies the Operator (e.g., Spark vs. Trino) and Error Category (Syntax vs. Timeout).


2.


**Analysis Chain:** Ingests code and logs to determine the root cause.


3.


**Solution Chain:** Generates a remediation plan or PR.


**Model Selection Strategy:** To balance cost and intelligence, we dynamically select models:


-


GPT-4o Mini (The Sprinter): Used for high-volume tasks like log classification (e.g classify the log error type). Fast and cheap.


-


Claude 4.5 Opus (The Thinker): Used for complex root cause analysis requiring large context windows and deep reasoning (e.g generate a python code for PR creation).


**Structured Output Guardrails**


LLMs are inherently non-deterministic, but SRE automation demands reliability. To bridge this gap, AirBot utilizes


[Pydantic](https://docs.pydantic.dev/latest/concepts/models/) **** output models


**** to enforce strictly typed JSON responses. Instead of requesting free-text solutions, we prompt the model to populate a specific


RemediationPlan


object. This ensures downstream code can reliably parse the AI's suggestions.


###


**4.4 Deployment & Infrastructure**


To ensure reliability and maintainability, the application infrastructure is built on modern DevOps principles:


-


**Containerization:** The entire bot logic is containerized using


**Docker** , ensuring identical execution environments across local testing and production.


-


**Serverless Application:** The bot is deployed as a


**Serverless Application** , allowing it to handle traffic efficiently without idle resource waste.


**Secret Management:** We leverage


**Vault** to inject sensitive credentials.


##


**5. Operational Workflows: A Day in the Life of AirBot**


Two examples demonstrate the bot's practical utility.


**Scenario A: The Broken Schema (Trino Operator)**


-


**Incident:** A query fails because a column (


[r.open](http://r.open/) _date


) does not exist in the target table.


-


**Action:** AirBot fetches the SQL from GitHub and the Schema from OpenMetadata. It identifies the mismatch.


-


**Resolution:** It opens a PR swapping the incorrect column for the correct one (


r.start_date


) and presents a "Review PR" button in Slack.


**AirBot response** to the Opsgenie alert


-


The


**PR** in Github


**Scenario B: The Silent Delay (DDs Operator)**


-


**Incident:** A pipeline times out waiting for data. It's not a code error.


-


**Action:** AirBot queries internal APIs to find the specific upstream table causing the delay (


prod.wt_logged_users.blocks


).


-


**Resolution:** It resolves the ownership tag of the


*non-updated* table and notifies that specific team, bypassing the downstream engineer entirely.


**AirBot response** to the Opsgenie alert


-


The Alert to the table


**Team Owner** slack channel


##


**6. Impact & ROI: By the Numbers**


Beyond simply reducing repetitive work, the metrics gathered from 30 team channels supporting 60 Data Engineers show a measurable shift in our operational efficiency from the last 30 days.


-


The PR Funnel: AirBot generated 180 candidate Pull Requests across our main repositories. While 28 were merged directly without human code changes (a 15% fully automated fix rate), many unmerged PRs still provided value by acting as a "Blueprint", helping engineers visualize the solution faster even if they chose to implement the fix manually.


-


**Time Reclaimed & Improved SLAs** **:**


A typical manual debugging cycle takes ~45 minutes. AirBot cuts this by at least 15 minutes per incident. This acceleration allows us to resolve production issues significantly earlier, directly improving data freshness and ensuring we meet strict SLAs for customer-facing features.


-


The Math: Based on 4,200 successful flows and a


**66% positive feedback rate** , we calculate ~2,700 impactful interventions.


-


Total Savings: 2,700 × 0.25 hours =


**675 engineering hours saved per month** .


That is the equivalent of adding


**~4 full-time engineers** to the organization, purely through automation.


-


Cost Efficiency: An average AirBot AI interaction workflow costs ~$0.30. When compared to the salary cost of saving 15 minutes of engineering time, the ROI is immediate.


-


**Daily Usage Trends**


##


**7. Conclusion: A Blueprint for Scaling Ops**


At the scale of thousands of pipelines, manual intervention inevitably becomes a bottleneck. However, AirBot demonstrates a lesson valuable to


*any* data organization: combining secure architecture with LLM reasoning allows us to break the link between system growth and operational overhead.


By adopting patterns like Socket Mode for security, MCP for context, and tiered reasoning for cost control, you can shift the on-call experience from reactive log-parsing to proactive decision-making. The ultimate value lies not just in the hours saved, but in the capability to let engineers focus on what they do best—designing architecture rather than maintaining it.


*Special thanks to* ***Nadav Mirkin*** *for guidance and support throughout this project.*


This post was written by


**Yarden Wolf**


**More of Wix Engineering's updates and insights:**


-


Follow us on:


[Twitter](https://twitter.com/WixEng) |


[Facebook](https://www.facebook.com/WixEngineering/) |


[LinkedIn](https://www.linkedin.com/showcase/wix-engineering/) |


[TikTok](https://www.tiktok.com/@wix_engineering)


-


Join our


[Telegram channel](https://t.me/wixeng)


-


Visit us on


[GitHub](https://github.com/wix)


-


[Subscribe to our monthly newsletter](https://www.wix.engineering/subscribe)


-


Subscribe to our


[YouTube channel](https://www.youtube.com/WixTechTalks)


-


[Follow our Medium publication](https://medium.com/wix-engineering)


-


Listen to our podcast on


[Apple](https://podcasts.apple.com/il/podcast/wix-engineering-podcast/id1503976848) ,


[Spotify](https://open.spotify.com/show/5CmjtjpdcKkHDnr0601uYS?si=PcOf7Rx_RUmGojFj5n7CEA) or


[Google](https://podcasts.google.com/?feed=aHR0cHM6Ly9yYW5sZXZpLmNvbS9mZWVkL3dpeF9wb2Qv&ved=0CAAQ4aUDahcKEwjY3bLcy7_oAhUAAAAAHQAAAAAQAQ)
