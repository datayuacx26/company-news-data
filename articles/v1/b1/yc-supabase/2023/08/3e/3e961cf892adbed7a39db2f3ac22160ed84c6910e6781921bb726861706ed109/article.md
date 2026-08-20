---
schema_version: "1.0.0"
document_id: "3e961cf892adbed7a39db2f3ac22160ed84c6910e6781921bb726861706ed109"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-local-dev"
published_at: "2023-08-08T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:c0bea6d9123d6314067cd697d5e02b35396c3a3606d1d5c0cea47a882cf45034"
---

# Supabase Local Dev: migrations, branching, and observability

One of our goals at Supabase is to make Postgres development delightful. To do this, we need to simplify the experience between our CLI, your code editor, and the remote Postgres database powering your applications.


We received feedback recently about our local development experience, encouraging us to improve. This iteration introduces many new features to address that feedback. Let’s jump into a few of the features we’re launching today.


## Postgres Language Server#


One of the unique features of Supabase is the ability to access your Postgres database directly from a browser or mobile app through our[Serverless APIs](https://supabase.com/docs/guides/api) . This means that developers are writing more[PL/pgSQL](https://www.postgresql.org/docs/current/plpgsql.html) .


While code editors have great support for most programming languages, SQL support is underwhelming. We want to make Postgres as simple as Python. Our recently announced[Postgres Language Server](https://github.com/supabase/postgres_lsp) takes us a step in that direction - eventually it will provide first-class support for Postgres in your favorite code editor including Linting, Syntax Highlighting, Migrations Parsing, SQL Auto-complete, and Intellisense.


The Postgres Language Server is not ready for Production just yet. The majority of work is still ahead, but we've verified that the technical approach works and we're making it public now so that we can develop it in the open with input from the community. We’re already receiving amazing[feedback](https://github.com/supabase/postgres_lsp/discussions) and[support](https://news.ycombinator.com/item?id=37020610) .


Follow the progress of the[Postgres Language Server on GitHub](https://github.com/supabase/postgres_lsp) .


## Observability tools for Postgres#


We’ve added[debugging utilities](https://supabase.com/docs/guides/database/inspect) to our CLI to identify production issues via the` supabase inspect db` command. This interface includes 19 different commands to help you solve everything from slow queries to redundant indexes.


A lot of the credit for this belongs to[Heroku’s pg-extras](https://github.com/heroku/heroku-pg-extras) feature, an amazingly useful set of functionality. We’ve adapted the work they started, added a few additional commands, and made it available for any Postgres database. Simply append the` --db-url` param to use these commands with your own Postgres database.


This is just a starting point for the Supabase inspector. We’ll grow this feature to become an essential part of your Postgres toolkit.


## Easier backups#


We’ve made it even easier to backup and migrate your database, using` supabase db dump` . Under the hood, this simply uses[pg_dump](https://www.postgresql.org/docs/current/app-pgdump.html) (it's just Postgres, after all). However we also handle a few of the hairy issues that you might need to navigate on your own, like object permissions.


## Improved Database Migrations#


We’ve extended the CLI migration feature and added Dashboard support. Database migrations give you a way to update your database using version-controlled SQL files. We’ve built a lot of tooling around our migrations, including[reparation](https://supabase.com/docs/reference/cli/supabase-migration-repair) , migration cleanup using the[squash](https://supabase.com/docs/reference/cli/supabase-migration-squash) command, and[diffing](https://supabase.com/docs/reference/cli/supabase-db-diff) (using[migra](https://github.com/djrobstep/migra) ) to generate a new migration or to detect schema drift.


With the new Postgres Language Server, we hope to make it as easy to write Postgres migrations as it is to develop applications in TypeScript, Go, Python, or Rust.


Finally, we’ve added a Migrations view[in the dashboard](https://app.supabase.com/project/_/database/migrations) to track your migration history to improve the discoverability of migrations.


## Test and lint your database#


We’ve simplified the database testing experience, with` supabase test` . Running` supabase test new` stubs out a[pgTAP test](https://pgtap.org/) for you to fill with testing logic. The CLI includes[pg_prove and the TAP harness](https://pgtap.org/pg_prove) , so all you need to do is run` supabase test db` .


To make life even easier, our friends at[Basejump](https://usebasejump.com/) have created an entire suite of[Supabase Test Helpers](https://github.com/usebasejump/supabase-test-helpers) which make it simple to create users, run tests as an[authenticated user](https://usebasejump.com/blog/testing-on-supabase-with-pgtap#testing-authenticated) , and test your[RLS policies](https://usebasejump.com/blog/testing-on-supabase-with-pgtap#rls-testing) .


Finally, while you wait for us to make progress on the Language Server, we’ve added[support for linting](https://supabase.com/docs/guides/cli/testing-and-linting#linting-your-database) through the excellent[plpgsql_check](https://github.com/okbob/plpgsql_check) extension.


## Database seeding#


Seeding is the process of populating a database with initial data, typically used to provide sample or default records for testing and development purposes. This gives you a reproducible development environment across your entire team.


We’ve added[support for seeding](https://supabase.com/docs/guides/cli/seeding-your-database) to populate your local databases with data whenever you run` supabase start` or` supabase db reset` .


We’ve also worked with our friends at Snaplet to[generate seed data](https://supabase.com/docs/guides/cli/seeding-your-database#generating-seed-data) directly from your database:


`
_10


npx snaplet generate --sql > supabase/seed.sql


`


## Type generators#


Type generators introspect a PostgreSQL schema and automatically generate TypeScript definitions. This gives you[end-to-end type safety](https://www.youtube.com/watch?v=VSNgAIObBdw) from the database to the browser.


In the past month, we've added relationship detection in supabase-js. Foreign keys are now included in the generated types so that supabase-js can detect whether a referenced table should be an array (one-to-many) or an object (many-to-one). We've also added Helper Types to improve the developer experience for common scenarios, like short-hand accessors:


`
_10


// Before


_10


let movie: Database\['public'\]\['Tables'\]\['movies'\]\['Row'\] = // ...


_10


_10


// After


_10


let movie: Tables<'movies'> = // ...


`


## Official GitHub Action#


We’ve developed an official[GitHub Action](https://github.com/marketplace/actions/supabase-cli-action) which leverages the CLI. You can[generate types on every PR](https://supabase.com/docs/guides/cli/github-action/generating-types) , or run[your tests on every commit](https://supabase.com/docs/guides/cli/github-action/testing) .


## Local Logging and Debugging#


Logs are now accessible locally in the Dashboard. Last launch week we released an open source logging server, with support for BigQuery. In the past few months we’ve added Postgres support to this server. This means that all of your local logs are accessible with no additional config - simply run supabase start and then visit the local dashboard to start debugging.


## Stable releases#


We’ve moved the Supabase CLI to a fortnightly stable-release cycle. Every 2 weeks, we will update the[latest](https://www.npmjs.com/package/supabase?activeTab=versions) tag on npm, the[supabase/tap](https://github.com/supabase/homebrew-tap/blob/main/supabase.rb) for homebrew, and the[supabase](https://github.com/supabase/homebrew-tap/blob/main/supabase.rb) scoop bucket. You can find the binary downloads in our GitHub[latest release](https://github.com/supabase/cli/releases/latest) .


For the adventurous feature hunters, we’ve added a` beta` release channel for the CLI, with new releases on every PR merged. You can follow[this guide](https://github.com/supabase/cli#install-the-cli) to install Supabase CLI (beta).


## Branching and Preview Environments#


And finally, probably our most anticipated feature - branching:


We’ve made major improvements to our local development with the features above - but we have bigger ambitions. For several months we’ve been developing Supabase branching and today we're opening it up for alpha testers.


Supabase isn’t simply a database, it’s an entire backend - everything from your Postgres database to your[50GB videos](https://supabase.com/blog/storage-v3-resumable-uploads) . Branching improves the experience of managing environments so that developers and teams spend less time on DevOps and more time building.


### Supabase branching is hard#


Every project is a Postgres database, wrapped in a suite of tools like[Auth](https://supabase.com/auth) ,[Storage](https://supabase.com/storage) ,[Edge Functions](https://supabase.com/edge-functions) ,[Realtime](https://supabase.com/realtime) and[Vectors](https://supabase.com/modules/vector) , and encompassed by[API middleware](https://supabase.com/docs/guides/api) and[logs](https://supabase.com/blog/supabase-logs-self-hosted) .


A good branching solution requires each tool to provide multi-tenancy support so that:


1. Data can be isolated from production for security.
2. Compute can be isolated from each other to avoid noisy-neighbors.


### How does branching work?#


We use Git to bridge the gap between your local development environment and your hosted database. For now, we’ve focused on GitHub.


Every time you open a new Pull Request on GitHub, a corresponding “Preview Environment” is created. Each preview branch is an isolated Firecracker instance that pauses automatically after a period of inactivity. Every time a change is pushed to GitHub, the migrations within the` ./supabase/migrations` folder are run against the Preview Branch so that your entire team is working from the same source of truth.


When you hit merge on your Pull Request we run the migrations on your Production database.


### What about data?#


We’re starting with seed data. Any SQL with` ./supabase/seed.sql` will populate your Preview Branch. This provides your entire team with an isolated and reproducible environment, safe from any data privacy concerns.


Importantly, we *aren’t* cloning production data until we find something appropriate for data security. We know that copy-on-write is an available option, and with the appropriate anonymization techniques it seems like a promising way to provide a “production-like” test environment.


We’ll also need to figure out what this means for large files in Supabase Storage. Do you need to anonymize your photos and videos? This is a work in progress and we’re open to feedback.


Either way, we want to support *both* seed data and anonymized production data, so that teams can choose their preference based on their risk profile. It makes sense to start with a seed.


### Is it available yet?#


Branching update 12/13/2023


We are[rolling out access](https://supabase.com/blog/supabase-branching) and we'll be onboarding organizations in batches over the next few weeks. You can still[sign up for access](https://forms.supabase.com/branching-request) .


## Get started#


Jump into our updated[Local Development documentation](https://supabase.com/docs/guides/cli) to get started with the CLI.


If you’re an existing user simply[update your CLI](https://supabase.com/docs/guides/cli/getting-started#updating-the-supabase-cli) and check out the[full command reference](https://supabase.com/docs/reference/cli/introduction) for all the latest commands.


## More Launch Week 8#


- [Hugging Face is now supported in Supabase](https://supabase.com/blog/hugging-face-supabase)
- [Launch Week 8](https://supabase.com/launch-week)
- [Coding the stars - an interactive constellation with Three.js and React Three Fiber](https://supabase.com/blog/interactive-constellation-threejs-react-three-fiber)
- [Why we'll stay remote](https://supabase.com/blog/why-supabase-remote)
- [Postgres Language Server](https://github.com/supabase/postgres_lsp)
