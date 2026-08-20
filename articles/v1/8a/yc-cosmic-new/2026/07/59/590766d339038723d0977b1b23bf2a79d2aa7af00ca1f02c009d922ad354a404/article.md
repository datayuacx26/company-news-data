---
schema_version: "1.0.0"
document_id: "590766d339038723d0977b1b23bf2a79d2aa7af00ca1f02c009d922ad354a404"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/migrate-from-umbraco-to-headless-cms"
published_at: "2026-07-23T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:bef2df067cf7f90cb7bd293d70a0e4f4a9d4b3015ffef60e4d31caceca97a26f"
---

# Migrating from Umbraco to a Headless CMS: A Developer's Guide

Umbraco has served a lot of teams well for a long time. It is a mature, open-source .NET CMS with a solid editor experience and a large community. But when teams move to a modern JavaScript frontend (React, Next.js, Astro, Nuxt), they hit a familiar set of friction points: Umbraco's .NET backend doesn't fit naturally into a Node/JS deployment pipeline, the API layer requires configuration work that headless-first platforms handle out of the box, and marketing teams end up bottlenecked waiting on developers for routine content changes.


This is the migration pattern Tripwire Interactive faced. The game developer behind the Red Orchestra and Killing Floor franchises was running Umbraco when they decided to rebuild their corporate website on a modern React stack. They needed each new game to get a dedicated landing page, and their marketing team needed to manage Blog, Career, and Press content without pulling in a developer every time.


They moved to Cosmic. Here is how a migration like that works, and what to think about before you start.


## Why teams leave Umbraco for headless


The common triggers:


- *Frontend rebuild.* Your team is moving to React, Next.js, Astro, or Nuxt and Umbraco's .NET backend creates a mismatch in the deployment stack.
- *Marketing team bottleneck.* Content changes require developer involvement because Umbraco's template system is tightly coupled to the backend.
- *API-first requirement.* You need to serve content to a web app, a mobile app, and potentially third-party integrations from one source of truth. Umbraco's Headless/Delivery API is available but adds configuration overhead compared to headless-first platforms.
- *Hosting and ops overhead.* Self-hosted Umbraco requires server maintenance, .NET runtime management, and database ops. A hosted headless CMS moves that off your plate.


None of this means Umbraco is a bad product. For .NET-native teams building traditional server-rendered sites it remains a strong choice. The question is whether your current stack still matches that profile.


## What you are migrating


A typical Umbraco-to-headless migration involves four things:


1. *Content types* (Document Types in Umbraco) → Object Types in your headless CMS
2. *Content nodes* (the actual content tree) → Objects
3. *Media library* → hosted media in your headless CMS
4. *Frontend templates* → your new JavaScript frontend (React, Next.js, etc.)


The frontend rebuild is usually the largest effort and is mostly independent of the CMS migration. The CMS migration itself (content types + content + media) is the more tractable part.


## Step 1: Audit your Umbraco content model


Before touching anything, export and document your existing Document Types. For each type, note:


- The type name and alias
- Every property (field), its data type (Textstring, Richtext, Media Picker, etc.), and whether it is required
- Any nested content or block list structures
- Relationships between types


Umbraco's backoffice gives you this via the Document Types section. You can also query the Content Delivery API to see the shape of your content in JSON, which maps more directly to what your headless CMS will expect.


## Step 2: Map Document Types to Object Types


In Cosmic, content is organized as Object Types with Metafields. Each Umbraco Document Type becomes a Cosmic Object Type. Each property becomes a Metafield.


Umbraco data type to Cosmic Metafield mapping:


- *Textstring* → metafield
- *Textarea / TinyMCE Richtext* → or metafield
- *Media Picker* → metafield
- *Content Picker / Multi-node treepicker* → / metafield (relationship)
- *Date Picker* → metafield
- *True/False* → metafield
- *Dropdown / Radiobutton list* → metafield
- *Checkbox list* → metafield
- *Nested Content / Block List* → or metafield group


For Tripwire, the mapping was straightforward: game landing pages, blog posts, career listings, and press releases each became a Cosmic Object Type with corresponding metafields.


## Step 3: Export content from Umbraco


Umbraco's Content Delivery API (available in Umbraco 13+) is the cleanest export path:


```text


```


For older Umbraco versions without the Delivery API, you have two options:


- Use the Umbraco Examine index or a custom controller to export content as JSON
- Use a community package like Umbraco.Headless.Client or a direct database query (riskier, last resort)


Export each content type to a separate JSON file. You will use these as your import source.


## Step 4: Import content to Cosmic


With your content exported and your Object Types defined, import using the :


```text


```


Run this in batches. Add a small delay () between inserts if you have hundreds of objects to avoid rate-limit spikes.


## Step 5: Update your frontend


With content in Cosmic, your React or Next.js frontend queries it via the SDK instead of Umbraco's Delivery API. The pattern is the same; only the client changes:


```text


```


## What Tripwire got out of it


Tripwire Interactive migrated from Umbraco to Cosmic when rebuilding their corporate site on React. The result, in the words of Owen Liversidge, Lead Developer:


> "Cosmic allowed us to easily integrate a secure and fast back-end API into our React app. Cosmic fit our needs with its simple web-based dashboard so that members of our marketing team can create, edit, and delete new content on the fly. Our team has been enjoying the ease of use with the new system."


The marketing team can now manage game landing pages, blog posts, career listings, and press releases without filing a ticket. New game launches get a dedicated landing page without developer involvement in the content layer.


## Common migration pitfalls


- *Rich text with embedded media.* Umbraco's TinyMCE rich text often contains inline image URLs pointing to your Umbraco media library. You need to migrate those media assets first, then do a find-and-replace on URLs in your exported content before import.
- *Nested content / Block List.* These map to Cosmic repeater or parent metafield groups, but the JSON structure is different. Budget extra time to write a transform function for complex nested structures.
- *URL redirects.* If your Umbraco site had clean URLs, make sure your new frontend preserves them or sets up 301 redirects. Google has indexed those URLs.
- *Editor training.* Cosmic's dashboard is simpler than Umbraco's backoffice but it is different. Budget 30 minutes of walkthrough with your marketing team.


## Is it worth it?


For teams rebuilding on a modern JavaScript frontend, the answer is almost always yes. The operational overhead of maintaining a .NET CMS alongside a JavaScript deployment pipeline compounds over time. Moving to a hosted headless CMS removes a category of infrastructure concern and gives your marketing team direct content ownership.


Tripwire's migration proves the pattern works at production scale for a real content-heavy gaming brand. The code is cleaner, the deploys are simpler, and the marketing team stopped filing tickets for content changes.


## Next steps


If you are evaluating the move, Cosmic's free tier is the fastest way to validate the content model before committing to a full migration. Create a bucket, define one or two Object Types mirroring your Umbraco Document Types, and run a small batch import to see how the data lands.


### Build AI-powered content workflows with Cosmic


Your content layer for AI agents. Structured, versioned, queryable, and analytics-ready out of the box.


[Start for free](https://app.cosmicjs.com/signup?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-signup-cta)[Book a demo](https://calendly.com/tonyspiro/cosmic-intro?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-demo)[Log in](https://app.cosmicjs.com/login?utm_source=cosmicjs.com&utm_medium=blog&utm_campaign=blog-content&utm_content=bottom-login)
