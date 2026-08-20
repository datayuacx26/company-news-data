---
schema_version: "1.0.0"
document_id: "e7accbeb71c481fdc321ba46fe814e93a3fb778767c9ecb0ccd323e932470b5f"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/technical-dive-openclaw-hardened-1-click-app"
published_at: "2026-01-30T18:36:29.934+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T22:22:53.892181+00:00"
content_hash: "sha256:92028db92e3e93eab0357a6b30d9ea8af5b11c4f70c36757b662269456ae0627"
---

# Technical Deep Dive: How we Created a Security-hardened 1-Click Deploy OpenClaw

[OpenClaw, an open source AI assistant](https://www.digitalocean.com/resources/articles/what-is-moltbot) (recently[renamed from Moltbot](https://x.com/openclaw/status/2017103710959075434) , and earlier Clawdbot), has exploded in popularity over the last few days, and at DigitalOcean we immediately wondered “how can we enable more people to try this new technology safely and easily?” We noticed that there was a lot of interest by folks looking to use this software, but also that there was concern around the security of the open source software, especially when connecting it directly to users’ own machines. We dug in to find a way to deliver this software to our customers as fast as possible, as easily as possible and as safe as possible.


At DigitalOcean, our[1-Click Deploy OpenClaw](https://marketplace.digitalocean.com/apps/moltbot) (formerly 1-Click Deploy Moltbot) through our Marketplace enables us to package the latest and greatest software configured for our Droplet® server, and make it easily available to customers. Creating our 1-Click Deploy OpenClaw was the natural next step in getting this to our customers.


Toying around with OpenClaw on a local machine is fun, but it could severely impact the ability to deploy and use the software for longer term use and may not meet the safe environment that you need. Some of the benefits to deploying on DigitalOcean include:


- Always available – the service is available to customers via the web
- Easy to connect to it – Droplets have a static ip address
- Vertical scalability – scale up CPUs, memory, and disk storage with higher workloads
- Cognitive overload – start with basic configs, tweak the ones that matter to you


We made a lot of changes as we built the 1-Click Deploy OpenClaw, but the main elements we focused on were


- How do we communicate with the service safely?
- How do we keep the agentic code isolated from the rest of the system?
- How do we prevent attacks from the wider internet?


All of that while providing a straightforward deployment UX to our customers! Let’s dig in…


## Delivering an Image with Safe Defaults


Our priority in creating a 1-Click Deploy OpenClaw on our Droplet was twofold: First, speed, as we wanted to get something out quickly to our users. Second was providing a solution that provided additional security benefits. These are the actions we took to meet those goals:


### Keeping deployments consistent (DevOps)


We saw that there are multiple ways to deploy the software – we chose the most consistent path, which was picking a stable release from the Git repository on GitHub, pulling it and building from there.


Why not pull the latest and greatest from main? Changes are happening at a rapid pace, which is awesome for feature development but can come at the expense of stability. Depending on the minute we build our 1-click image, we could get a working version or a broken version.


So we make sure that we can deliver the latest *stable* version.


### TLS (Keep communications safe and auditable)


Once we had a Packer image that we could iterate on, we applied our security best practices for the 1-clicks to set up TLS. This is a crucial step to make sure that our customers can communicate with the bot in a safe way that doesn’t allow eavesdropping.


Our best practices consist of using Caddy as a reverse proxy with a TLS certificate issued by LetsEncrypt. Caddy ensures that the service is deployed externally is the service we want to publish and provides a safe channel with which to serve it. Furthermore, Caddy outputs logs to a location that can be audited after the fact, allowing the end user to see how their service is actually being used.


A new UX improvement we added to this image is seamless TLS configuration with LetsEncrypt via IP addresses without requiring a domain name! While OpenClaw spins up, Caddy is requesting a new certificate on your behalf, no configuration required.


### Authz (Gateway Key + Pairing)


How do we know that the requests are coming from you? We have a OpenClaw gateway key in place to make sure that the user who is supposed to use the platform is the correct one.


Next, we leaned into a feature that OpenClaw provides called “Paring” – this exists to make sure that the devices that are going to communicate with the main server are the trusted ones.


### Sandboxing (keep safe from Agents)


Part of the configuration is an Anthropic / OpenAI / Model key – these are sensitive pieces of material that are required in order to allow the software to function! So how do we let agents that can run arbitrary code on the machine, not read and abuse these tokens?


Furthermore, how do we stop the agents from potentially destroying the machine itself?


Luckily, there is a configuration available that puts the agent deployments into their own containers. If an agent blows up, it will destroy its own ephemeral docker container, but the host filesystem will still be safe from incorrect agentic modifications.


### Safe Defaults


These boxes are taking the best configurations that we implement for all of our 1-clicks, including but not limited to:


Fail2ban – Make sure that the background noise of the internet doesn’t cause disruptions to your droplet. It does this by monitoring the logs of failed requests to the system and dynamically updating firewall rules to block known bad patterns on the internet.


Unattended upgrades – we want to make sure that your Droplet is always up to date. We have Ubuntu configured with unattended upgrades that periodically will check for vulnerable packages and automatically patch them.


### Deployment Constraints and Upcoming Features


To ensure a stable and repeatable installation, we utilize Packer for our image provisioning; however, during testing, we found that smaller Droplet configurations consistently encountered out-of-memory errors during the snapshot creation process. While this currently necessitates a minimum $24/month Droplet size to match the snapshot’s disk and memory requirements, we chose to prioritize getting this tool into your hands today rather than delaying for further optimization. We are already iterating on the image to reduce its footprint and support lower-cost tiers, and in the spirit of transparency, we have[made our Packer scripts public](https://github.com/digitalocean/droplet-1-clicks) so you can audit the provisioning process and gain confidence in the one-click experience. We are also working to quickly add support for all DigitalOcean Gradient AI models, including OpenAI, add auto provisioning of Gradient AI API Key and injecting for the user, and more updates as OpenClaw evolves over time.


### After deploy (make it yours!)


1-Click Deploy OpenClaw is a great launch point, but OpenClaw is infinitely customizable once up and running in the Droplet. Choose which messaging platforms are the best fit for your workflows, and get chatting.


## Get started with the 1-Click Deploy OpenClaw


Get started with the[1-Click Deploy OpenClaw by visiting the Marketplace](https://marketplace.digitalocean.com/apps/moltbot) , and[follow this tutorial](https://www.digitalocean.com/community/tutorials/how-to-run-moltbot) for step by step instructions on how to get started.
