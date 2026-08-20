---
schema_version: "1.0.0"
document_id: "cf0a24f61dbee0b7a17fe1ebdaf5e774c29f7539e98a5d4bbc36c30d9280426a"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-power-for-kiro"
published_at: "2025-12-03T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T22:25:05.780563+00:00"
content_hash: "sha256:9d9ee8567f7b14e0f0f4d4c7d8f9ae35520bdb31259fc9d623e52e94fb34a207"
---

# The new Supabase power for Kiro

Today we are announcing new Supabase powers for[Amazon's Kiro IDE](https://www.kiro.dev/) . With these powers, you can build full-stack applications faster by giving Kiro deep knowledge of your Supabase project, best practices for database migrations, edge functions, and security policies.


## What are Kiro powers?#


[Kiro powers](https://kiro.dev/blog/introducing-powers/) bundles MCP tools and steering files into a single install, giving your agent specialized knowledge without overwhelming it with context. Traditional MCP servers load all their tools into context immediately, overwhelming the agent. Powers load only when relevant.


When you install a Supabase power in Kiro, the AI assistant gets immediate access to:


-


Your Supabase database schema


-


Best practices for writing edge functions


-


Security guidelines for Row Level Security (RLS) policies


-


SQL syntax optimized for Postgres


## What you can do with the Supabase powers#


The Supabase powers connect Kiro to your Supabase projects and gives you one-click access to common development tasks. We're releasing two powers—one for developing on hosted projects, and one for developing with the local Supabase stack through Supabase CLI.


### Manage database schemas#


Kiro can read your database schema, suggest migrations, and help you structure tables following Postgres best practices. The powers uses Supabase's CLI and MCP server to understand your current schema and make informed suggestions about changes.


### Review edge functions#


Click a button and Kiro will review your edge functions for common issues, performance problems, and best practices. The powers includes specific guidance on how Supabase edge functions work, including the Deno runtime, environment variables, and local testing.


### Check security and performance#


The powers includes automations to check advisors that scan your local project for security issues and performance bottlenecks. Kiro can identify missing RLS policies, inefficient queries, and common security mistakes before you deploy.


### Write better SQL#


Kiro gets context-aware help for writing SQL that works well with Postgres. The powers include guidance on formatting queries, naming conventions, writing secure database functions, and structuring complex queries with CTEs. Kiro helps ensure your SQL follows Postgres standards and Supabase best practices.


## How it works#


Kiro powers use the Model Context Protocol (MCP) to connect AI assistants to external tools and knowledge. The Supabase power bundles:


-


**MCP server configuration** that connects to your hosted or local Supabase project


-


**Steering files** that tell Kiro how to use Supabase tools correctly


-


**Manual triggers** that give you one-click access to reviews and checks


-


**Dynamic context** that loads only when you're working on database, security, or edge function tasks


When you ask Kiro to help with a database migration or edge function, it automatically loads the right context without cluttering the conversation with unnecessary information. Kiro also unloads the context when the power is no longer needed.


## Get started#


To use the Supabase powers:


1.


Install[Kiro](https://kiro.dev/) and add the Supabase power for local or hosted development


2.


Ask Kiro to setup your Supabase project


3.


Start building


## Built on open standards#


Kiro powers use the same Supabase MCP Server used by other AI coding assistants, ensuring that your app development experience in Kiro remains familiar to you.


We're committed to delivering a great Supabase development experience across every tool. These Kiro powers gives developers instant access to best practices, security checks, and deep platform knowledge without leaving their editor.


Try the Supabase powers today and let us know what you build.
