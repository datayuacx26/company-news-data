---
schema_version: "1.0.0"
document_id: "c6827da1bed9f29d394a13b18a28a47a40689d908e6724d5ec7ee1e7e3dcd12c"
company_key: "yc-cosmic-new"
company: "Cosmic"
source_id: "yc-cosmic-new-atom-eb157756d832"
canonical_url: "https://www.cosmicjs.com/blog/contentful-to-cosmic-migration"
published_at: "2026-06-08T00:00:00+00:00"
first_seen_at: "2026-07-27T08:40:33.238493+00:00"
fetched_at: "2026-07-28T21:11:40.706155+00:00"
content_hash: "sha256:f5ac643d1c0141817355a08116b04eeaba9758d95e638be5abaca7eb1339d7d1"
---

# How to Migrate from Contentful to Cosmic in 30 Minutes

Since Salesforce completed its acquisition of Contentful, teams across the industry have been re-evaluating their CMS stack. Pricing changes, roadmap uncertainty, and enterprise-first repositioning are pushing developers and content teams to look for a more focused alternative. If you've already decided to move on, this guide covers the practical how-to. For the "why," see our posts on[the best Contentful alternative](https://www.cosmicjs.com/blog/contentful-alternative) and[what the Salesforce acquisition means for your team](https://www.cosmicjs.com/blog/contentful-salesforce-acquisition-2026) .


This walkthrough takes roughly 30 minutes for a typical project. Larger spaces with thousands of entries or complex localization setups may take longer, but the steps are the same.


---


## What You'll Need


- Node.js 18+ installed
- A Contentful account with space access and a Management API token
- A Cosmic account (free plan works —[sign up here, no credit card required](https://app.cosmicjs.com/signup) )
- The package
- Basic familiarity with the command line


---


## Step 1: Export Your Content from Contentful


Contentful provides a first-party CLI that handles the full export to JSON. Install it globally:


```text


```


Authenticate with your Management API token:


```text


```


Then run the export:


```text


```


This produces a single file containing your , , , and . The flag pulls the actual media files to your local machine alongside the JSON. You'll need them in Step 4.


**What the export file looks like:**


```text


```


Keep this file. Every subsequent step reads from it.


---


## Step 2: Map Contentful Content Types to Cosmic Object Types


This is the most important step and the one that takes the most thought. The concepts map closely but are not identical.


Contentful Cosmic


Space Bucket


Content Type Object Type


Field Metafield


Entry Object


Asset Media (imgix CDN)


Environment Bucket (separate)


**Opening your export and reading content types:**


```text


```


**Field type mapping reference:**


Contentful Field Type Cosmic Metafield Type


Symbol (short text)


Text (long text)


RichText or


Integer / Number


Boolean


Date


Link (Asset)


Link (Entry)


Array of Links (Entries)


Array of Symbols


JSON


Color


Cosmic supports over 20 metafield types in total, including (for nested arrays of fields), (for grouped fields), and . If a Contentful field has no direct equivalent, the type is a reliable fallback.


**A key difference worth noting:** Cosmic requires no schema migrations. You define Object Types and their metafields once in the dashboard or via the SDK, and you can modify them at any time without downtime or a migration script. Fields are added or removed instantly.


---


## Step 3: Create Your Object Types in Cosmic


You can create Object Types in the Cosmic dashboard under **Bucket Settings > Object Types** , or programmatically using the . Here is a TypeScript script that reads your Contentful content types and creates the corresponding Cosmic Object Types:


```text


```


Verify in your Cosmic dashboard that each Object Type was created with the right metafields before moving to the next step.


---


## Step 4: Import Your Entries via the TypeScript SDK


With Object Types in place, you can now write entries into Cosmic. This script reads and creates a Cosmic Object for each entry:


```text


```


**A note on RichText fields:** Contentful RichText is stored as a deeply nested JSON document. You'll want to convert it to HTML or Markdown before storing it in Cosmic. The package handles this cleanly:


```text


```


```text


```


---


## Step 5: Migrate Assets to the imgix CDN


Cosmic serves all media through imgix, which means every asset gets automatic image optimization, resizing, and format conversion with zero configuration.


Upload assets to your Cosmic bucket using the . Fetch each Contentful asset as a buffer and upload it via :


```text


```


Save the file. You can use it to do a second pass over your imported Objects and update any metafield values to point to the new imgix URLs.


**imgix advantage:** Once assets are in Cosmic, you get URL-based transformations for free. For example:


```text


```


No additional CDN configuration required.


---


## Step 6: Set Up URL Redirects


If your Contentful-backed site had URLs tied to Contentful entry IDs or specific slug patterns, you'll want redirects in place before you flip DNS.


The exact approach depends on your frontend framework and hosting. Common options:


**Next.js ():**


```text


```


**Vercel ():**


```text


```


**Netlify ( file):**


```text


```


If you maintained the same slug structure in your Cosmic import (recommended), you may need zero redirects at all. Check your slug mapping from Step 4.


---


## Step 7: Validate with the Cosmic SDK


Before cutting over traffic, run a quick validation to confirm your content landed correctly.


```text


```


Cross-reference the object counts against your Contentful export file:


```text


```


If the counts match, you're ready to update your frontend's environment variables to point at your Cosmic bucket and go live.


---


## Realistic Time Estimate


Task Estimated Time


Install CLI + export from Contentful 5 minutes


Review export, map content types 5-10 minutes


Create Object Types via SDK 5 minutes


Import entries via SDK script 5-10 minutes


Upload assets via SDK 3-5 minutes


Set up redirects 2-5 minutes


Validate with SDK 5 minutes


**Total** **~25-40 minutes**


Larger spaces (10,000+ entries, complex localization, or many content types) should plan for a longer scripted run and a testing window. The migration logic is the same; it just takes more time to execute.


---


## Let Cosmic AI Agents Help


If you'd rather not write the migration scripts by hand, Cosmic AI Agents can help. From inside your Cosmic dashboard, you can prompt an agent to inspect your export file, generate a schema mapping, write the import scripts, and validate the results, all from a natural language interface.


This is especially useful for complex content models with nested relationships, multi-locale content, or large entry volumes where manual mapping would be tedious.


---


## You're Live on Cosmic


Once validation passes, update your frontend's environment variables:


```text


```


Then redeploy. Your content is now served from Cosmic's global CDN, with assets on imgix, and you have a free plan that includes unlimited API requests with no credit card required.


**Pricing starts at $0/month** (Free plan: 1 Bucket, 2 team members, 1,000 Objects). Paid plans start at $49/month (Builder) and scale to $499/month (Business, 50,000 Objects, 10 team members). Additional users are $29/user/month on any paid plan.


---


## Next Steps


- [Start for free on Cosmic](https://app.cosmicjs.com/signup) — no credit card required
- [Book a 30-minute migration walkthrough with Tony](https://calendly.com/tonyspiro/cosmic-intro)
- Browse the[Cosmic documentation](https://www.cosmicjs.com/docs) for SDK and API references
- Questions? Join the[Cosmic Discord community](https://discord.gg/MSCwQ7D6Mg)


---


## Related Reading


- [What the Salesforce Acquisition Means for Contentful Customers (2026)](https://www.cosmicjs.com/blog/contentful-salesforce-acquisition-2026) — Understanding the business context behind why teams are choosing to migrate from Contentful to Cosmic right now.
- [The Best Contentful Alternative in 2026](https://www.cosmicjs.com/blog/contentful-alternative) — A full breakdown of Cosmic as a Contentful alternative: pricing, API speed, AI agents, and developer experience.
- [How Enterprise Teams Are Replacing Contentful with an AI-Native CMS](https://www.cosmicjs.com/blog/enterprise-teams-replacing-contentful-ai-native-cms-2026) — Migration considerations and evaluation criteria for large organizations.
- [Cosmic AI Agents vs. Contentful Skills](https://www.cosmicjs.com/blog/cosmic-ai-agents-vs-contentful-skills) — What you gain on the AI side when you move to Cosmic.
