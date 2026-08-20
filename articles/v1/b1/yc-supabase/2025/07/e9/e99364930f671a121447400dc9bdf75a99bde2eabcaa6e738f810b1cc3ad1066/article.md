---
schema_version: "1.0.0"
document_id: "e99364930f671a121447400dc9bdf75a99bde2eabcaa6e738f810b1cc3ad1066"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/supabase-ui-platform-kit"
published_at: "2025-07-14T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:57:22.944976+00:00"
content_hash: "sha256:aef770c9761fd91f4b171cb6d93026a791596cd239de4f491aafb6910debdc64"
---

# Supabase UI: Platform Kit

Today we’re releasing our Platform Kit, new components in the Supabase UI Library that makes it incredibly easy to build platforms on top of Supabase.


## What’s Supabase UI?#


Earlier this year we released[Supabase UI Library](https://supabase.com/blog/supabase-ui-library) . This was designed mostly for building *apps* . Since we released it, we have had a strong pull - not from *app* builders, but from *platform* builders, such as[AI Builders](https://supabase.com/solutions/ai-builders) .


## What’s the Platform Kit?#


You can think of the Platform Kit as a bunch of UI components that sit on top of our[Management API](https://supabase.com/docs/reference/api/introduction) . Who is it for? Well, anyone who is providing Supabase projects to their users.


It includes the following components:


### Database components#


Display all the data in your database:


### Storage components#


### Authentication components#


### AI components#


## How does Supabase UI work?#


The library is 100% shadcn/ui compatible by leveraging the[component registry](https://x.com/shadcn/status/1829646556318089509) feature. Components are styled with shadcn/ui and Tailwind CSS and are completely customizable. Read the[original launch post](https://supabase.com/blog/supabase-ui-library) for more details, or check out the docs:[ui.supabase.com](https://supabase.com/ui)


## Supabase for Platforms#


While we started Supabase as Service directly for developers, more and more companies are building on top of Supabase to provide critical infrastructure for their own users. Today,[30% of Supabase sign ups](https://x.com/kiwicopple/status/1894251335224320000) can be attributed to some sort of AI Builder:


We’re in the process of adding more features so that you can use Supabase as a “Platform as a Service” for many other tools:


> ***Platform as a service** ( **PaaS** ) is a cloud computing model where users run a modular bundle of a computing platform and applications, without the complexity of building and maintaining the infrastructure associated with developing and launching applications.
>
>
> Source:[Wikipedia](https://en.wikipedia.org/wiki/Platform_as_a_service)


If you’re building your own plaform, reach out to the team:[supabase.com/solutions/ai-builders](https://supabase.com/solutions/ai-builders)


## Getting started with Platform UI#


Head on over to the docs to get started:[ui.supabase.com](https://supabase.com/ui/docs/platform/platform-kit)


You can also check out the[repo](https://github.com/supabase/supabase/tree/master/apps/ui-library/registry/default/platform) to download and run the app yourself to see how it works, or just pull key files and components to use in your own implementation. The code is yours to use and improve however you see fit.
