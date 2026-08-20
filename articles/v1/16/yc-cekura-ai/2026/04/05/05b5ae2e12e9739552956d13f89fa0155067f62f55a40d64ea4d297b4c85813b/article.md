---
schema_version: "1.0.0"
document_id: "05b5ae2e12e9739552956d13f89fa0155067f62f55a40d64ea4d297b4c85813b"
company_key: "yc-cekura-ai"
company: "Cekura"
source_id: "yc-cekura-ai-news-import-2109cda3ccd2"
canonical_url: "https://www.cekura.ai/blogs/knowledge-base-connectors-rag-agentic-retrieval-voice-ai-agents"
published_at: "2026-04-25T00:00:00+00:00"
first_seen_at: "2026-07-24T05:11:51.123297+00:00"
fetched_at: "2026-07-28T21:45:29.554500+00:00"
content_hash: "sha256:5ad270c5eb63c1f38b00e599a2995abad362d126e9d8d132b504ee849de3963e"
---

# Knowledge Base Connectors and RAG: Agentic Retrieval for Voice AI Agents

## Introduction


Modern AI systems need access to dynamic, up-to-date information. Static knowledge bases quickly become stale, and manual updates don't scale. This post explores how to build production-grade knowledge base connectors that automatically sync external data sources, and how to implement agentic retrieval patterns using the Claude Code SDK.


We'll dive into real production architecture from Cekura's voice AI testing platform, examining:


- Multi-source knowledge base connector architecture
- Async syncing with scheduled refreshes
- Agentic retrieval patterns for intelligent context loading
- Security considerations (SSRF protection, credential encryption)


## Part 1: Knowledge Base Connector Architecture


### The Problem


AI agents need fresh knowledge from multiple sources:


- **Data warehouses** (e.g. BigQuery) - Analytics data, customer metrics, historical trends
- **Websites** - Public documentation, blog posts, knowledge bases
- **File uploads** - Internal documents, reports, and reference material in any format


Each source has different:


- Authentication mechanisms (service accounts for data warehouses, optional headers for websites)
- Data formats (JSON from queries, HTML/Markdown from pages, raw content from uploaded files)
- Update frequencies (hourly to daily syncing)
- Security requirements (SSRF prevention for websites, credential encryption for all)


### Core Architecture


The production schema uses two main models:


**KnowledgeBaseFile** - Stores individual knowledge base files in S3/Supabase:


- Links to AI agents
- Accepts files in any format — drop in whatever your team works with
- Tracks file metadata (name, type, size, timestamps)


**KnowledgeBaseConnector** - Manages external data source syncing:


- Connects to AI agents
- Supports multiple connector types (data warehouse queries, website scraping, and more)
- Separates credentials (encrypted) from configuration (public)
- Integrates with Celery Beat for automatic scheduling
- Tracks sync status (pending, syncing, success, failed)
- Uses ManyToMany relationship with files (website scrapers create multiple files per sync)


**Key Design Decisions:**


1. **Separation of Concerns** : Credentials vs. configuration split keeps sensitive data encrypted while allowing public configuration inspection
2. **ManyToMany Files** : Website scrapers create one file per page; data warehouse connectors create consolidated files
3. **Celery Beat Integration** : Automatic scheduling without custom cron jobs
4. **Status Tracking** : Observability for sync failures and debugging


### Connector Implementations


#### Data Warehouse Connector (e.g. BigQuery)


Executes SQL queries and stores results as JSON files. BigQuery, for example, uses Google Cloud service account authentication. Configuration includes the SQL query to execute, with support for parameterized queries. Results are converted to JSON with unique timestamped filenames. The same pattern extends to other data warehouses and query engines your stack relies on.


#### Website Scraper Connector


The website scraper fetches and converts web content to knowledge base articles:


- SSRF protection: blocks private IPs, localhost, link-local, and multicast addresses
- Converts HTML to markdown format
- Supports crawling multiple pages with pagination


### Async Syncing with Celery


Syncing runs asynchronously via Celery tasks to avoid blocking the main application:


- Executes connector-specific data fetching (data warehouse queries, website scraping, or other source types)
- Stores fetched content in S3 and links to the connector
- Tracks sync status with timestamps for observability
- Implements automatic retry with exponential backoff on failures


### API Endpoints


A REST API provides full CRUD operations for managing connectors, including:


- Creating connectors (triggers immediate sync)
- Listing and filtering by agent
- Manual sync triggering
- Updating configuration and credentials


Security features include credential exclusion from list views and duplicate sync prevention.


## Part 2: Agentic Retrieval with Claude Code SDK


Now that we have knowledge bases automatically syncing, how do we intelligently retrieve the right context for each agent call?


### The Challenge


Consider a customer support bot with:


- 1,000+ KB articles
- 50+ product documentation pages
- Real-time metrics from a data warehouse
- User conversation history


**Problem** : We can't send all 1,000 articles to Claude on every request (context window limits, latency, cost).


**Solution** : Agentic retrieval - let Claude decide what knowledge it needs and fetch it dynamically.


### Agentic Retrieval Architecture


```text
KB Sync  →   Download all KB files to local filesystem
↓
Claude Agent SDK runs in sandbox environment
↓
Agent has read access to KB directory
↓
User Request  →   Claude uses Read tool to access relevant KB files
↓
Claude responds with KB context


```


### Knowledge Base Manager


A singleton manager handles KB lifecycle:


- Fetches and caches KB files locally from the backend API
- Automatically refreshes stale content (24 hour TTL)
- Cleans content for optimal context (removes links, formatting noise)
- Initializes lazily without blocking the application


### Building a Claude Agent with KB Access


The Claude Agent SDK provides built-in filesystem tools that give Claude direct access to the knowledge base:


**Read** - Read contents of specific KB files


**Glob** - Find files matching patterns (e.g.,` **/*.txt` to find all text files)


**Grep** - Search file contents for keywords or topics


**Workflow Example** : User asks: "How do I set up voice AI testing with custom prompts?"


Behind the scenes, Claude:


1. Uses Grep to search KB files for keywords: "voice AI testing", "custom prompts"
2. Identifies relevant files from search results
3. Uses Read to load full content of matching files
4. Synthesizes answer using KB content
5. Returns response: "Based on our Voice AI Setup Guide, here's how to..."


Cekura integrates natively with Retell, VAPI, ElevenLabs, LiveKit, Pipecat, Bland, Synthflow, Cisco, Twilio, Plivo, and SIP — so KB retrieval works out of the box with whichever voice stack you're running.


## Security Best Practices


### Credential Encryption


All sensitive credentials (API keys, service accounts, tokens) are encrypted at rest using industry-standard encryption and never exposed in logs or API responses.


### SSRF Protection


Website scrapers validate URLs to prevent Server-Side Request Forgery attacks by blocking access to private networks, localhost, and internal IP ranges.


### Rate Limiting


API endpoints implement throttling to prevent abuse, with different limits for anonymous users, authenticated users, and sync operations.


### File Validation


Uploaded files are scanned for safety on ingestion, ensuring that accepting any format doesn't come at the cost of security.


### Compliance


Cekura is SOC 2 Type II audited and supports HIPAA-scoped deployments under BAA. GDPR-compliant with DPA available. Reports available under NDA — request via[sales](https://www.cekura.ai/expert) .


## Performance Optimization


### Parallel Downloads


Knowledge base files are fetched concurrently to minimize sync time, with timeout protection and graceful error handling.


### Caching


Frequently accessed KB articles are cached in-memory with TTL-based expiration to reduce disk I/O and improve response times.


### Retry Logic


Async tasks implement exponential backoff for transient failures while quickly failing on permanent errors to avoid wasted resources.


### Sync Status Dashboard


Admin interface provides visibility into connector health, sync history, and failure debugging with filtering and search capabilities.


### Metrics & Alerts


The system tracks sync durations, success/failure rates, and automatically alerts on repeated failures for proactive issue resolution.


### Agent Tracing


Structured logging captures tool usage, processing times, and errors for debugging and performance optimization.


## Conclusion


Building production-grade knowledge base connectors requires:


- **Flexible architecture** supporting multiple data sources and file formats
- **Async syncing** with retry logic and status tracking
- **Security hardening** (SSRF prevention, credential encryption, file validation)
- **Performance optimization** (parallel downloads, caching, efficient storage)
- **Observability** (metrics, alerts, trace logging)


Agentic retrieval with Claude Code SDK enables:


- **Dynamic context loading** - Claude uses filesystem tools to access relevant KB articles
- **Tool-based KB access** - Built-in Read, Glob, and Grep tools for intelligent retrieval
- **Flexible deployment** - Runs in sandbox environments with KB directory access


## Next Steps


Try building your own:


1. Start with a data warehouse connector (BigQuery is a great example) for analytics data
2. Add website scraping with SSRF protection
3. Drop in existing files — any format works out of the box
4. Set up async syncing with Celery
5. Build a Claude agent with filesystem tools for KB access
6. Deploy in a sandbox environment for production use
