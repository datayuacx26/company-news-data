---
schema_version: "1.0.0"
document_id: "f1743f53ac7b250b5adbbae45668712ae8b8975c130857957ae05bf5464c5ab3"
company_key: "yc-howdy-com"
company: "Howdy.com"
source_id: "yc-howdy-com-news-import-7de6c48bfb23"
canonical_url: "https://www.howdylatam.com/blog/docker-compose-local-vs-production"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-28T17:58:10.597501+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:9a5deae43be581b6486ed6caba24c72c4e5db84117392cd51ebb17d584ba6ae8"
---

# Docker Compose Isn't the Problem. Your Local-to-Prod Assumptions Are.

You run docker compose up, everything comes online in twelve seconds, and the feature you've been building all week finally talks to Postgres, Redis, and that internal API mock without a single manual step. Then it ships. Three days later you're on a call at 11pm because the service that worked flawlessly on your laptop is throwing connection timeouts in production, and nobody can explain why the thing that "definitely works" doesn't.


This is not a Docker Compose problem. Compose did exactly what it was built to do. The problem is that somewhere between your laptop and the cluster, the team started treating compose.yml as if it were a production specification instead of what it actually is: a really good local development tool.


## **What Docker Compose Actually Gets Right**


Before getting into where this goes wrong, it's worth being honest about how good Compose is at its actual job. Spinning up a multi-service environment used to mean a README with fifteen steps, half of which were out of date. Now it's one file and one command. New hire joins the team, clones the repo, runs docker compose up, and by lunch they have the full stack running locally, matching what everyone else on the team has.


That consistency is real value. Compose kills "works on my machine" at the individual level. Every engineer gets the same Postgres version, the same environment variables, the same service topology, without anyone hand-configuring anything. For local iteration, for onboarding, for running integration tests against real dependencies instead of mocks, it's genuinely one of the better tools in the ecosystem. None of what follows is an argument against using it that way.


## **Where compose.yml Quietly Becomes a Production Contract**


The trouble starts with a very reasonable-sounding idea: "we already have all our services defined here, why not just use this same file to deploy?" Someone adds a few environment overrides, points it at a VPS or a single Docker host, and it works. It keeps working, for a while, so it stays.


What actually happened is that a file designed to answer "how do I run this on my laptop" got promoted to answer "how do we run this in front of customers" without anyone deciding that on purpose. Nobody sat down and said "let's skip proper orchestration." It just accumulated, one deploy at a time, until the docker compose file was the closest thing the team had to an infrastructure spec, and by then nobody wanted to be the one to say it wasn't built for that.


This is the actual failure mode. Not that Compose was used. That it was used past the boundary of what it was ever designed to handle, and the team didn't notice the boundary had been crossed until something broke.


## **The Drift Between "It Runs" and "It's Actually in Production"**


Here's where the gap shows up, concretely, because it's rarely the code. It's everything Compose abstracts away for the sake of a smooth local experience.


Networking is the first one. Compose gives every service a flat network where anything can talk to anything else by service name, no questions asked. Production, if it's actually production, has network policies, security groups, service meshes, or at minimum a load balancer sitting between things that were never meant to be directly reachable. The service that resolved api:8080 instantly on your machine hits a wall of DNS and firewall rules it never had to think about before.


Resource limits are the second one. Your local mem_limit and cpus settings, if you even bothered to set them, are guesses made on a machine with more headroom than any single production node will have. Kubernetes or ECS enforce real limits, with real consequences: OOM kills, throttling, pods getting evicted because they asked for more than the scheduler could give them. A container that idled happily at 200MB locally can behave completely differently under actual load with actual memory pressure.


Secrets are the third, and the most dangerous one, because it's the one most likely to look fine until it isn't. Local Compose setups load secrets from a .env file sitting right there in the repo, or hardcoded straight into the compose file for convenience. That's fine on a laptop nobody else has access to. It's a different situation entirely when that same pattern gets copied into a deploy script pointed at a real server, because now credentials are sitting in plaintext somewhere they were never meant to persist.


And service discovery is the fourth. Compose's DNS-by-service-name is elegant and it's also nothing like how a real orchestrator finds and routes to healthy instances across multiple nodes, with health checks, rolling restarts, and instances that come and go. The moment you have more than one node, or more than one replica of a service, the assumptions baked into a Compose network stop applying, and nothing in the file tells you that.


None of this is a bug in Docker Compose. It's a mismatch between what the tool promises (a working local environment) and what the team started expecting from it (a production-equivalent runtime).


## **Docker Compose Was Never Designed to Orchestrate Anything**


It's worth saying plainly: Compose has no concept of a cluster. It doesn't reschedule a failed container onto another node, because in its model there is no other node. It doesn't do rolling deploys, it doesn't manage multi-host networking, it has no built-in secrets manager, and it was never trying to. Comparing it to Kubernetes or ECS isn't really a fair fight, because they're solving different problems. One answers "how do I run this stack right now, on this machine, so I can build against it." The other answers "how do I keep this running, correctly, across failure, scale, and time, on infrastructure I don't fully control."


Calling compose.yml infrastructure-as-code is where the confusion starts. Infra-as-code implies the file is the source of truth for how the system runs in the environment that matters to users. Compose was never trying to be that. It's a developer experience tool, and a very good one, that happens to use a syntax that looks close enough to a Kubernetes manifest that teams under deadline pressure convince themselves the leap is smaller than it is.


## **What Actually Closes the Gap**


None of this means throwing out Compose or feeling bad about using it. It means being explicit about what it's for. Keep it as the fast, disposable, developer-facing tool it's good at being, and treat the actual deploy target as a separate concern with its own manifest, whether that's Kubernetes YAML, an ECS task definition, or a Nomad job spec, written for the orchestrator you're actually running on.


The teams that avoid the 11pm incident aren't the ones who found a clever way to reuse compose.yml in production. They're the ones who accepted early that local and production are different environments solving different problems, and built two separate, honestly-named things instead of one file quietly doing double duty. Docker Compose images can still be the same images that get promoted through a registry, environment variables can still be defined once and templated out, but the orchestration layer itself, the networking model, the resource enforcement, the secrets handling, needs its own definition, reviewed with the same seriousness as the application code.


A staging environment that actually mirrors production, same orchestrator, same network topology, same resource ceilings, is what catches the gap before a customer does. It doesn't need to be expensive. It needs to be honest about running on the real thing instead of a local approximation of it. If your staging environment is also just docker compose up on a bigger server, you haven't closed the gap, you've just moved it one hop further from your laptop.


There's also a simpler tell worth watching for. If nobody on the team can point to the actual production manifest without also pointing at compose.yml, that's not a documentation gap. That's the drift already in motion, and it's worth flagging in a standup before it turns into an incident report. The fix costs an afternoon of writing a real manifest. The alternative costs a night of debugging in production while customers watch.


## **The Real Position Here**


Docker Compose is not a lightweight version of production infrastructure. It's a different category of tool entirely, and the moment a team starts treating it as one is the moment silent drift starts accumulating between what engineers see locally and what actually runs behind a domain name. The fix isn't a better compose file. It's admitting that a developer experience tool and an infrastructure specification are not the same artifact, no matter how similar the YAML looks, and refusing to let deadline pressure blur that line until a production incident draws it back for you.
