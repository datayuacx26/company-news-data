---
schema_version: "1.0.0"
document_id: "100c8e2a0642b6914eae29a083a165931b657e532ef6e7171df5f5ceae57c98e"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-integrations-marketplace"
published_at: "2023-08-10T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:83666622c55bf6bd5f994c8734ff0b0e73a1b9d816e10af9d3b0cf56fb4255d1"
---

# Supabase Integrations Marketplace

We've been running our[Integrations Marketplace](https://supabase.com/partners) in “stealth mode” for about a year now. What started as a dog-fooding project has now transformed into a marketplace with[over 60 integrations](https://supabase.com/partners/integrations) . (It's also an[open source template](https://vercel.com/templates/next.js/supabase-partner-gallery) that you can use yourself).


Supabase Integrations allows Partners to extend the Supabase platform with useful tooling. Today we're adding[OAuth2.0 Applications](https://supabase.com/docs/guides/platform/oauth-apps/publish-an-oauth-app) . For Supabase users, this makes it even easier to connect their favorite tools to their Supabase projects. Within minutes you can:


- Add your favorite[Low Code](https://supabase.com/partners/integrations#low-code) tools on top of your Supabase database.
- Integrate your favorite[DevTools](https://supabase.com/partners/integrations#devtools) : including secrets managers and database management tools.
- Add[caching](https://supabase.com/partners/integrations#caching%20/%20offline-first) to your Supabase database.
- Not a fan of the Supabase admin dashboard? Try[one of these](https://supabase.com/partners/integrations#data%20platform) .
- Try out a different[SMS and email provider](https://supabase.com/partners/integrations#messaging) .


## Featured Partners#


For the initial launch we've started with a few partners to help us build and test the OAuth functionality.


### Cloudflare#


We worked with Cloudflare to build[support for databases](https://blog.cloudflare.com/announcing-database-integrations/) inside Cloudflare Workers. The Cloudflare integration makes it incredibly easy to connect to your Supabase database directly from the Cloudflare Dashboard.


Check out the[latest episode](https://cloudflare.tv/event/using-supabase-with-cloudflare-workers/dgM90RgD) on Cloudflare TV to see it in action.


### Resend#


[Resend](https://resend.com/) (YC[W23](https://www.ycombinator.com/companies/resend) ) is building the modern email sending platform. If you're using Supabase for Auth, then you'll know already that we handle all your Auth emails. But did you know that the email configuration we provide you is only for testing purposes? When you're[going into production](https://supabase.com/docs/guides/platform/going-into-prod#restricted-access-levels-for-team-members) , you need to integrate your own email provider. That's where Resend come in. They've built a one-click integration to add Resend as a custom SMTP provider for Supabase.


Read more on[Resend's blog](https://resend.com/blog/how-to-configure-supabase-to-send-emails-from-your-domain) .


### Snaplet#


Snaplet is a tool for Typescript developers to copy your database, transform sensitive data, and share it with your team without worrying about PII. If you followed our[Tuesday launch](https://supabase.com/blog/supabase-local-dev#database-seeding) you'll be familiar with Snaplet - they are one of the best tools for[generating seed data](https://supabase.com/docs/guides/cli/seeding-your-database#generating-seed-data) for your local development environment. Now they are making it even easier, with their official OAuth App, to spin up production-like development environments for your team.


[Learn more on snaplet.dev](https://www.snaplet.dev/post/now-live-supabase-x-snaplet-integration) .


### Trigger.dev#


[Trigger.dev](http://trigger.dev/) (YC[W23](https://www.ycombinator.com/companies/trigger-dev) ) is the open source Background Jobs framework for Next.js. You can create long-running Jobs directly in your codebase with features like API integrations, webhooks, scheduling and delays. And today you can use their one-click integration to[trigger anything from a database change](https://trigger.dev/supabase) in Supabase.


Learn more about their integration at:[trigger.dev/supabase](http://trigger.dev/supabase)


### Vercel#


One that requires no introduction - since so many of you use Vercel, we've dedicated an entire blog post to the upgraded Vercel integration.


Learn more about the Vercel integration[updates we're launching](https://supabase.com/blog/using-supabase-with-vercel) today.


### Windmill#


[Windmill](https://windmill.dev/) (YC[S22](https://www.ycombinator.com/companies/windmill) ) is an open source alternative to Retool and a modern Airflow. They provide a developer platform to quickly build production-grade complex workflows and integrations from minimal Python and Typescript scripts. Their one-click integration with Supabase makes it simple to launch new databases, process large quantities of data (maybe even convert them into[embeddings](https://supabase.com/modules/vector) ), and build internal dashboards.


Read the[official blog post on windmill.dev](https://www.windmill.dev/blog/2023/08/10/supabase-partnership) .


## Building Supabase Integrations#


We've released full instructions in our[Build with Supabase](https://supabase.com/docs/guides/platform/oauth-apps/build-a-supabase-integration) documentation so that you can build your own Supabase OAuth application for your users. Simply visit your[Organization settings](https://supabase.com/dashboard/org/_/apps) and click “Add application” to get started:


The Integrations marketplace is open to everyone. After your submission is complete, you can share the integration with your own users - simply create a button to launch your new app. We've provided some[brand assets](https://supabase.com/brand-assets) so that developers can quickly identify the integration on your site.


## Building custom integrations#


You don't actually need to build an OAuth Application to build an integration with Supabase. If you're building something for yourself or your team, the[Management API](https://supabase.com/docs/reference/api/introduction) is the way to go.


The[Trigger.dev](https://trigger.dev/) team deserve a special shout out. While developing their Integration they also developed[supabase-management-js](https://github.com/supabase-community/supabase-management-js) , a Typescript library for the[Supabase Management API](https://supabase.com/docs/reference/api/introduction) . This makes it even easier to get started with the Supabase API.


It's useful beyond just integrations. Want to programmatically spin up databases? Easy:


`
_13


import { SupabaseManagementAPI } from "supabase-management-js";


_13


_13


const client = new SupabaseManagementAPI({


_13


accessToken: "<access token>"


_13


})


_13


_13


const newProject = await client.createProject({


_13


name: 'staging',


_13


db_pass: 'XXX',


_13


organization_id: 'XXX'


_13


plan: 'free',


_13


region: 'us-east-1'


_13


})


`


## Become a Partner#


Supabase is a collaborative company. We love working with other communities (especially open source ones!), and we'd love to work with you. Get started today:


- [Build an OAuth integration](https://supabase.com/docs/guides/platform/oauth-apps/build-a-supabase-integration)
- [Learn more about our Management API](https://supabase.com/docs/reference/api/introduction)


## More Launch Week 8#


- [Supabase Local Dev: migrations, branching, and observability](https://supabase.com/blog/supabase-local-dev)
- [Hugging Face is now supported in Supabase](https://supabase.com/blog/hugging-face-supabase)
- [Launch Week 8](https://supabase.com/launch-week)
- [Coding the stars - an interactive constellation with Three.js and React Three Fiber](https://supabase.com/blog/interactive-constellation-threejs-react-three-fiber)
- [Why we'll stay remote](https://supabase.com/blog/why-supabase-remote)
- [Postgres Language Server](https://github.com/supabase/postgres_lsp)
