---
schema_version: "1.0.0"
document_id: "6cfc5bad07fdff0b798e4d0c961bdab8adc8396b04f1dd768d8ab759fc6de09e"
company_key: "yc-freestyle"
company: "Freestyle"
source_id: "yc-freestyle-news-import-21b14ff45bb6"
canonical_url: "https://www.freestyle.sh/blog/engineering/private-networking-for-ai-agents"
published_at: "2026-06-04T00:00:00+00:00"
first_seen_at: "2026-07-25T13:26:33.334256+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:2a6fd3abe4a5a21f8ca52e3c8359c356e20f500bc9a51620295d6e6b20407ce1"
---

# Private Networking for AI Agents

The first version of an agent product usually has one machine.


The agent runs code, writes files, calls a model, maybe starts a dev server, and returns a result. That is enough for a demo. It is not enough for a production agent system.


Production agents are rarely single-process programs. They need databases, queues, model gateways, browser workers, preview servers, background jobs, local caches, and debugging access. Some of those services should be reachable from the public internet. Most should not be. The more capable the agent gets, the more important the network boundary becomes.


That is the job of private networking for AI agents: give every agent a real computer, give related computers a private network, and decide deliberately which edges are public.


[Freestyle VMs](https://www.freestyle.sh/products/vms) are built for this shape. A VM can run normal Linux workloads, accept commands and network traffic, stop and start again, fork for parallel exploration, and attach to a Freestyle VPC. The VPC is the private layer: VMs in the same VPC communicate over private IPs without depending on public VM domains.


## Why agent networking gets complicated


Agents do not just make outbound HTTP requests.


A useful coding agent might start a web app on one port, a worker on another, and a database beside it. A data agent might keep a local vector index warm while separate workers transform files. A support agent might need to call internal services without exposing those services to every user session. An eval harness might want one controller VM and a pool of isolated worker VMs.


If you route all of that through public URLs, you create work for yourself:


- Every internal service needs an exposure decision.
- Service discovery becomes a pile of generated hostnames.
- Local debugging requires a different path from production execution.
- Agent-to-agent traffic looks like public internet traffic even when it is private product plumbing.
- Credentials start compensating for weak network boundaries.


Public domains are still useful. They are the right interface for a user preview, webhook receiver, or external callback. But they should not be the default way internal agent infrastructure talks to itself.


The default should be a private network.


## The basic architecture


A practical agent VPC has three layers.


First, create the agent runtime VM. This is the machine the model drives through shell commands, SDK calls, SSH, or a product-specific control plane. It owns the main working session.


Second, attach supporting services to the same VPC. These can be databases, model-sidecar processes, browser workers, build workers, cache servers, or other VMs that should not have public routes as their primary interface.


Third, expose only the ports that need to be public. A preview server can still be mapped through a domain. An internal Redis, Postgres, queue, or worker API can stay on a private IP.


In Freestyle, the VPC is the private address range. A VM joins it by attaching a routed network interface when the VM is created.


```text
import { freestyle } from "freestyle";


const { vpcId } = await freestyle.vpc.create({
name: "agent-network",
cidr: "192.168.10.0/24",
});


const { vm: agentVm } = await freestyle.vms.create({
nics: [
{
default: true,
vpc: vpcId,
mode: "routed",
ipv4: "192.168.10.10",
},
],
});


const { vm: workerVm } = await freestyle.vms.create({
nics: [
{
default: true,
vpc: vpcId,
mode: "routed",
ipv4: "192.168.10.11",
},
],
});


```


That gives the agent VM a stable private address for the worker. If the worker starts a service on` 0.0.0.0` , the agent can reach it over the VPC address:


```text
await workerVm.exec(
"nohup python3 -m http.server 8080 --bind 0.0.0.0 >/tmp/http.log 2>&1 &",
);


const response = await agentVm.exec("curl http://192.168.10.11:8080");
console.log(response.stdout);


```


The important part is not the Python server. The important part is that the agent is using ordinary network tools inside an ordinary Linux VM. It can` curl` ,` ping` , open sockets, run service health checks, and debug the same way your engineers would.


## Pick private IPs like product state


Private IPs are part of your product architecture. Treat them that way.


For small systems, explicit addresses are easiest to reason about. Give the controller VM` .10` , a worker` .11` , a database` .20` , and a cache` .21` . That lets prompts, logs, dashboards, and debugging scripts talk about stable endpoints.


For larger systems, your application can allocate addresses from the VPC CIDR. The operational rule is the same: the address range should not overlap with networks your users, developers, VPN clients, or other internal systems already use. The Freestyle VPC docs call this out because overlap creates confusing routing failures. If your laptop is already on` 192.168.10.0/24` , do not make that your agent VPC range.


The current documented VPC interface uses routed VM interfaces. A VM can attach one routed VPC interface, and` mode: "routed"` is the supported mode. That is enough for the common agent architecture: one private network per workspace, tenant, eval group, or service cluster.


## Debug private systems without making them public


Private networking should not make debugging worse.


When a user reports that an agent is stuck, an engineer needs to inspect the machine, query the service, read logs, and reproduce the failing request. The easy but dangerous answer is to publish more ports. The better answer is temporary operator access into the private network.


Freestyle supports that through ephemeral WireGuard VPN sessions for VPCs. Your computer joins the VPC while the session is active, and traffic to the VPC CIDR goes through the tunnel. Normal internet traffic keeps using your regular network.


The flow is:


1. Create or select the VPC.
2. Create an ephemeral WireGuard session.
3. Write the generated client config to a protected local file.
4. Bring it up with` wg-quick` .
5. Connect to VMs by private IP.
6. Bring the tunnel down and close the Freestyle session when you are done.


The documented API is` vpc.wireguard.createEphemeral()` :


```text
const { vpc } = await freestyle.vpc.create({
name: "debuggable-agent-network",
cidr: "192.168.10.0/24",
});


const connection = await vpc.wireguard.createEphemeral();


console.log(connection.clientConfig);
console.log(connection.sessionId);


// Later, when the operator disconnects:
await connection.close();


```


The generated WireGuard config contains a private key, so handle it as a credential. For local debugging scripts, write it with restrictive file permissions, clean it up on exit, and close the ephemeral Freestyle VPN session. The useful property is that the service did not become public just because a human needed to debug it.


## Give clients scoped access, not your API key


Private networking is one boundary. Identity is another.


If a browser client or end-user agent needs to operate an existing VM, do not ship your Freestyle API key to the client. Create an identity, grant permissions for the VM, create a token, and send that token to the client.


```text
import { freestyle } from "freestyle";


const { vmId } = await freestyle.vms.create();


const { identity } = await freestyle.identities.create();
await identity.permissions.vms.grant({ vmId });


const { token } = await identity.tokens.create();


return { token, vmId };


```


Then the client initializes the SDK with an access token:


```text
import { Freestyle } from "freestyle";


const freestyle = new Freestyle({ accessToken: token });


const { vm } = await freestyle.vms.get({ vmId });
await vm.exec("pwd");


```


This matters for agent products because user-facing control surfaces are often close to powerful infrastructure. A browser UI may need to stream command output, run a health check, or reconnect to a stopped VM. It does not need account-wide resource creation privileges.


Freestyle client session tokens are for operating existing VMs, not replacing your server-side API key. That distinction keeps the product shape clean: your server provisions infrastructure, your client operates the specific workspace it was granted.


## Make lifecycle part of the network design


Agent infrastructure is bursty. A workspace may be active for fifteen minutes, suspend overnight, resume in the morning, and fork when the agent wants to try two approaches at once.


Networking should survive that lifecycle without becoming a special case in your product.


Freestyle VMs are durable runtime objects. A VM can run, stop, start again, resize, fork, and delete. Stopping preserves disk state but not memory. Forking creates a new VM from the current running state so an agent can explore parallel branches of work from the same environment. Idle timeouts let Freestyle reclaim VMs that have no network activity.


For private agent systems, this leads to a simple rule: keep durable service identity in your product metadata, not in a prompt. Store the VPC ID, VM IDs, private IP assignments, exposed ports, and access policy. When the agent resumes, your control plane can recover the topology and tell the model what it needs to know.


If the agent is producing code, documents, or other reviewable project files, keep that durable work in[Freestyle Git](https://www.freestyle.sh/products/git) or another source-of-truth system. The VPC should describe how machines talk to each other; it should not become the only place important project state lives.


Do not make the model rediscover the network every turn. Give it stable facts:


```text
Agent VM: 192.168.10.10
Worker API: http://192.168.10.11:8080
Cache: 192.168.10.21:6379
Public preview: https://preview.example.com


```


That is enough structure for the agent to use the system without turning the network into a guessing game.


## When to expose a public domain


Use a public domain when the caller is outside the private system.


A user preview should be public or at least externally reachable. A webhook receiver needs an address the webhook provider can call. A demo URL should use the domain layer. Freestyle VM domains are built for routing HTTPS traffic from custom domains to services running inside VMs.


Use the VPC for service-to-service traffic inside the agent system. A worker API, local database, queue, model proxy, internal dashboard, or coordination service should usually stay private. If a human needs access, use WireGuard. If a browser client needs to operate a specific VM, use a scoped client token. If the whole internet needs to reach it, then promote that one edge to a domain.


This is the same principle cloud teams already use, but agent products need it earlier. Agents make infrastructure dynamic. They start processes, fan out workers, retry tasks, and keep state across sessions. Without a private network, every internal edge becomes either public by accident or hidden behind a custom tunnel that only works in the happy path.


## A checklist for agent VPCs


Before you ship an agent system, ask these questions:


- Which VMs belong to the same private network?
- Which services need stable private IPs?
- Which ports are internal only?
- Which ports must be reachable by users or third-party services?
- How will engineers debug a private VM without publishing it?
- Which client actions need scoped tokens?
- What happens to the topology when the VM stops, resumes, forks, or is deleted?
- Where does your product store VPC IDs, VM IDs, IP assignments, and access policy?


If those answers are clear, the agent gets a better computer and the product gets a better boundary.


That is the real point of private networking for AI agents. It is not about hiding machines for its own sake. It is about giving agents enough infrastructure to do real work while keeping the blast radius understandable. Public where the product needs public. Private everywhere else.
