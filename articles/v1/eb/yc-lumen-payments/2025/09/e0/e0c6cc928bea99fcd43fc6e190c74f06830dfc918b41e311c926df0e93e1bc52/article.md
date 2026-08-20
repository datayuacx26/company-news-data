---
schema_version: "1.0.0"
document_id: "e0c6cc928bea99fcd43fc6e190c74f06830dfc918b41e311c926df0e93e1bc52"
company_key: "yc-lumen-payments"
company: "Lumen Payments"
source_id: "yc-lumen-payments-news-import-87c7fb1c30f2"
canonical_url: "https://getlumen.dev/blog/how-i-prototype-as-a-ycombinator-founder"
published_at: "2025-09-09T00:00:00+00:00"
first_seen_at: "2026-07-22T02:53:16.718446+00:00"
fetched_at: "2026-07-28T21:59:45.283870+00:00"
content_hash: "sha256:0c2c637b5c3c6761b78da784e323e37ad7d7afdaa58ab3fd9fd5b26f0a90a136"
---

# How I prototype as a Y-Combinator founder

TL;DR: I use Next.js (hosted in Vercel), Supabase and Lumen. The entire stack is free to use.


All the code is available in[Github](https://github.com/pretzelai/lumen-nextjs-starter)


A live version can be found at[https://lumen-nextjs-starter.vercel.app](https://lumen-nextjs-starter.vercel.app/)


## Why I chose this stack


**NextJS** : Easy to use and free to deploy in Vercel. AI knows it very well so it makes it easy to debug and build. I have used it extensively so it is the easiest to use for me. Trying Vite or other alternatives would only slow me down.


**Supabase** : generous free tier that is good enough for demos. Integrated auth makes it super easy to ship fast.


**Lumen** : out of the box credit system to limit AI usage per account. If you connect Stripe, you also get payments working without any extra effort.


## Supabase configuration


### Supabase per-user JSON store with RLS


This is probably the most controversial part. I just make each user have a big json storing all the user data so I can reapidly iterate. Most of the prototypes I have made never got out of the demo stage so I only add proper tables and optimizations after I get the first paying customer.


### Table & RLS


Create a table` public.user_data` with` data` as` jsonb` and` user_id` as` uuid` defaulting to` auth.uid()` . Enable RLS and add a policy that allows all operations only when` user_id = auth.uid()` .


sql


```text
-- Table
create table if not exists public.user_data (
user_id uuid not null default auth.uid(),
data jsonb not null,
primary key (user_id)
);


-- Row Level Security
alter table public.user_data enable row level security;


create policy "Users can manage their own user_data"
on public.user_data
for all
to authenticated
using (user_id = auth.uid())
with check (user_id = auth.uid());
```


### Supabase storage: per-user "files" bucket policy


Create a storage bucket named` files` and add a policy that restricts access to objects inside a folder named after the` user_id` . Save files to paths like` ${auth.uid()}/avatar.png` so only the owner can read and write them.


sql


```text
-- Create private bucket (no-op if it already exists)
insert into storage.buckets (id, name, public)
values ('files', 'files', false)
on conflict (id) do nothing;


-- Storage RLS policy (applied to storage.objects)
create policy "Users can manage files in their own folder"
on storage.objects
for all
to authenticated
using (((bucket_id = 'files'::text)
and ((select (auth.uid())::text as uid) = (storage.foldername(name))[1])))
with check (((bucket_id = 'files'::text)
and ((select (auth.uid())::text as uid) = (storage.foldername(name))[1])));
```


## Lumen configuration


Since many of my demos are AI based, I always want to put some limits per user to prevent abuse and track usage. I make a free account in[Lumen](https://getlumen.dev/login) and skip adding Stripe API keys for now (or you can add them if you want to validate that users want to pay already). Then I create a free plan and I add the credits I want to allow per month.


## Wrapping up


This is the easiest way I found to build MVPs fast. Althought we built Lumen to process payments, it also helps build credit systems for AI even before implementing payments (I wish I had Lumen when I built the demo that got us into Y-Combinator).
Please let me know what you think, I reply to all the people who reach out! Email me at[\[email protected\]](https://getlumen.dev/cdn-cgi/l/email-protection#9fedfef2f0f1dff8faebf3eaf2faf1b1fbfae9) or message me on[LinkedIn](https://www.linkedin.com/in/ramon-garate-4b379093/) or[Twitter](https://x.com/ramonverse)


#### Ramon Garate


Founder of Lumen
