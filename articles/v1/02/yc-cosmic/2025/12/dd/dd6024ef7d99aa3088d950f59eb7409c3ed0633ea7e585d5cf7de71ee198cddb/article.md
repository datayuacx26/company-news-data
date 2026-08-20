---
schema_version: "1.0.0"
document_id: "dd6024ef7d99aa3088d950f59eb7409c3ed0633ea7e585d5cf7de71ee198cddb"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/cms-debate-cosmic-ai-powered-development"
published_at: "2025-12-16T00:00:00+00:00"
first_seen_at: "2026-08-10T04:57:33.751116+00:00"
fetched_at: "2026-08-10T04:57:36.475692+00:00"
content_hash: "sha256:1e2c7d667e8e4c58b74a2939ef279eceeca910bac34be0fa613c23bf51ebd249"
---

# The CMS Debate: Why Cosmic Offers the Best of Both Worlds for AI-Powered Development

The developer community is engaged in an interesting debate about the role of CMSs in an AI-first world. Lee Robinson's recent article about[migrating cursor.com from a CMS to raw code and Markdown](https://leerob.com/agents) sparked intense discussion, followed by a thoughtful response from Sanity's team titled["You should never build a CMS"](https://www.sanity.io/blog/you-should-never-build-a-cms) . Both perspectives reveal important truths—and point toward a better path forward.


## The Case Against Traditional CMSs


Lee's experience resonates with many developers. He spent $260 in tokens and ran hundreds of coding agents to migrate cursor.com away from their headless CMS, completing in three days what he estimated would take weeks. His core argument: **AI coding agents work best when content is code** .


The pain points he identified are real:


### 1. Abstraction Overhead


Traditional CMSs introduce layers between developers and their content. Instead of asking an AI agent to modify a navigation menu directly in code, you're clicking through UI menus. The network boundary between your codebase and CMS content becomes a barrier to AI-powered workflows.


### 2. Preview Complexity


Getting draft content visible requires draft mode, toolbar toggles, account management, and often multiple authentication systems. Lee notes that with code-based content, you simply create a PR and share a preview URL—no login required.


### 3. Context Fragmentation


AI agents excel at grep-ing codebases. When content lives behind an authenticated API, agents can't easily access it. This context barrier limits what AI can accomplish autonomously.


### 4. Operational Costs


Lee's team spent $56,848 on CDN usage in a few months—a hefty premium for CMS convenience. While this was largely due to hosting video through the CMS (not ideal), it illustrates how quickly costs can escalate.


## Sanity's Counterargument: You're Building a CMS Anyway


Sanity's response makes an equally compelling case. Their key insight: **Lee didn't eliminate the CMS—he rebuilt one** .


Look at what cursor.com ended up with:


- Asset management GUI (built with "3-4 prompts")
- User management via GitHub permissions
- Version control via git
- Localization tooling
- Content model (markdown frontmatter)


These are CMS features. They exist because the problems are real: managing assets, controlling publishing permissions, tracking changes, structuring content for reuse.


Sanity argues that the real issues aren't with CMSs conceptually, but with **how they've been implemented** :


### Git Isn't Built for Content


Git excels at code but struggles with content collaboration:


- Merge conflicts in content are semantic, not mechanical
- Line-based diffing doesn't capture paragraph-level changes
- Real-time collaboration matters for distributed content teams
- Branching doesn't map to content workflows


### Structured Content Beats Markdown at Scale


When your pricing lives in three places (pricing page, comparison table, footer), markdown files require updating three separate files. With structured content, you update once and references propagate automatically. It's the same reason you don't store customer names as strings in every order row—you normalize data.


### Grep Isn't Query


Lee celebrates that agents can grep the codebase. But grep is pattern matching, not semantic search. Try writing a grep command for "the three most recent case studies in the finance category." You can't—you need a proper query engine.


## The Missing Piece: Separate Tools for Separate Teams


Both perspectives miss a crucial insight: **the future isn't CMS vs. code—it's AI-native platforms that give each team the tools they need** .


This is where Cosmic comes in.


## Cosmic: The Best of Both Worlds


[Cosmic](https://www.cosmicjs.com/) was built from the ground up as an AI-powered content management and application development platform. We've addressed the exact pain points Lee identified while preserving the structured content benefits Sanity champions.


### For Developers: AI Agents That Work in Code


Lee's frustration was that AI agents couldn't easily work with CMS content. Cosmic solves this fundamentally differently than traditional CMSs.


**Code Agents** integrate directly with your GitHub workflow:


- Work within your actual codebase, not through a CMS UI
- Create isolated branches for each task automatically
- Execute parallel tasks without conflicts
- Generate preview URLs that anyone can access (no authentication required)
- Create pull requests ready for review with complete context


When you ask a Code Agent to "add a new landing page for our enterprise features," it:


1. Creates a new branch in your repository
2. Generates the page component with proper routing
3. Pulls structured content from Cosmic's API
4. Implements responsive design and SEO optimization
5. Creates a preview deployment automatically
6. Opens a PR with all changes documented


**The key difference** : Developers stay in their native environment (code, GitHub, terminal) while AI agents handle both the code *and* content structure. There's no clicking through CMS menus because developers aren't using the CMS—they're using AI agents that understand both worlds.


### For Content Teams: AI Agents That Work in Content


Meanwhile, content teams get their own AI-powered environment designed for content operations, not code.


**Content Agents** work within the Cosmic dashboard to:


- Generate new blog posts matching your existing tone and style
- Bulk update metadata across hundreds of objects
- Create SEO-optimized descriptions at scale
- Generate localized versions of content
- Run scheduled content operations (daily social posts, weekly newsletters)


Content teams can say "Create 5 blog post drafts about our new API features, each focusing on a different use case" and Content Agents will:


1. Analyze your existing blog content for style and structure
2. Research your API documentation for accuracy
3. Generate complete drafts with images, metadata, and SEO fields
4. Organize them with proper categories and tags
5. Set them to draft status for editorial review


**The key difference** : Content teams never see code or Git commands. They work in a content-focused interface where AI handles the heavy lifting of generation, optimization, and organization.


### Unified Architecture, Separate Experiences


This is the crucial insight both Lee and Sanity missed: **you don't need to force everyone into the same workflow** .


Developers want to work in code with AI agents that understand version control, branching, and pull requests. Content teams want to work with content through interfaces designed for editorial workflows, publishing schedules, and content relationships.


Cosmic provides both experiences backed by the same unified content infrastructure:


```text
// Developers use the SDK in their code
import     {   createBucketClient   }     from     '@cosmicjs/sdk'


const   cosmic   =     createBucketClient  (  {
bucketSlug  :     'your-bucket'  ,
readKey  :     'your-read-key'
}  )


// Query structured content like any other data source
const   posts   =     await   cosmic  .  objects
.  find  (  {   type  :     'blog-posts'     }  )
.  props  (  'slug,title,metadata'  )
.  depth  (  1  )
```


While content teams use the dashboard to:


- Manage that same content through an intuitive interface
- Collaborate in real-time without merge conflicts
- Use AI to generate and optimize content at scale
- Maintain relationships between content types
- Schedule publishing and track content workflows


### Solving the Real Problems


Let's revisit Lee's core complaints and show how Cosmic's dual-tool approach solves them:


**Preview Complexity** ✅
Developers generate preview URLs from their Git workflow—no CMS login required. Content teams generate previews from the dashboard—no terminal commands required. Both get shareable URLs that work for anyone.


**Context Fragmentation** ✅
AI Code Agents see your entire codebase *and* can query your content structure through Cosmic's API. AI Content Agents see your entire content library with full relationship context. No artificial boundaries.


**User Management** ✅
One SSO system (GitHub, Google, email). Developers added as developers, content team as editors. Each role gets appropriate permissions and tools without extra configuration.


**Workflow Mismatch** ✅
Developers use Git workflows (branches, PRs, reviews). Content teams use content workflows (drafts, scheduled publishing, editorial review). The underlying system supports both naturally.


**Operational Costs** ✅
Efficient asset delivery without CMS markup. Optional integration with specialized services (like Mux for video). No surprise bills because content and infrastructure are separate concerns.


### The Architecture Advantage


Cosmic's architecture enables this dual-experience approach because we separate:


**Content Structure** (the "what")


- Defined through Object Types and Metafields
- Queryable through powerful APIs
- Accessible to both code and content tools
- Versioned and validated automatically


**Content Access** (the "how")


- Developers access via SDK in their code
- Content teams access via dashboard interface
- AI agents access via specialized APIs
- All backed by the same unified data layer


This separation means:


- **Sub-100ms API responses** for developers querying content
- **Real-time collaboration** for content teams without code conflicts
- **Complete context** for AI agents across both domains
- **Single source of truth** despite multiple access patterns


## Real-World Performance: The Proof


Consider the workflows each approach enables:


**Lee's Approach (Code + Markdown)** :


1. Update navigation items in code
2. Commit changes
3. Push to GitHub
4. Wait for deployment
5. Preview on staging


**Traditional CMS Approach** :


1. Log into CMS
2. Navigate through UI
3. Update content
4. Trigger rebuild
5. Wait for deployment


**Cosmic Approach** :


1. Tell AI Agent: "Update navigation to include new Contact Sales page"
2. Agent updates both content model and code
3. Preview link generated instantly
4. Approve and merge


Cosmic's unified architecture enables operations that would be slow or complex in either pure-code or traditional CMS approaches:


1. Code Agent workflow: Generate content with AI, create pages
2. Content Agent workflow: Generate blog posts, optimize SEO
3. Both working simultaneously, no conflicts


This entire operation—whether it's code generation or content creation—completes in seconds because agents work in their native environment with full context.


## Who This Serves


### Developers Get:


- AI agents that work with code AND content
- GitHub integration and familiar workflows
- Fast iteration cycles without CMS friction
- Clean API they can reason about
- No operational overhead


### Content Teams Get:


- AI assistance for generation and optimization
- Real-time collaboration without Git commands
- Structured content that scales
- Workflow states and publishing controls
- No technical barriers


### Business Owners Get:


- Complete platform from concept to production
- Predictable, transparent pricing
- Fast time-to-market
- Both teams working efficiently
- Scalability without complexity


## The Bottom Line


The CMS debate presents a false choice between "everything in code" and "everything in a CMS."


Cosmic recognizes that different team members need different tools:


- **Developers** get AI-powered code agents that work with GitHub workflows
- **Content teams** get AI-powered content agents that work with editorial workflows
- **Both** benefit from structured content that scales


Lee was right that forcing developers through CMS UIs is frustrating. Sanity was right that you can't escape content management problems. Cosmic shows there's a better way: give each team AI-enabled tools designed for *their* workflow while sharing the same content infrastructure underneath.


That's not compromise—it's the best of both worlds.


---


*Ready to experience unified, AI-enabled content and code management?[Explore Cosmic](https://www.cosmicjs.com/) and see how our platform bridges the gap between traditional CMSs and AI-powered workflows.*
