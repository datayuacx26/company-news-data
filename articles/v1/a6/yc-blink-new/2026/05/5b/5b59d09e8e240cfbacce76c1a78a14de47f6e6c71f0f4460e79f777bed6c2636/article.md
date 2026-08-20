---
schema_version: "1.0.0"
document_id: "5b59d09e8e240cfbacce76c1a78a14de47f6e6c71f0f4460e79f777bed6c2636"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-wiki-app"
published_at: "2026-05-24T01:41:06+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:31.360725+00:00"
content_hash: "sha256:7b6d5f4d616fb728538a7be1fb587b0c3c8544006598bd68cd1d50e5fe588392"
---

# How to Build a Company Wiki With AI (Confluence Alternative in Hours)

## Step 1: Map Your Wiki Architecture


Before you open Blink, decide what goes where.


A standard structure for a 15–100 person company:


- **Engineering** — specs, architecture decisions, API docs, runbooks
- **Product** — roadmap, feature briefs, OKRs
- **HR** — policies, benefits, onboarding checklists (private)
- **Sales** — playbooks, case studies, scripts
- **Onboarding** — shared section every new hire reads first
- **External/Public** — API docs, changelog, customer-facing FAQs


Write down three things: what is private (HR), what is internal-only (all employees), what is public-facing (no login required). You will describe this permission model to Blink word-for-word in Step 3.


The structure you plan here becomes your top-level navigation. Every section after this one builds on it.


## Step 2: Build the Editor and Page Structure


Open[Blink](https://blink.new/) and describe the core system:


> "Build a company wiki. Pages have a title, body (rich text WYSIWYG editor), a parent page for nesting, an author, a last-modified timestamp, and a draft/published status. Nested pages go unlimited levels deep. Pages auto-save every 30 seconds."


Blink generates the database schema —` pages` table with` parent_page_id` ,` content` ,` author_id` ,` created_at` ,` updated_at` — and wires the editor. The database is included automatically. No Supabase account to configure.


The output: a working tree-structured page system with a rich-text editor, ready in minutes. Nested pages show in a sidebar tree just like Notion.


## Step 3: Add Team Permissions


This step is where every off-the-shelf tool forces a compromise. Your org chart rarely maps to Notion's teamspace model or Confluence's space structure cleanly.


Describe your exact model to Blink:


> "Add roles: admin (see and edit everything), editor (write in their assigned team section), viewer (read-only). Add team-level access: HR team can only see the HR section. Engineering can see all sections. External viewers can only see pages marked as public."


Blink adds auth with role-based access — built in, no Clerk or Firebase Auth to configure. State your permission rules in plain language. Blink implements exactly what you describe, not a closest approximation.


This is the step that justifies building instead of buying: the permission model matches your actual org chart on day one, not after a week of configuration workarounds.


## Step 4: Wire Up Full-Text Search


Search is what separates a used wiki from a digital graveyard.


> "Add full-text search across all pages. Index the page title and body content. Return results in under 500ms. Show a matching text snippet in each result. Respect the current user's permissions — HR-only pages should not appear in search results for Sales."


Blink builds the search endpoint with permission-aware filtering. Every page your team writes is indexed immediately. Every update shows in search within seconds.


Notion's search is notoriously slow at scale. Confluence's search is good but full-text search is only available on Standard and above. A custom search index is fast and free from the start.


## Step 5: Add Version History


Every save becomes a snapshot. Every change is attributed. Any version restores with one click.


> "Add version history to pages. Every save creates a version record with the full content, the editor's name, and the timestamp. Show the version history list in a sidebar panel. Let users preview any previous version and restore it."


Blink adds the` page_versions` table and wires the UI. No data migration — history starts accumulating from the first save after you deploy.


This is the feature that makes a wiki safe. Teams that can revert trust the wiki enough to edit it. Teams that can't revert treat it as read-only — and it dies.


## Step 6: Deploy and Bring Your Content Over


Deployment takes two minutes. Blink handles hosting — no Vercel config, no domain setup headaches.


For migrating existing content:


- **From Notion** : export as Markdown or CSV. Ask Blink to build an import screen for Notion's export format.
- **From Confluence** : use Confluence's built-in HTML export. Ask Blink to parse and import it.
- **From Google Docs** : export individual docs as Markdown, paste into the wiki editor.


You do not need to migrate everything on day one. Start with your most-referenced docs — onboarding guide, engineering setup, HR policies — and migrate the rest as you go.


The migration is the longest part. The build is not.


## What to Build Next


A working wiki is a starting point. Three additions most teams ship within the first month:


**AI "ask the wiki" assistant.** A chat interface that searches your wiki and answers questions in natural language. Ask Blink: "Add an AI assistant that can answer questions using the wiki content." Teams that add this cut repeated "where is X?" Slack messages by half.


**Slack /wiki command.** A slash command that searches and surfaces results in-channel. Ask Blink to build a webhook endpoint that accepts a search query and returns matching pages.


**Onboarding checklist linked to wiki pages.** A checklist new hires complete with each item linking to the relevant wiki page. Ask Blink: "Add an onboarding checklist where each task can link to a wiki page and check off as complete."


For teams that want to extend further: pair your wiki with a[custom internal tool dashboard](https://blink.new/blog/how-to-build-internal-tool) , or build a separate[customer-facing knowledge base](https://blink.new/blog/how-to-build-knowledge-base) for external docs. Both use the same Blink project — database and auth are already wired.


## Frequently Asked Questions


Notion is a general-purpose tool with a permission model designed around Notion's UI — not your org chart. A custom-built wiki lets you define the exact permission structure your team needs: engineering sees everything, HR pages stay private, external users see only public content. There are no per-seat fees that grow with every new hire and contractor, and no features locked behind a Business tier upgrade.


No. You describe what you want to Blink in plain English — "build a company wiki where engineering sees everything, sales only sees their section, and HR has private pages" — and Blink generates it. Every step in this article is a prompt, not a code block. Iteration happens in natural language.


Core wiki — editor, nested pages, search — under an hour. Adding permissions and version history adds another 30–45 minutes of back-and-forth with Blink. Total build time before you invite the team: 2–3 hours. Migrating content from Notion or Confluence is the longest part, and that depends entirely on how much you have.


Export from Notion as Markdown or CSV. Ask Blink to build an import screen that handles the format. For Confluence, use the built-in Export to HTML and ask Blink to parse it on import. You don't need to migrate everything at once — start with the five most-referenced documents and let the rest migrate naturally as people need them.


Ship it the same day you build it. Send the link in Slack, invite three people to edit the onboarding doc right now, and make it the answer you give the next time someone asks "where is X?" in Slack. The research is clear: wikis die from friction, not from imperfect content. A wiki built around your team's exact workflow — not Confluence's space model — has less friction from day one.


Yes. Ask Blink to add a public toggle on pages. Pages marked public are accessible without login — useful for API docs, changelogs, or customer-facing FAQs. The permission layer built in Step 3 handles this natively: external viewers see only pages where` is_public` is true. No separate deployment or subdomain needed.
