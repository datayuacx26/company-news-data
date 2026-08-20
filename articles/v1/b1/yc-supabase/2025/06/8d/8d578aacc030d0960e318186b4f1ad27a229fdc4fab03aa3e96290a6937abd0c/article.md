---
schema_version: "1.0.0"
document_id: "8d578aacc030d0960e318186b4f1ad27a229fdc4fab03aa3e96290a6937abd0c"
company_key: "yc-supabase"
company: "Supabase"
source_id: "yc-supabase-rss-47281c9e7110"
canonical_url: "https://supabase.com/blog/multigres-vitess-for-postgres"
published_at: "2025-06-10T07:00:00+00:00"
first_seen_at: "2026-07-20T23:24:12.344578+00:00"
fetched_at: "2026-07-28T20:57:27.560850+00:00"
content_hash: "sha256:39d47f9e8854465509ac0d720c7a23b1d9a29400fd5bb9ee045c916e62e1995d"
---

# Announcing Multigres: Vitess for Postgres

Today we’re welcoming[Sugu](https://www.linkedin.com/in/sougou/) , the co-creator of Vitess, to the Supabase team. He is joining Supabase to build **Multigres: Vitess for Postgres** .


## Background: what is Vitess?#


Vitess is a database clustering system for scaling **MySQL** (it doesn’t work for Postgres). Vitess adds:


- Sharding
- Connection pooling
- Query routing
- Resiliency and failover
- Cloud-native orchestration (Kubernetes)


Sugu originally built Vitess at YouTube. This (outdated) video explains some of the concepts and motivation:


* The video mentions 3-5ms latency. This was for hard disks. SSDs now provide sub-millisecond latency.


## What is Multigres#


Multigres is a new proxy that sits in front of your Postgres database. It shares the same goals as Vitess but it will be focused on the Postgres ecosystem. The ultimate goal of a sharded solution is scale:


Sharding inevitably requires tradeoffs, so one of the top priorities for this project is **compatibility:** something that the Postgres community values above almost everything else.


We plan to give developers a gradual on-ramp, providing simple connection pooling on the lower-end, high-availability on the top end, all the way through to a sharded solution as you grow into petabyte-scale.


## Why Multigres for Postgres?#


Sugu[shared](https://x.com/ssougou/status/1932445665512284645) some of his personal thoughts on the project and timing:


> For some time, I've been considering a Vitess adaptation for Postgres, and this feeling had been gradually intensifying. The recent explosion in the popularity of Postgres has fueled this into a full-blown obsession. As these databases grow, users are going to face a hard limit once they max out the biggest available machine.
>
>
> The project to address this problem must begin now, and I'm convinced that Vitess provides the most promising foundation. After exploring various environments, I found the best fit with Supabase. I'm grateful for how they welcomed me. Furthermore, their open-source mindset and fully remote work culture resonated with me.
>
>
> Now, it's time to make this happen.
>
>
> Sugu, co-creator of Vitess.


## Compatibility with Supabase#


There will be no fundamental changes to the way Supabase operates today, especially for smaller workloads.


That said, there are some overlapping initiatives at Supabase and we will assess how to build the same experience with Multigres. The largest overlap is with[Supavisor](https://supabase.com/blog/supavisor-1-million) (Vitess has vttablet and vtgate). We’ll spend some time assessing the viability of building Vitess around existing Postgres poolers and then decide the best steps forward.


Another important initiative at Supabase is OrioleDB, which is a scalable storage engine for Postgres. OrioleDB and Multigres are complementary and we will work on both simulateneously. Both are extremely important to us.


And importantly, one non-goal: Supabase will be 100% focused on Postgres. If you are using MySQL then we recommend that you use Vitess.


## Open Source#


Like Vitess, Multigres will be open source using the same license: Apache 2. You can follow the repo[here](https://github.com/multigres/multigres) .


Remember to “Watch” the repo to follow our releases. Note: we will initially be focused on getting the project to a stable state and then we will open it up to contributions.


## Design partners and early adopters#


We are looking for partners who would like to use Multigres - either with self-hosting or on the Supabase platform. If you already use Postgres and you are hitting scaling limits,[get in touch](https://forms.supabase.com/multigres) .


## Join us to build Multigres#


We are assembling a team to build Multigres. If you are a Go programmer consider applying[here](https://jobs.ashbyhq.com/supabase/2e718684-4f75-4a99-8d6b-3b6bd44e4228) .
