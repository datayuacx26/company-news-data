---
schema_version: "1.0.0"
document_id: "ffee58f58b289c933ada836b6708d38766ad5e63e6ed312d217a424a75ac79e4"
company_key: "yc-docker"
company: "Docker"
source_id: "yc-docker-rss-8dbda93e62b5"
canonical_url: "https://www.docker.com/blog/empty-sandboxes-break-developer-experience/"
published_at: "2026-08-03T13:00:00+00:00"
first_seen_at: "2026-08-03T13:07:48.193676+00:00"
fetched_at: "2026-08-03T14:10:52.670502+00:00"
content_hash: "sha256:dc03249e4db33ec506be1164f0d56f3ec2d0728eb92e5ba425c3eda7ae3b4af1"
---

# Empty sandboxes break developer experience

I work on[Docker Sandboxes](https://docs.docker.com/ai/sandboxes/) , so I spend a lot of time talking about isolation, microVMs, disposable filesystems, blast radii, all the good infrastructure things.


But the Docker Sandboxes feature I keep reaching for in daily use is[kits](https://docs.docker.com/ai/sandboxes/customize/kits/) .


Kits sound like a packaging detail until you try to use a sandbox for real work. An empty sandbox is a good boundary. It’s also (eventually) ephemeral and empty, and that combination means annoyance and repeated setup work.


The agent gets a clean filesystem, a baseline restricted network, and a clean credentials environment. Then it immediately needs` gcloud` , Java, Maven, some internal CLI, your package registry credentials, and that one skill where you distilled the tacit knowledge your team accumulated for years.


Kits are the escape hatch from that ritual. A kit lets you describe what the sandbox needs, how it should get it, what it may reach, and which credentials it can use, then apply that description when the sandbox starts.


## Empty means setup work


The usual sandboxing story is security-shaped: put the risky thing behind a boundary and limit the blast radius.


Developers rarely keep using tools because the architecture diagram has a nice boundary on it. They keep using tools when the workflow is less annoying than the alternative.


A blank sandbox starts from a place developers rarely start from in practice. Real developer machines have: SDKs, package managers, cloud CLIs, shell setup, local credentials, project docs, cached tools, and configuration nobody wants to reconstruct from memory. Some of it is good engineering. Some of it is archaeology. Both affect whether the agent can complete the task.


The failure is rarely dramatic. The agent spends a few minutes installing packages, hits a blocked registry, asks for an API key it should never see, and the sandbox starts to feel like the thing between you and the work.


At that point, the developer has a choice: spend ten minutes preparing the isolated environment, or run the agent on the host and move on with their life.


We all know which one will win.


## What is an sbx kit?


The[kits docs](https://docs.docker.com/ai/sandboxes/customize/kits/) describe a kit as a` spec.yaml` plus optional files. The useful mental model is simpler: a kit is the contract between the sandbox and the tool you want available inside it.


A kit can install tools:


```text
schemaVersion: "1"
kind: mixin
name: jq


commands:
install:
- command: "apt-get update &amp;&amp; apt-get install -y jq"


```


That is the smallest version. Useful kits usually do more. They can drop files into` /home/agent/` or the workspace, set non-secret environment variables, run startup commands, start background services, and add agent context to files such as` CLAUDE.md` or` AGENTS.md` .


They can also describe the outside world the sandbox is allowed to touch:


```text
network:
allowedDomains:
- api.example.com
- "*.cdn.example.com"
deniedDomains:
- telemetry.example.com


```


And they can connect credentials without copying real secrets into the microVM. The standard pattern keeps the credential on the host, gives the agent a sentinel value, and lets the sandbox proxy inject the real header only when the request goes to an approved service.


```text
network:
allowedDomains:
- api.example.com
serviceDomains:
api.example.com: my-service
serviceAuth:
my-service:
headerName: Authorization
valueFormat: "Bearer %s"


credentials:
sources:
my-service:
env:
- MY_SERVICE_API_KEY


environment:
proxyManaged:
# Agent sees "proxy-managed"; the host proxy injects the real token.
- MY_SERVICE_API_KEY


```


Inside the sandbox the agent sees` MY_SERVICE_API_KEY=proxy-managed` . The actual secret stays on the host. The proxy replaces the header on the way out.


That distinction is why credential support belongs in the kit contract. If the sandbox exists to keep the agent away from host secrets, copying those secrets into the microVM would be a strange way to celebrate.


## Mixin kits are the norm


There are two kit shapes in the spec. A` kind: sandbox` kit defines a full agent runtime: image, entrypoint, policy, the whole thing. Use that when you are building an agent.


Most integrations should be mixins.


A mixin kit extends an existing sandbox with one capability. It installs the tool, opens the narrow network path, wires credentials, and gives the agent enough instructions to use the thing. The runtime stays with the agent kit.


That is the shape I use for most of my own kits. For example, the kits I keep using daily are[agy](https://github.com/shelajev/agy-sbx-kit) ,[yt-transcript](https://github.com/shelajev/yt-transcript-sbx-kit) , and[tessl](https://github.com/shelajev/tessl-sbx-kit) .


The YouTube kit is exactly what you think: give the sandbox the tools to fetch transcripts and media metadata without turning every new sandbox into a small dependency archaeology project. The Tessl kit is even more direct. It brings skills into the agent running inside the sandbox, so I do not need to inject them manually like a medieval peasant.


The nice part of mixins is that they stack.


A giant “Oleg’s entire laptop, but in a microVM” kit would be funny once and then become a maintenance incident. You want small kits with clear jobs:


- a Java kit that installs a JDK, Maven, SDKMAN!, team Maven settings, and links to Spring docs;
- a` gcloud` kit that installs the CLI, allows the right Google API domains, and wires credentials through the proxy;
- a Google Workspace kit that gives the agent access to your email and Google Docs;
- a Tessl kit that brings skills into the sandbox;
- a YouTube transcript kit that adds` yt-dlp` ,` ffmpeg` , and whatever network access those need.


Then a sandbox can be assembled for the task:


```text
sbx run claude . \
--kit docker.io/acme/sbx-java-kit:1.0 \
--kit docker.io/acme/sbx-gcloud-kit:1.0 \
--kit docker.io/acme/sbx-tessl-kit:1.0


```


The same agent now starts with a different contract around it.


At that point kits stop being a packaging mechanism and start being a productivity feature. The sandbox stays disposable, but the setup becomes repeatable. The developer can throw away the environment without throwing away the knowledge of how to rebuild it.


## Sharing is caring


Local setup scripts are fine until the second person needs them. At that point they become documentation, and documentation becomes stale with excellent punctuality. Then someone pastes a token into a config file because the happy path was missing.


A kit gives that setup a place to live.


Vendors can publish kits for their CLIs or APIs. Inside a company, the same pattern works for package registries, cloud accounts, corporate proxy certificates, and preferred language toolchains. The user gets one` --kit` flag instead of a wiki page and a feeling of mild dread.


Distribution matters here. Kits support local directories, Git URLs, and OCI artifacts. For shared kits, OCI distribution is the obvious path because users can reference a versioned artifact directly:


```text
sbx run claude --kit docker.io/acme/sbx-my-product-kit:1.0


```


Keep the source in GitHub or wherever your team collaborates. Publish the artifact to Docker Hub or another OCI registry. The source repo is where people review, patch, and complain politely. The registry is what makes the kit easy to consume.


## All in all


Security is a good reason to care about kits. The network and credential contract becomes explicit, which is useful by itself. The daily-use reason is more prosaic: kits make sandboxes survivable as a development tool.


An empty sandbox is a boundary. A configured sandbox is a place where an agent can actually work. Kits are how that configuration becomes repeatable, reviewable, and shareable.


The[kits docs](https://docs.docker.com/ai/sandboxes/customize/kits/) and[examples](https://docs.docker.com/ai/sandboxes/customize/kit-examples/) are enough to build a first mixin kit without inventing the shape from scratch.


Isolation only survives contact with developers when it is at least as convenient as skipping it.
