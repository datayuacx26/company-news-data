---
schema_version: "1.0.0"
document_id: "4631188aa26f8a69ecff1a4760aca786ca68cda16048ce3d5c05f508e64f092b"
company_key: "yc-slite"
company: "Slite"
source_id: "yc-slite-news-import-01424a9593db"
canonical_url: "https://slite.com/changelog/slite-mcp-server"
published_at: "2026-02-16T00:00:00+00:00"
first_seen_at: "2026-07-24T13:26:38.265771+00:00"
fetched_at: "2026-07-28T22:20:18.906160+00:00"
content_hash: "sha256:662cc3c9387f7bd3a05353f9351ecffcb327a82a340676481c11a29cf4048213"
---

# Slite MCP Server

Connect AI assistants like Claude and ChatGPT directly to your Slite workspace. Search docs, create notes, and update content through natural language—all while respecting your existing permissions.


Our MCP server follows the authenticated remote[MCP spec](https://modelcontextprotocol.io/specification/2025-03-26) , so it's centrally hosted and uses OAuth for secure authentication. No API keys or config files to manage.


Connect via` https://api.slite.com/mcp` in Claude, ChatGPT, and other MCP-compatible clients.


**Available tools include:**


- Search and retrieve docs with advanced filtering
- Create and update notes with full formatting support
- Access your recently edited and visited docs
- Find users, groups, and channels
- Modify specific blocks within docs


[Learn how to set up ↗](https://slite.slite.page/p/77mvFqJWG1tduF/Slite-MCP)


### Rich Text in Hints and Quotes


You can now add rich content inside hints and quotes, including bullet points, images, sketches, titles, and almost any formatting you'd use in regular content. This makes these blocks much more powerful for organizing complex information.


### Images in Comments


Comments now support images! You can paste, drag and drop, or use the image button in the comment toolbar to add visual context to your feedback and discussions.


### Improvements


- **Collections/Board View:** Increased contrast, full-height columns, improved view picker UX, and table/board selector now shows as tabs
- **Mermaid Diagrams:** Better editing experience with auto-closing of subgraphs and improved rendering
- **Diagram to Sketch Conversion:** Convert Mermaid flowcharts to Excalidraw sketches
- **Find & Replace:** Performance optimizations and support for adding Markdown formatting during replacement
- **Code Blocks:** Improved Python syntax highlighting with better multi-line string and comment support
- **Protected Documents:** Clearer UI when attempting to edit protected content
- **Archived Docs:** More visible banner indicating archived status


### Fixes


- Fixed Linear task tiles rendering as blank on mobile
- Fixed keyboard shortcuts applying formatting in protected table cells
- Fixed code block decorations with fast typing
- Fixed Find & Replace scrolling to top when closing
- Fixed expand arrow showing for empty channels in sidebar
