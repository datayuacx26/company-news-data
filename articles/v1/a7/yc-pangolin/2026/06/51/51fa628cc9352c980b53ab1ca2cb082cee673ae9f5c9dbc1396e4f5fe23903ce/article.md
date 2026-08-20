---
schema_version: "1.0.0"
document_id: "51fa628cc9352c980b53ab1ca2cb082cee673ae9f5c9dbc1396e4f5fe23903ce"
company_key: "yc-pangolin"
company: "Pangolin"
source_id: "yc-pangolin-news-import-841387734568"
canonical_url: "https://pangolin.net/news/what-are-mcp-tunnels"
published_at: "2026-06-03T00:00:00+00:00"
first_seen_at: "2026-07-22T08:11:44.188163+00:00"
fetched_at: "2026-07-28T21:46:32.935029+00:00"
content_hash: "sha256:6e6ecbb11b8740d8fc4014db77a14e23a03073df5ec09bd4f3b7766d9b42ea25"
---

# What Are MCP Tunnels? Secure Private MCP Servers Explained

AI assistants are only as useful as the systems they can reach. For most enterprises, the valuable data and tools live on private networks: internal APIs, ticketing systems, runbooks, configuration databases, and custom automation endpoints. Those services were never meant to sit on the public internet.


The[Model Context Protocol (MCP)](https://modelcontextprotocol.io/) gives agents a standard way to call tools and read context from those systems. The hard part is connectivity. Opening inbound firewall ports, publishing internal MCP servers behind a public load balancer, or allowlisting a vendor's IP ranges on every origin conflicts with how modern security teams want to operate.


**MCP tunnels** address that gap. They let a hosted agent platform or your own agent backend reach MCP servers inside your private network over an **outbound-only** connection. Traffic is encrypted end to end, routed by hostname inside your environment, and layered with authentication on each upstream server. You do not expose MCP listeners to the internet, and you do not depend on users or agents sitting on the corporate LAN.


This article explains what MCP tunnels are, how they differ from ordinary remote MCP hosting, how the architecture works, what the security model assumes, and where they fit next to patterns like tunneled reverse proxies and zero trust remote access.


## MCP tunnel definition


An **MCP tunnel** is a managed connectivity path between a backend. This could be your own backend servers running agent infrastructure, or an AI provider's backend like Anthropic or OpenAI. It connects to one or more **upstream MCP servers** running inside a private network. A small **tunnel stack** you deploy initiates connections outward to a **tunnel edge** , typically managed by the backend side. A **proxy** terminates TLS, validates that traffic originated from the backend, and forwards requests to the correct MCP server based on hostname.


Each exposed server gets a hostname under your tunnel domain (for example,` docs.your-tunnel-domain.example` ). The AI session or API request targets that hostname; the proxy routes to the real MCP process on your LAN, VPC, or cluster.


The defining property is **connection direction** : your infrastructure calls out. Inbound firewall rules for MCP are not required, and you avoid pinning access to a shifting set of vendor egress IPs at every internal service.


## MCP servers and MCP tunnels


Remote agents typically call MCP **servers** over **HTTP** (Streamable HTTP at paths such as` /mcp` is common). The protocol defines tool discovery and invocation; it does not solve network reachability.


An **MCP tunnel** is separate infrastructure: outbound connectivity, TLS, and hostname routing so an external MCP **client** can reach a private HTTP listener. The tunnel carries the same MCP traffic your server would accept on the LAN; it does not replace the protocol or implement your tools.


## Why MCP tunnels matter


### Private tools without public exposure


Agents need the same systems your engineers use: Grafana, Jira, internal CRMs, fleet APIs, or bespoke Python FastMCP services. Publishing those endpoints to the public internet or punching holes in perimeter firewalls increases blast radius. MCP tunnels keep listeners on private addresses while still allowing governed access from managed AI sessions.


### Outbound-only aligns with zero trust


[Zero Trust](https://www.nist.gov/publications/zero-trust-architecture) assumes no implicit trust based on network location. Outbound-initiated connectivity matches how ZTNA connectors, site-based remote access, and tunneled reverse proxies work: the protected environment establishes the path, not the internet at large. MCP tunnels apply that same inversion to AI tool traffic.


### Compliance and data boundaries


Regulated environments often require that payloads stay encrypted in transit, that third-party networks cannot read content, and that access is scoped per application. Tunnel setups combined with strong per-application authentication, such as **OAuth** or **bearer tokens** on each MCP server, help meet these requirements more robustly than solutions like flat VPNs or a single shared API key on a public URL.


## How MCP tunnels work


### Components


A typical deployment runs a lightweight **tunnel connector** inside your network:


Component Role


Tunnel connector Initiates outbound encrypted connections to the tunnel edge, accepts tunneled traffic from the provider, and forwards requests to your internal MCP server. Sometimes, the connector also contains the proxy, so everything is handled within a single component.


Proxy *(optional)*


Terminates TLS, validates that traffic originated from the backend, and routes requests to the correct MCP server by hostname. In some deployments, this function is integrated into the tunnel connector itself rather than running as a separate component.


On the provider side, the **tunnel edge** receives traffic from the AI backend. The **setup** component (often run once at deploy time) talks to the Tunnels API for tokens, certificate registration, and rotation.


You also run one or more **upstream MCP servers** , for example FastMCP, a custom Node service, or an internal gateway, that speak MCP over HTTP at paths such as` /mcp` or` /` .


The stack layers look like this: an external access gateway reaches your network only over an outbound secure tunnel; a connector (often containerized) and optional internal proxy route traffic to MCP tools behind the firewall without opening inbound ports.


### Request flow


At a high level, a request crosses from the agent or API client in the cloud through policy checks, down the encrypted tunnel, and into a private MCP server on your LAN or VPC:


The numbered steps below follow the same path in more detail:


1. An operator starts an **agent session** in a vendor console, or an **API integration** issues a request that references an MCP server URL on your tunnel domain.
2. The provider's cloud gateway applies policy, including **IP allowlists** where configured, then sends the MCP request through **outer mTLS** to the tunnel edge (transport over the connector's outbound path).
3. Traffic reaches your **proxy** , which completes **inner TLS** using a certificate signed by a CA you registered with the provider.
4. The proxy forwards the HTTP request to the upstream MCP server matching the hostname (path is passed through unchanged).
5. The MCP server handles tool calls; responses return along the same encrypted path.


### Hostname routing


Each logical MCP service maps to a **subdomain** of your tunnel domain. The proxy configuration ties` echo.your-tunnel-domain` to` http://echo-svc.internal:8080/mcp` , and` docs.your-tunnel-domain` to another upstream. That mirrors how reverse proxies use virtual hosts, but the client on the internet is the AI platform rather than a browser tab on a user laptop.


### Credential provisioning


Before a tunnel carries traffic, you provision your stack so the provider trusts your connector. The provider gives you a tunnel **ID and token** (from their console or API). You pass that pair to the setup component or connector on boot; it registers with the provider, pulls certificates, and completes the TLS handshake.


For example:


```text
./tunnel -- id   tun_7xk2m9p4 --secret 3f8a1b2c9d0e4f7a6b5c2d1e0f9a8b7c


```


This is a generic example. Flag names, binary names, and formats vary by vendor; the ID and secret are always provider-issued credentials.


The provider cannot connect to your proxy until provisioning finishes, so no tunneled MCP traffic flows until you have explicitly completed trust setup.


## Security model


MCP tunnel security combines **several controls at different layers** , not a single switch. Some run on the provider's cloud gateway before traffic enters the tunnel; others protect the path over the transport network and at each MCP server inside your edge. Understanding where each applies clarifies what the tunnel does and does not guarantee.


Layer What it protects


IP allowlists at the cloud access gateway Random internet hosts attempting the first connection to your tunnel endpoint


Outer mTLS (provider to transport edge, with IP validation) Unauthorized clients impersonating the tunnel path


Inner TLS (provider backend to your proxy) Payload inspection by the transport provider or intermediaries


OAuth or tokens on each MCP server Unauthorized use of tools even when tunnel traffic is legitimate


### IP allowlists at the cloud gateway


Alongside certificate-based tunnel auth and per-server OAuth or bearer tokens, many setups add **IP allowlists** on the **cloud access gateway** , enforced before traffic enters the tunnel toward your edge. Traffic from anything not on the list is dropped; it never hits your connector or MCP servers.


Rules are usually built from the provider's published egress, your own agent cloud, or broad cloud nets, for example a single host` 203.0.113.42/32` , a vendor prefix like` 198.41.192.0/19` , your agent egress` 52.94.1.0/24` , or ASN filters such as` AS16509` (AWS),` AS15169` (Google), or` AS13335` (Cloudflare). Use the ranges your vendor documents; these are illustrative anchors, not a universal checklist.


ASN rules trade precision for maintainability when IPs churn. They complement, not replace, mTLS and application auth. The gateway also spares you from pinning the same vendor IPs on every internal MCP server separately.


### Shared responsibility


**The provider** typically controls tunnel access in your organization, validates your CA before connecting, and restricts which tunnels a workspace can use.


**Your organization** owns everything inside the boundary: tunnel tokens, TLS private keys, proxy hardening, network segmentation for MCP servers, OAuth configuration, certificate renewal, and incident response if credentials leak.


If an attacker obtains both your **tunnel token** and a **TLS private key** , they could impersonate your proxy and read MCP payloads. Treat those secrets like cluster-admin kubeconfig or VPN private keys.


### What the transport provider can see


When the tunnel uses a third-party transport network, that provider **cannot read MCP request or response bodies** if inner TLS is terminated only on your proxy. It may still observe **connection metadata** : egress IP of the host running the connector, timing, byte volume, and the assigned tunnel subdomain. Architecture reviews should document that telemetry for subprocessors and acceptable-use policies.


### MCP tunnel vs. tunnel authentication vs. server authentication


**IP allowlists** (cloud gateway), **tunnel authentication** (certificates and mTLS on the path), and **server authentication** (OAuth, tokens, headers on the MCP process) answer different questions. The first limits which networks can open a conversation with your tunnel domain. The second proves the traffic path and encrypts payloads across the transport layer. The third decides whether a given caller may use specific tools once the request arrives upstream.


The tunnel secures **reachability** and **transport integrity** ; it does **not** log users into your MCP server. If the upstream requires OAuth, API keys, or mutual TLS, configure that on the server the same way you would for any other remote MCP deployment. The agent runtime—whether a console session or an API client—supplies those credentials according to your MCP server documentation.


## Prerequisites and network requirements


Before deploying, teams usually need:


- A **deployment target** : Kubernetes (Helm chart) or a VM with Docker Compose.
- A **tunnel** created in the provider console, with API access for the workspace that will call MCP tools.
- **Outbound connectivity** from the setup component to the provider's control-plane API (hostname and port in vendor docs, commonly 443 TCP), from the connector to tunnel edge addresses (for example` 198.41.192.0/19` on port 7844 TCP and UDP), and from the proxy to internal MCP listeners on your chosen ports.
- One or more **MCP servers** already running and reachable from the proxy host or pod network.


Vendor-managed tunnels usually pair with that vendor's **hosted agent console** and **HTTP APIs** for attaching remote MCP servers. They are not a substitute for every local MCP integration (stdio, IDE plugins); product boundaries change as vendors ship updates.


## Common use cases


- **On-prem files and records** : MCP server on the LAN queries file shares, SharePoint Server, or internal doc repos; the tunnel lets outside agents search without exporting the corpus.
- **Private databases** : MCP server beside SQL Server, Oracle, or Postgres runs governed queries; the tunnel reaches in without opening the database to the internet.
- **On-prem ERP and LOB apps** : MCP server wraps SAP, CRM, or HR APIs on internal hostnames; agents read and write through scoped tools, not a public URL.
- **On-site ops automation** : MCP server drives backup, patching, or facility tools that cannot leave the building; the tunnel replaces VPN-wide LAN access.
- **Pre-production labs** : MCP server on sanitized clones in a test VLAN; tunnel only from non-production agent workspaces.


In each case, security review should cover **which tools** are exposed, **which identities** may invoke them, and **what data** can leave the environment in tool responses.


## Limitations and product reality


Tunnels solve **connectivity** , not **governance** by themselves. You still need tool allowlists, logging, PII review, and human approval workflows for high-risk actions. Zero Data Retention and HIPAA BAA eligibility depend on the provider's feature matrix, not on the tunnel alone.


## How MCP tunnels relate to tunneled reverse proxies


Both patterns share the same architectural idea: **publish capability through an outbound-initiated encrypted path** instead of opening the private network inbound.


A **tunneled reverse proxy** targets human or machine HTTP clients reaching internal web applications: browsers, mobile apps, or service meshes. An **MCP tunnel** targets **MCP clients** embedded in AI runtimes reaching **tool servers** . The payload shape differs (MCP JSON-RPC and tool schemas vs. HTML and REST), but the security story (connector, proxy, hostname routing, deny-by-default at the edge) is familiar to platform engineers.


Organizations already using outbound connectors for ZTNA or tunneled app access can treat MCP tunnels as just **another private upstream** , not an entirely new security paradigm. Pangolin's identity-aware access model fits the same environments where MCP servers are commonly deployed.


## Final thoughts


MCP tunnels exist because AI agents need the same private systems as your staff, and those systems cannot move to the public internet without unacceptable risk. The pattern combines outbound connectivity, layered TLS, and per-server authentication so external agent backends can invoke tools on your terms.


For architects, the decisive questions are who can open a tunnel, which hostnames map to which tools, how credentials rotate, what the transport provider can observe, and what happens when secrets leak. Clear answers there beat any vendor checklist.


Start in a sandbox with one low-risk MCP server; prove tunnel auth, logging, and OAuth before you widen scope.


## Related reading


- [What Is a Tunneled Reverse Proxy? Architecture & Uses](https://pangolin.net/news/tunneled-reverse-proxy)
- [What is an Identity-Aware Proxy (IAP)?](https://pangolin.net/news/what-is-an-identity-aware-proxy)
- [How Does ZTNA Work?](https://pangolin.net/news/how-ztna-works)
- [How Pangolin Works](https://pangolin.net/news/how-pangolin-works)


## FAQ


**What is an MCP tunnel?**


*An MCP tunnel is an outbound-only encrypted path that lets an AI provider reach Model Context Protocol servers inside your private network without opening inbound firewall ports or publishing those servers on the public internet.*


**Do MCP tunnels replace authentication on my MCP server?**


*No. The tunnel provides secure connectivity and routing. Each upstream MCP server should still enforce its own OAuth, API keys, or other auth as you would for any remote MCP deployment.*


**Can I use MCP tunnels with any AI product?**


*Major model providers often ship managed MCP tunnels so their hosted AI backends can reach your private MCP servers, see[Anthropic's MCP tunnels](https://platform.claude.com/docs/en/agents-and-tools/mcp-tunnels/overview) and[OpenAI's Secure MCP tunnels](https://developers.openai.com/api/docs/guides/secure-mcp-tunnels) ; availability and beta headers vary by vendor. That is one deployment shape, not the whole idea. An MCP tunnel is fundamentally a way to give any agentic system, a provider cloud or agents you run yourself, outbound access to tools and data on a private network without inbound firewall holes.*


*If you deploy your own agents in your own cloud against custom tools on private networks, or prefer not to rely on a provider's tunnel infrastructure, you can run your own tunnel stack and route your runtime to the same hostname-scoped MCP endpoints. Check vendor docs for managed options; for self-hosted agents, you own tunnel auth, CA registration, and routing end to end.*


**How is an MCP tunnel different from a VPN for AI tools?**


*A VPN often grants broad network access. An MCP tunnel exposes specific hostname-routed MCP servers to the AI backend, which aligns better with least-privilege tool access.*


**Is the transport provider able to read MCP tool payloads?**


*In designs that terminate inner TLS on your proxy with keys only you hold, the transport layer should not decrypt MCP bodies. Connection metadata such as IPs, timing, and subdomain may still be visible to the transport operator.*


**What do I need before production use?**


*A deployment environment, registered tunnel and CA, outbound network rules, hardened proxy and MCP servers, credential rotation, and governance over which tools and data agents may touch.*
