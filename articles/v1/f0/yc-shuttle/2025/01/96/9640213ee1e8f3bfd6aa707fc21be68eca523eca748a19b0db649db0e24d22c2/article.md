---
schema_version: "1.0.0"
document_id: "9640213ee1e8f3bfd6aa707fc21be68eca523eca748a19b0db649db0e24d22c2"
company_key: "yc-shuttle"
company: "Shuttle"
source_id: "yc-shuttle-rss-52efc69d7cac"
canonical_url: "https://www.shuttle.dev/blog/2025/01/17/shuttle-web3-weavevm"
published_at: "2025-01-17T15:00:00+00:00"
first_seen_at: "2026-07-20T23:24:10.103156+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:3f359c9781bbad1d433bc0cb2bb4d494f59909fc14778d4a1f60e13bfdfa77aa"
---

# Supporting Web3: How WeaveVM Ships Rust Microservices with Shuttle

## Introduction #


We're really excited by Web3.


There's great work being done by really smart people. The mainstream adoption of decentralized infrastructure is moving the world in a direction of transparency and fairness and censorship resistance.


Our Rustacean cousins in Web3 are spending time building robust protocols and reference implementations of validators which work and scale beautifully.


At the same time, the *auxilliary* infrastructure such as indexers, DA layers, etc. which are deployed off chain are a different story.


Speaking to multiple Web3 teams, we've discovered that running these services reliably is an orthogonal skillset. While important, this also distracts them from their core mission.


This is the story of how[WeaveVM](https://www.wvm.dev/) solved this painpoint with Shuttle and why a Rust-focused PaaS fits like a glove.


## About WeaveVM #


WeaveVM is building permanent data storage and high-throughput data availability for blockchain networks, settling 100k daily transactions and securing over $1.5B in total value across networks like Metis, Humanode, Avalanche and Dymension.


Integrated with a wide network of chains and data protocols, they needed auxilliary services deployed off-chain quickly and reliably - this is where Shuttle comes in.


## Before Shuttle #


The WeaveVM team started with Heroku. Over time this grew into more of a headache than they asked for. Unnecessarily complex deployment flows, dependency mismatches, SSL certs were absorbing their limited and valuable time from doing what actually mattered.


"We were spending precious dev time on infrastructure instead of building features that actually mattered," the team shared. "Then we found Shuttle, and it won on every single DevOps point."


## Enter Shuttle #


Moving to Shuttle was really quick for the WeaveVM team. Using Shuttle's annotation system on an existing service gave a migration time of less than 30 minutes (although your mileage may vary).


```text
#[shuttle_runtime::main]
async    fn    rocket  (
#[shuttle_shared_db::Postgres]   pool :    PgPool  ,
)    ->    shuttle_rocket ::   ShuttleRocket    {
let   state  =    MyState    {   pool  }  ;
let   rocket  =    rocket ::   build  (  )
.  mount  (  "/"  ,    routes!  [  hello ]  )
.  manage  (  state )  ;
Ok  (  rocket .  into  (  )  )
}


```


WeaveVM's CTO Rani and the team are Rust pros, so translating that expertise to infrastructure was natural and liberating. Databases, secrets management, SSL certificates - everything just worked out of the box. "Shuttle's nice DevX reduces the development and deployment time and lets us focus on the thing that matters: the code" Rani said.


## Looking at Today #


Today, WeaveVM's backend team ships Rust microservices - APIs, ETL pipelines, cron jobs without breaking a sweat. Their backend services, which have been migrated to Rust, scale seamlessly, while the team can quickly and easily spin up new services and experiment freely. When they need support, they get it directly through Josh and our team on Discord, a nice contrast to layers of impersonal support bots.


## Help us help you #


We're here to help! If what you've read sounds familiar sign up to Shuttle or reach out directly on[Discord](https://discord.gg/shuttle) or by email. Our team is excited to help you just deal with Rust and save you the headache of maintaining infrastructure.


Help us help you deliver Web3.
