---
schema_version: "1.0.0"
document_id: "fda0412cd370b7f6210033e6d85d89b64490b74803be01b0a58c256793d4352a"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-01424a9593db"
canonical_url: "https://slite.com/changelog/slite-ask-now-powered-by-the-super-engine"
published_at: "2026-03-17T00:00:00+00:00"
first_seen_at: "2026-07-24T13:26:38.265771+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:4506673fb0bbf11290aa33e4696890b5644853e141783f60b5f45cdb3a8c07c3"
---

# Slite Ask, now powered by the Super engine

Ask has been upgraded to run on the same AI engine that powers[Super](https://super.work/?redirect=no) . This brings a significant improvement in answer quality, reliability, and speed.


- Answers are more accurate and more consistent
- Ask is now available on public docs
- No custom API key required — Ask uses our optimized setup by default
- The experience has been refreshed with a new UI closer to Super's


### In-editor AI features: rebuilt from the ground up


All AI features accessible from the editor toolbar have been rebuilt with a new backend.


- The AI now understands every block and content type in your docs — tables, hints, diagrams, lists, and more
- Rewrites, summaries, and suggestions are more accurate and better fit the structure of your content


### Redesigned billing settings page


The billing settings page has been completely redesigned for a cleaner, more intuitive experience.


- All billing information in one screen
- Clearly shows seat counts for Slite, Super, or both
- Faster access to subscription details and payment management


### Enhanced Slite MCP


The Slite MCP server has been significantly expanded with new tools and capabilities.


**New tools:**


- ` move-note` — Move documents to reorganize your workspace
- ` update-note` — Update document titles and content
- ` archive-note` and` restore-note` — Archive and restore docs
- ` create-collection` and` update-collection` — Create and manage Collections
- VerificationExpired review state now exposed in API and MCP


**Change tracking:**


- MCP edits are now tracked in document history with a bot icon indicator
- Easily identify which changes were made via MCP versus manual edits


[Learn how to set up ↗](https://slite.slite.page/p/77mvFqJWG1tduF/Slite-MCP)


### Sketches & Mermaid Diagrams in PDF exports


Sketch and Mermaid Diagrams elements are now included when exporting docs to PDF — so your visual annotations, drawings and diagrams make it into the final export.


### Improvements


- Video player fullscreen functionality improved using native Fullscreen API
- Comments and block action gutters hidden on small screens for better mobile experience
- Multiple date variables now supported in document titles
- Usage analytics now includes Slack Ask queries
- Knowledge Suite teams can now control who has Super seats
- API keys now support read/write scopes


### Fixes


- Fixed hint and quote blocks not being linkable
- Fixed table copy/paste creating ID conflicts
- Fixed Linear tiles sometimes showing blank content
- Fixed sketch color picker crashing documents
- Fixed missing answers in Ask not being auto-reported
- Fixed Ask links opening in new tabs instead of same tab
- Fixed Ask filters not being applied from search input
- Fixed metadata changes not saving in protected collections
- Fixed comment threads breaking on archived zombie threads
- Fixed long Mermaid diagrams not displaying fully
- Fixed Mermaid diagram select-all behavior on Windows
- Fixed full-width tables not reverting to normal width
- Fixed MCP-created table rows having invalid positions
- Fixed internal Slite links from API/MCP not unfurling as doc links
- Fixed MCP not creating synced cards for Linear/Jira links
- Fixed Confluence import images not displaying
