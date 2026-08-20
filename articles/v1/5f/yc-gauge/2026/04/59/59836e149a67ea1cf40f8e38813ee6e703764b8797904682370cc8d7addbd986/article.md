---
schema_version: "1.0.0"
document_id: "59836e149a67ea1cf40f8e38813ee6e703764b8797904682370cc8d7addbd986"
company_key: "yc-gauge"
company: "Gauge"
source_id: "yc-gauge-news-import-b2f5831b5413"
canonical_url: "https://www.withgauge.com/blog/how-to-build-a-blog-with-next-js-three-approaches-to-content-management/"
published_at: "2026-04-06T00:00:00+00:00"
first_seen_at: "2026-07-21T21:19:22.794183+00:00"
fetched_at: "2026-07-28T22:16:01.195245+00:00"
content_hash: "sha256:11858330927475a0ae0b1c44fd36a5ad84ff708160957e1ee49401c231e6b2ae"
---

# How to Build a Blog with Next.js: Three Approaches to Content Management

Next.js sites are quickly becoming the default for marketing sites and disrupting website builders like WordPress, Webflow, and Framer. When code is cheap and just a prompt away, you might as well roll your own site.


If you're building a blog into a Next.js app, one of the first decisions you'll make is where your content lives. In this post, I overview three common patterns for building your blog and discuss the tradeoffs of each. Spoiler alert: I recommend hosting your content in a database with something like[Supabase](https://supabase.com/) (one of our customers at Gauge!).


## Blog Pattern 1: File-based markdown


Write each blog post as a` .md` or` .mdx` file committed to your project. At build time, Next.js reads the files, parses the markdown, and generates static pages.


This is the simplest pattern to set up. There's no database, no external service, and no API layer. Your content lives right next to your code.


The tradeoff is that every content change goes through your development workflow. Fixing a typo means opening a PR. Publishing a new post means triggering a build.


For personal blogs or documentation sites where the author is also the developer, that's fine. My personal site ([ethanfinkel.com](https://www.ethanfinkel.com/) ) runs off this pattern. But for teams where non-engineers need to publish, this pattern creates some serious friction.


**Works well for:** Personal dev blogs, open-source documentation, small sites with infrequent updates.


**Gets painful when:** Non-technical teammates need to publish, or you're posting frequently enough that the commit-build-deploy cycle slows you down.


## Blog Pattern 2: Headless CMS


Platforms like Sanity, Ghost, Contentful, and Strapi give you a hosted content backend with a visual editor, structured content models, and an API to query from your Next.js app.


These tools are purpose-built for content management and they do it well. Rich editing, media handling, draft/publish workflows, role-based access. For content-heavy operations with multiple editors, these features are worth the investment.


The tradeoff is complexity. You're adding an external dependency to your stack. Each platform has its own content modeling approach that you need to learn and work within. Some require their own hosting (Strapi, Ghost self-hosted). Others come with usage-based pricing that scales up as your content grows (Sanity, Contentful).


For a startup blog with a handful of posts per month, the overhead usually outweighs the features. You end up managing a CMS when all you needed was somewhere to put your markdown.


**Works well for:** Teams with dedicated content editors, sites that need localization, approval workflows, or complex content relationships.


**Gets painful when:** You just need a straightforward blog and the CMS platform becomes another thing to manage and pay for.


## Blog Pattern 3: Database-driven markdown


This is the pattern I use and recommend for most startups. Instead of storing your content as markdown files in your git repo, you store the content directly in a database. Each post is a row in a table with fields for the markdown body, title, slug, tags, and metadata. Your Next.js app queries the database at request time (or with ISR), parses the markdown, and renders the page.


You get the key benefit of a CMS (no redeploy to publish) without the overhead of adopting a full platform. But, since you own code for the data model and rendering, you can extend both whenever you need to.


The tradeoff is that you're building the content infrastructure yourself. There's no visual editor unless you build one. No media library, no drag-and-drop. But the implementation is pretty straightforward and easy to vibe code.


I recently created a new site called[vcsoftware.vc](https://www.vcsoftware.vc/) where I used this pattern, and it's been extremely easy for me to scale content as a result. I can just write directly into my home built UX and have it render without needing to redeploy. Based on my experience with all three, I would definitely recommend this option for teams scaling content curation. The engineering lift is not too high and marketing teams can easily use the infrastructure once it's set up.


‍


**Works well for:** Startups that want to publish without redeploying, teams that value control over their content rendering, and projects where the blog is part of a larger app already backed by a database.


**Gets painful when:** You need rich editorial workflows, multiple content editors with different permission levels, or localization across many languages.


## Implementing the database-driven approach


Here's how I'd build this from scratch with Next.js.


##### Set up your database


Use whatever you're comfortable with. Supabase and PocketBase are both solid if you don't already have a database in your stack. If your app already runs Postgres, just add a table.


Your posts table needs at minimum:


Column Type Purpose


` id` uuid / auto-increment Primary key


` title` text Post title


` slug` text (unique) URL identifier


` content` text Full markdown body


` excerpt` text (nullable) Short summary for meta descriptions and social cards


` published_at` timestamp (nullable) Publish date, null if unpublished


` is_draft` boolean Controls visibility


` created_at` timestamp Record creation time


` updated_at` timestamp Last edit time


Add columns for tags, categories, author, or featured image URL as you need them. The nice thing about owning the schema is that you can continually iterate on it with a simple database migration.


##### **Create a data access layer.**


Use an ORM like Prisma to keep your queries type-safe. You only need a handful of functions: get all published posts, get a post by slug, and upsert a post. That covers your blog index, post pages, and admin editor.


##### **Build your blog routes.**


A dynamic route that takes a slug, fetches the post, and renders the markdown. A blog index that lists all published posts. Generate your SEO metadata directly from the post row so titles, descriptions, and Open Graph tags stay in sync automatically.


##### **Handle custom components.**


Since the page is rendered as JavaScript, you can intercept anything in the markdown and replace it with a React component. Callouts, product cards, interactive demos. If you can build it in React, you can embed it in a blog post.


##### **Build a simple admin interface.**


You don't need a polished CMS dashboard. A text input for the title, a textarea for markdown, and a few dropdowns for metadata. Add a side-by-side markdown preview if you want a better writing experience. I built one for a project recently. Took a few hours.


##### **Server-side render everything.**


This is the important part. Your blog pages should be server-rendered, not client-rendered. When a post is fetched and rendered on the server, new content is live immediately without a redeploy. No build step, no static generation, no waiting. Write a post, save it to the database, and the next request serves it.


## Choosing the right pattern


Each pattern has a clear sweet spot:


File-based Headless CMS Database-driven


Setup effort Minimal Moderate Low-moderate


Publish without deploy No Yes Yes


External dependencies None CMS platform Database


Custom rendering MDX only Limited by CMS Full control


Editorial workflows Git-based Built-in Build your own


Best for Dev blogs, docs Content teams Startup blogs, custom apps


For most startups, the database-driven approach hits the right balance. It's quick to set up, it doesn't add platform dependencies, and it scales naturally with your product's existing infrastructure. You give up the polish of a dedicated CMS in exchange for simplicity and full control over how your content is stored and rendered.


If you're starting a blog today and you already have a Next.js app, spin up a Supabase project, create a posts table, and start writing. You can always graduate to a full CMS later if you need to. Most teams never do.


Related: once posts are shipping, see our guide on[optimizing content for AI citations](https://www.withgauge.com/blog/how-to-optimize-content-for-ai-citations) so the pieces you publish actually get picked up by AI search, and check out the[best AI content platforms](https://www.withgauge.com/resources/top-ai-content-platforms-in-2026) if you want to scale production beyond what one writer can keep up with.


‍
