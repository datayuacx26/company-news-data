---
schema_version: "1.0.0"
document_id: "ad177f3679e9bfab01ba181895175f30461613fbb9ea74250397867075430c2a"
company_key: "digitalocean-holdings-inc-common-stock"
company: "DigitalOcean Holdings Inc."
source_id: "digitalocean-holdings-inc-common-stock-atom-50ed4adbc240"
canonical_url: "https://www.digitalocean.com/blog/run-codex-in-the-cloud"
published_at: "2026-06-25T21:16:06.199+00:00"
first_seen_at: "2026-07-20T03:30:06.260557+00:00"
fetched_at: "2026-07-28T21:10:03.278263+00:00"
content_hash: "sha256:6d25d2b97c89ddac8d853d4c5705de3b372ad5d2c2a34915daccf1b00d2aa02f"
---

# Run Codex in the cloud – DigitalOcean for Codex is now available

As your agents are working on more complex, long-running work, they need a clean, persistent environment to keep running. Setting up a persistent remote machine by hand means creating a cloud server, configuring SSH keys, installing dependencies, and wiring everything back to your workflow. It’s a lot of infrastructure work before you write a single line of code.


Today, we’re making that easier. The **DigitalOcean plugin for Codex** is now available in Public Preview, letting developers create and connect Codex-ready cloud development machines in their own DigitalOcean account directly from within Codex — using natural language, with no manual setup. This means that not only can your work continue running when you step away, but with Codex in the ChatGPT mobile app you can stay in control — starting, steering, or monitoring work from wherever you are.


## What is DigitalOcean for Codex?


The DigitalOcean plugin connects your DigitalOcean account to the Codex app, letting you provision a persistent remote development environment on demand. Instead of manually spinning up a server and configuring it, you can ask Codex to do it.


The result: a DigitalOcean Droplet® that’s pre-configured with the Codex CLI, common programming language tooling (based on the codex-universal Docker image), and SSH access — so your work can keep running and stay within reach, even when you’re not at your desk.


## How it works


### Starting from Codex


Install the DigitalOcean plugin from the Codex plugin directory. During installation, you’ll connect your DigitalOcean account via OAuth — no API tokens to create or paste. Then prompt:


```text
@DigitalOcean create a new remote machine


```


Codex will:


-


Provision a new Droplet from the Codex Droplet template


-


Generate and configure an SSH key on your device


-


Wait for the machine to finish provisioning (and check back automatically when it’s ready)


-


Return a deeplink to the Codex SSH connections page to finalize the connection


Once connected, you’re running Codex on a persistent machine in your own DigitalOcean account. You can ask Codex to set up projects, install dependencies, configure your environment, or spin up and shut down additional machines — all from conversation.


### Starting from the DigitalOcean Marketplace


Already a DigitalOcean user? Find the **Codex Droplet** template in the DigitalOcean Marketplace, create a Droplet, then follow the link to install the plugin in Codex. After OAuth, prompt:


```text
@DigitalOcean connect <droplet id>


```


Codex connects the Droplet to your Codex environment using the same SSH key and deeplink flow.


You can find the droplet id in the URL at[cloud.digitalocean.com/droplets/](http://cloud.digitalocean.com/droplets/)


## What you can do once you’re connected


After setup, your Codex session is running on a cloud machine you control. A few things you can do from there:


-


Ask Codex to configure the environment, install dependencies, or set up a new project


-


Use Codex’s handoff functionality to move an existing local thread to the Droplet and continue work in the cloud


-


Spin up additional machines or shut down ones you no longer need


-


Connect your Droplet to Codex in the ChatGPT app to start, steer, and continue work from your phone


## Getting started


The DigitalOcean plugin for Codex is available today in Public Preview. Install the plugin from the Codex plugin directory or find the Codex Droplet template in the DigitalOcean Marketplace to get started.


**Get started →**[https://marketplace.digitalocean.com/apps/codex-universal](https://marketplace.digitalocean.com/apps/codex-universal)
