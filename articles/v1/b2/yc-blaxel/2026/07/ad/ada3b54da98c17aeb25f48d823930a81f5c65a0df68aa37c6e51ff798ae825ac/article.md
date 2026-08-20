---
schema_version: "1.0.0"
document_id: "ada3b54da98c17aeb25f48d823930a81f5c65a0df68aa37c6e51ff798ae825ac"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/hackersquad-streamlines-developer-time-to-value-with-blaxel-sandboxes"
published_at: "2026-07-09T13:11:24+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:f83819a40eeaf5ae891f651d9748e79cdcf376f8abaf8e757bc96b66df204890"
---

# HackerSquad streamlines developer time-to-value with Blaxel sandboxes

[HackerSquad](https://hackersquad.io/) is a developer marketing tool designed to host hackathons, technical workshops, and developer events while preserving the generated content and code in perpetuity. Founded by Adam Chan, who also runs the Builders Collective community of ~30,000 members, HackerSquad provides a digital environment combining live streaming, a cloud IDE, and a leaderboard, so companies can measure real developer engagement.


At Blaxel, we provide a[perpetual sandbox](https://docs.blaxel.ai/Sandboxes/Overview) platform for secure and scalable compute. While our platform is optimized for the continuous execution loops of autonomous AI agents, its underlying microVM architecture provides the foundational layer required for transient developer workloads. HackerSquad uses Blaxel sandboxes to power its Cloud IDE, adapting our agent-native architecture to serve human developers instantly during live events.


> "The sandbox deployment is really fast. We were able to get a database running and loaded with a couple gigabytes of embeddings in less than a minute." - Adam Chan, Founder, HackerSquad


## Local environment configuration bottlenecks developer events


Technical workshops and hackathons frequently suffer from long configuration delays. When a company hosts a technical workshop, attendees must typically download Docker containers, install local system dependencies, configure environmental variables, and clone repositories across varying operating systems.


This setup process often consumes one to two hours of a finite event window before participants can interact with the core technology. Furthermore, downloading large datasets and database images simultaneously over local event Wi-Fi strains network bandwidth and introduces intermittent failures.


HackerSquad previously used traditional virtual machines to mitigate these local setup challenges, but this architecture was not ideal. Most of the time, the team either had to provision a separate application instance for every user to avoid shared resource conflicts, or assign unique public IPv4 addresses to each container at an inflated operational cost. Managing custom proxies to expose specific application ports to users added complexity to the process.


> "Before Blaxel, it was either one app across all users, in which case everyone had shared resources and I couldn't define IP addresses per user, or one user per app, which meant that every app needed to get its own IPv4 address, which increased cost." - Adam Chan, Founder, HackerSquad


## Blaxel sandboxes enable instant development environments


HackerSquad resolved these setup bottlenecks by moving its infrastructure to Blaxel sandboxes, leveraging an architecture explicitly engineered for autonomous code execution to serve human developers instantly. The same microVMs designed for agent loops translate perfectly into[on-demand, ephemeral environments](https://docs.blaxel.ai/Sandboxes/Overview#sandbox-lifecycle) for workshop participants.


Now, when an event participant clicks to deploy a workshop environment, the HackerSquad platform programmatically triggers a Blaxel sandbox. The Blaxel sandbox provides a custom development environment built from the company's Git repository, with all services (such as a database instance) and runtime binaries already baked in.


This cloud-based architecture isolates the execution layer entirely from the attendee's local machine and event Wi-Fi constraints. Instead of spending hours troubleshooting installation failures, developers access a prepared codelab in under 30 seconds. The platform handles the underlying configuration, allowing attendees to focus immediately on core technical concepts.


> "We're just trying to enable developers to build with new products faster, and we're able to do that with Blaxel sandboxes powering the HackerSquad Cloud IDEs." - Adam Chan, Founder, HackerSquad


## Blaxel's real-time previews simplify operational access


Blaxel provides HackerSquad with granular and programmatic access to compute infrastructure, streamlining image customization and session lifecycle management. By leveraging Blaxel's[preview URLs](https://docs.blaxel.ai/Sandboxes/Preview-url) , HackerSquad was able to simplify end-user access to hackathon environments, without needing custom network orchestration logic or a custom proxy layer.


During a typical workshop, HackerSquad smoothly provisions between 40 and 100 isolated sandboxes concurrently, enabling the team to support large enterprise events without performance degradation.


Workshop requirement Blaxel sandbox solution


Provisioning time Near-instant sandbox creation and resume


Network routing Real-time preview URLs


Resource isolation Isolated micro-VM environments with dedicated memory


Infrastructure scaling Automated concurrent scaling


> "Blaxel sandboxes make the process of setting up a workshop and getting people running way faster than alternative solutions. Teams have more confidence running these workshops and can scale to hundreds of people now." - Adam Chan, Founder, HackerSquad


## Expanding developer engagement with scalable infrastructure


By offloading infrastructure management to Blaxel, HackerSquad avoids the necessity of maintaining complex virtual machine clusters or networking layers directly. The engineering team can focus instead on refining event analytics, video ingestion pipelines, and interactive features.


> "I think I have better programmatic access to Blaxel as a platform, which makes it easier to customize things. From that standpoint, the developer experience is much smoother for me and the reliability of Blaxel enables Developer Relations teams to run workshops with hundreds of developers in parallel - Adam Chan, Founder, HackerSquad


Like HackerSquad, you can start building agent-native applications today on[https://app.blaxel.ai](https://app.blaxel.ai/) , or use our Python SDK to launch your first sandbox:


python


` import


asyncio


from


blaxel


.


core


import


SandboxInstance


async


def


main


(


)


:


# Create a new sandbox


sandbox


=


await


SandboxInstance


.


create


(


{


"name"


:


"my-sandbox"


,


"image"


:


"blaxel/base-image:latest"


,


# public or custom image


"memory"


:


4096


,


# in MB


"ports"


:


\[


{


"target"


:


3000


}


\]


,


# optional; ports to expose


"region"


:


"us-pdx-1"


# optional; if not specified, Blaxel will choose a default region


}


)


# Create public preview


preview


=


await


sandbox


.


previews


.


create


(


{


"metadata"


:


{


"name"


:


"my-preview"


}


,


"spec"


:


{


"port"


:


3000


,


"public"


:


True


,


"responseHeaders"


:


{


"Access-Control-Allow-Origin"


:


"https://example.com"


,


"Access-Control-Allow-Methods"


:


"GET, POST, PUT, DELETE, OPTIONS, PATCH"


,


"Access-Control-Allow-Headers"


:


"Content-Type, Authorization, X-Requested-With, X-Blaxel-Workspace, X-Blaxel-Preview-Token, X-Blaxel-Authorization"


,


"Access-Control-Allow-Credentials"


:


"true"


,


"Access-Control-Expose-Headers"


:


"Content-Length, X-Request-Id"


,


"Access-Control-Max-Age"


:


"86400"


,


"Vary"


:


"Origin"


}


}


}


)


# Get preview URL


url


=


preview


.


spec


.


url


if


__name__


==


"__main__"


:


asyncio


.


run


(


main


(


)


)


`
