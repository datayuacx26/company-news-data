---
schema_version: "1.0.0"
document_id: "a51d3bb6185d25d48209144aa0efebe3b7bb1a7db7ba449fc23d3d1b593cb702"
company_key: "yc-blaxel"
company: "Blaxel"
source_id: "yc-blaxel-rss-eda12eea7869"
canonical_url: "https://blaxel.ai/blog/sandbox-providers-with-custom-domains"
published_at: "2026-05-21T17:28:17+00:00"
first_seen_at: "2026-07-20T23:20:26.598006+00:00"
fetched_at: "2026-07-28T20:50:36.289404+00:00"
content_hash: "sha256:289b6350cc6b4fa44fed057858c69b369edfcb50c916a4b4abe0d68c0b310cee"
---

# AI sandbox providers with custom domain support

Your enterprise customer's security team flagged` acme-corp.someprovider.dev` in a vendor assessment. The finding: subdomain takeover risk on a domain your team doesn't control. The deal stalls until you can serve the product from` app.acmecorp.com` .


Buyers of enterprise-facing AI products expect the networking stack they see from production SaaS. Branded URLs, predictable egress IPs, and clean credential boundaries show up as common requirements. When sandbox infrastructure skips this layer, the engineering cost lands on the platform team. Engineers run reverse proxies, manage TLS certificates, and build IP tunneling infrastructure instead of shipping product features.


This guide explains why custom domains became a procurement gate. It walks through the networking stack behind a branded domain and compares how providers handle each piece.


## **Why custom domain support shapes AI platform deals**


Custom domain support comes up at three different points in an enterprise security review. Each one stalls a deal on its own when the underlying sandbox infrastructure can't support branded URLs.


### **Branded URLs as enterprise procurement requirements**


The enterprise requirement traces to a classified security vulnerability. The[OWASP testing guide](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover) lists subdomain takeover as test case WSTG-CONF-10. The classification falls under Configuration and Deployment Management Testing. NS-record takeovers carry the highest impact: a successful attack hands the attacker full control of the DNS zone.


When your platform assigns` customer.vendor.ai` , the customer has no DNS control. Without DNS control, they cannot remediate subdomain takeover risk on their own. The gap surfaces as a documented finding that blocks contract execution until a CISO grants an exception.


The consequences land on the enterprise: loss of control over subdomain content, brand damage, negative press, and reputational risk. AI platforms that dynamically provision and deprovision inference endpoints carry elevated risk. Each provisioning cycle that leaves DNS records pointing to deleted resources creates a dangling DNS window. Attackers can claim that record before cleanup completes.


The vulnerability maps to multiple compliance frameworks, including ISO 27001, NIST 800-53, and NIST 800-171. Enterprises under these frameworks usually flag vendor-controlled subdomains through supplier risk management and security governance reviews. Custom domain support clears the finding before it ever appears.


### **Cookie scoping and authentication boundaries**


Generic provider subdomains break authentication in three ways that custom domains solve:


- **Cross-tenant cookie exposure:** Setting a cookie domain to` .platform.com` causes the browser to transmit that cookie to every subdomain. In multi-tenant architectures, one customer's session token reaches every other customer's endpoint, even when application logic tries to scope it.
- **Enterprise SSO verification:** DNS-based domain verification is an industry standard for SSO setup. Only the enterprise can complete that verification because they alone control the DNS zone. A vendor-assigned subdomain makes the verification structurally impossible.
- **Silent token renewal failures:** The renewal flow uses an iframe in a cross-site embedded context. Browsers treat the authorization server's session cookies as third-party, so` SameSite=Lax` cookies don't transmit. Teams build localStorage and refresh-token workarounds that add code paths to maintain and widen the attack surface for authenticated flows.


Each problem stalls a different part of the enterprise authentication stack. Custom domains rather than vendor subdomains are the standard for production SaaS for exactly this reason.


### **TLS certificate lifecycle across many customer domains**


When you ship branded subdomains for many enterprise customers, certificate management becomes ongoing work. Each customer domain runs its own authorization and challenge response sequence under the[ACME protocol](https://datatracker.ietf.org/doc/html/rfc8555) . The protocol has no batching mechanism for authorizations, so large tenant counts mean many parallel authorization flows.


[Let's Encrypt rate limits](https://letsencrypt.org/docs/rate-limits/) add a hard constraint: 50 certificates per registered domain per seven days. The limit applies globally, not per account, and every subdomain of` bigcorp.com` shares one pool. Platform teams that ignore this hit the limit during customer onboarding spikes.


Wildcard certificates cover exactly one subdomain level. A wildcard for` *.platform.com` doesn't cover` api.tenant.platform.com` . Multi-level subdomain architectures require either multiple wildcards or per-tenant certificates. Platforms that automate this lifecycle take certificate engineering off their customers' plates.


Without automation, every integrating team owns the work. Engineering hours that should go toward product features get spent on rotating tokens, debugging failed challenges, and chasing expiry alerts.


## **What production-grade networking looks like for AI sandboxes**


Custom domains handle inbound traffic. Enterprise-facing AI products with customer-facing endpoints or allowlist-driven outbound access also need outbound traffic control and clean credential boundaries. The full stack has three layers: managed custom domains, static egress IPs, and proxy secrets injection. Most providers handle one piece and leave teams to build self-hosted workarounds for the rest.


### **Managed custom domains with automated TLS**


"Managed" means the platform handles DNS verification, ACME challenge flows, certificate issuance, renewal, and routing to the correct sandbox instance. The platform team adds a domain through an API call or dashboard. Certificates get provisioned automatically, renewed before expiry, and matched to traffic based on subdomain.


When this layer isn't managed, teams build it themselves. The work means running a reverse proxy like Caddy, Nginx, or Traefik. Configuration covers ACME DNS-01 or HTTP-01 challenges and Cloudflare API tokens for challenge resolution. Teams also monitor certificate expiry across every customer domain. DNS-01 credentials need careful scoping because a compromise allows certificate issuance for any hostname in the managed zone.


For platforms operating across many customer domains, DCV delegation cuts down validation work. The customer sets a permanent CNAME at` _acme-challenge.app.customer.com` pointing to the platform's DNS zone. From there, the platform controls the write path for validation tokens entirely. The only customer involvement is the initial CNAME setup. Without managed custom domains, your engineering team builds and maintains this infrastructure for every customer.


### **Static egress IPs for enterprise allowlists**


Enterprise firewalls follow the default-deny model defined in[NIST SP 800-41](https://nvlpubs.nist.gov/nistpubs/legacy/sp/nistspecialpublication800-41r1.pdf) . All traffic is blocked unless explicitly allowed. If your sandbox's outbound IP changes, the enterprise firewall drops the traffic. Static egress IPs give enterprise customers a stable IP range to allowlist.


The requirement triggers when sandbox outbound calls reach enterprise-controlled infrastructure. Three patterns surface most often:


- **Customer-hosted APIs behind corporate firewalls:** Internal services only accept traffic from allowlisted IPs, so the sandbox needs a stable outbound identity.
- **Partner databases with IP-based access controls:** Direct database connections bypass the customer's API layer, which means the database firewall sees the sandbox IP directly.
- **Webhook receivers enforcing inbound IP allowlists:** The customer's webhook endpoint validates source IP before accepting payloads. Egress IP changes cause silent delivery failures.


Network isolation and egress control are standard items in any regulated-industry security review.


Self-hosted egress infrastructure carries real cost. A single-AZ[AWS NAT Gateway](https://aws.amazon.com/vpc/pricing/) adds a recurring baseline before data processing even starts. Adding AWS Network Firewall for domain filtering raises the baseline further per AZ.


A production multi-AZ deployment reaches meaningful recurring cost before any traffic flows. The infrastructure bill compounds with engineering time spent on HOME_NET scoping, IGW route tables, rule propagation, and dual logging.


### **Proxy secrets injection**


Proxy secrets injection keeps credentials out of the sandbox runtime entirely. An outbound API call from the agent routes through a proxy. The proxy injects the credential into HTTP headers or body before forwarding to the upstream. The agent process never sees the raw key.


This pattern differs from environment variable injection. Environment variables materialize secrets inside the agent's process memory at startup. They stay readable via` /proc/self/environ` on Linux throughout execution. A prompt injection attack that causes the model to inspect its environment exposes real upstream credentials. Proxy injection makes that attack worthless because the process environment holds no real credentials.


The compliance difference is concrete. Proxy injection generates a discrete, attributable audit event for every credential use. That trail supports[NIST SP 800-53](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final) control families: access control, audit and accountability, and identification and authentication. The same logging granularity satisfies HIPAA tracking requirements for systems handling protected health information.


## **How sandbox providers with custom domains compare today**


The comparison below covers six providers across the three networking capabilities discussed above. No single provider ships all three at general availability. The differences come down to whether each capability is native, self-hosted, preview-only, or platform-level rather than sandbox-level.


Provider Custom domains Static egress IPs Proxy secrets injection TLS automation Notes


Blaxel Native (apex, subdomain, wildcard) Private preview Native Automated Perpetual sandbox platform with full networking stack


E2B Self-hosted workaround documented Self-hosted approach Not documented Team-managed via Caddy ACME flow Self-hosted setup documented


Modal Launched (May 2026) Beta (for proxies/static egress IPs) Not documented Automated (incl. wildcard) Custom domains launched


Cloudflare Sandboxes Preview URLs only Not available Not available Via Workers ecosystem Ephemeral (files deleted on pause)


Vercel Sandbox Platform-level only Not available Firewall proxy for outbound key injection Via Vercel platform Integrated with broader Vercel platform


Daytona Not a managed feature, customer must build their own custom proxy Requested feature, not shipped Not documented Not documented for sandboxes Proxy functionality documented


### **Native managed custom domains**


Blaxel and[Modal](https://blaxel.ai/blog/modal-pricing-alternatives-guide) handle domain routing, TLS, and certificate lifecycle as first-class platform features.


Blaxel's[custom domains](https://docs.blaxel.ai/Infrastructure/Custom-domains) cover wildcard subdomains automatically once a domain is registered. Setup happens in the Blaxel Console: add the domain, configure DNS records, verify ownership. Each domain binds to a single region, and registered domains feed white-label sandbox preview URLs without separate certificate orchestration.


Custom domains are plan-gated, and initial certificate generation adds latency on first access. For sandbox tunnels specifically, Modal uses NS record delegation to its nameservers.


"First-class" means the platform handles the full lifecycle. No GCP VMs, no Caddy configurations, no Cloudflare API tokens managed by your team. For AI platform builders serving dozens or hundreds of enterprise customers, this difference matters. Domain management becomes a config setting rather than a staffing decision.


### **Self-hosted networking workarounds**


E2B doesn't ship native custom domain support on its hosted platform. The[official E2B path](https://e2b.dev/docs/sandbox/custom-domain) requires teams to assemble the routing layer themselves in five steps:


1. Provision a GCP VM (` n2-standard-2` ) and install Docker and Caddy.
2. Write a Caddyfile that extracts sandbox IDs from subdomains.
3. Configure a wildcard DNS A record in Cloudflare.
4. Set up TLS via Caddy's ACME DNS challenge with a` CLOUDFLARE_API_TOKEN` .
5. Map` 8080-sandboxid.mydomain.com` to` 8080-sandboxid.e2b.app` so sandboxes stay accessible under` *.e2b.app` .


The operational consequences land on the team. Engineers own VM uptime, Caddy configuration, DNS records, and TLS renewal across every customer domain. The proxy becomes another piece of infrastructure your on-call rotation monitors. Multi-tenant deployments scale the overhead linearly. Expect a GCP VM per region, a Caddyfile per routing pattern, and a Cloudflare integration per DNS zone. Audit your team's capacity for this work before committing to the architecture.


E2B's[Python SDK reference](https://e2b.dev/docs/sdk-reference/python-sdk/v1.5.1/sandbox_async) lists a` domain` parameter for requests, but it only applies to self-hosted environments. It does not assign a custom public domain on E2B's hosted cloud.


### **Platform-level domain wrappers**


Vercel and Cloudflare both support custom domains, but mainly at the platform level rather than at the sandbox primitive.


[Vercel Sandbox](https://vercel.com/docs/vercel-sandbox) runs inside Firecracker microVMs on Amazon Linux 2023, accessed through the Vercel SDK and CLI. Vercel's[domain configuration](https://vercel.com/docs/domains/set-up-custom-domain) covers team-level settings, project-level settings, deployment assignment, and CLI-based setup. The docs don't describe binding a custom domain directly to a sandbox instance. Vercel Sandbox does document a firewall proxy that intercepts outbound requests and injects API keys at the network level. That's a notable credential isolation pattern. The sandbox runs inside the broader Vercel platform rather than as a standalone service with its own domain routing.


[Cloudflare Sandboxes](https://developers.cloudflare.com/sandbox/) run in Cloudflare's container runtime. The docs describe custom domains for sandbox preview URLs via wildcard DNS and Worker routing. No documented workflow binds a custom domain directly to a specific sandbox instance. Preview URLs are auto-generated for development use.


The persistence model also limits production use. Per[Cloudflare's architecture documentation](https://developers.cloudflare.com/containers/platform-details/architecture/) , all disk is ephemeral. When a container instance pauses, the next start brings a fresh disk from the container image. Files reset on sleep or shutdown. Teams can mount R2 buckets or use a backup-and-restore API for persistence, but the configuration is manual.


For AI platform builders, platform-level and sandbox-level domain support serve different purposes. Platform-level routing handles deployments while sandbox-level routing handles per-customer isolation. Verify which abstraction level your architecture requires before committing.


### **Limited custom domain support**


Daytona requires customers to implement their own Custom Preview Proxy for sandbox preview URLs. The proxy handles custom domains, authentication, and error handling. The feature targets preview URLs rather than persistent production endpoints.


Static egress IPs aren't available.[GitHub issue #3825](https://github.com/daytonaio/daytona/issues/3825) in Daytona's official repository tracks "Predictable Outbound Networking" as a requested feature. The request covers predictable outbound IP addresses or CIDR ranges for firewall allowlists, but the capability isn't shipped today.


Proxy-level secrets injection isn't documented either, though the proxy injects authentication headers for routing and auth. Daytona's networking surface covers port forwarding, preview URLs, outbound network controls, and VPN tunnels to development environments. Native custom domains for production endpoints, static egress IPs, and proxy credential injection are all missing.


The combined gap puts Daytona below the networking bar enterprise-facing AI products hit during procurement. Teams considering Daytona for production AI agent workloads should expect to layer their own networking infrastructure on top. The operational tax follows directly from that decision.


## **Evaluating sandbox providers for branded AI products**


Procurement evaluation comes down to three areas. Each one is where a polished vendor demo often hides production failure modes.


### **Match the networking stack to your customer base**


Teams selling into self-serve and SMB markets often don't need static egress IPs for every sandbox. Enterprise customers with allowlist-based security policies change the calculus entirely. If your near-term deals include firewall allowlist requirements, static egress IPs move from nice-to-have to procurement dependency.


Pre-GA feature status matters here too. Modal's static egress IPs sit in beta, while Blaxel's dedicated egress gateways are in private preview. Factor those timelines into vendor evaluation and confirm GA dates in writing during contract discussions.


### **Test the full stack end-to-end before procurement**


A custom domain demo can still fail under heavy TLS renewal volume, wildcard edge cases, or multi-region routing. Migrating away from a provider that fails under real usage costs far more than thorough evaluation upfront.


Before signing a contract, run a representative test across three areas:


- **Certificate lifecycle:** Provision a representative set of customer subdomains and verify certificate renewal cycles complete on time.
- **Egress routing:** Route outbound traffic through the egress path and confirm allowlisted destinations receive traffic from the expected source IPs.
- **Credential injection:** Confirm that proxy secrets injection handles multiple upstream APIs with different credential schemes.


Each test surfaces failure modes that vendor demos rarely cover.


### **Extend evaluation to storage and tool networking**


If your architecture also includes shared storage or tool servers, evaluate those networking boundaries too. The same domain, egress, and credential questions extend beyond sandboxes into data sharing and tool execution. In Blaxel's stack, that means evaluating Sandboxes alongside Agent Drive and Batch Jobs, not sandbox previews in isolation.


## **Choose sandbox providers with custom domains that match your full networking stack**


Custom domain support is one piece of a broader networking stack. Enterprise-facing AI products with customer URLs or allowlist-driven outbound access also need egress control and credential boundaries. Engineering teams don't want to run reverse proxies, IP tunnels, and credential proxies on top of another sandbox layer. Every self-hosted workaround adds on-call surface area and another failure mode your team owns.


Among the providers covered here,[Blaxel](https://blaxel.ai/sandbox) offers managed custom domains,[dedicated egress gateways](https://docs.blaxel.ai/Infrastructure/Dedicated-egress-gateways) , and[proxy secrets injection](https://docs.blaxel.ai/Sandboxes/Proxy-secrets-injection) . Dedicated egress gateways are currently in private preview. Teams building[coding agents](https://blaxel.ai/blog/sandbox-management-for-ai-coding-agents) can extend that networking surface with[Agent Drive](https://docs.blaxel.ai/Agent-drive/Overview) for shared context across sandboxes.[Batch Jobs](https://docs.blaxel.ai/Jobs/Overview) handles parallel processing workflows.


[Book a demo](https://blaxel.ai/contact) or[start free](https://app.blaxel.ai/) .


Ship branded AI products with managed networking


Native custom domains, dedicated egress gateways, proxy secrets injection, and automated TLS. No self-hosted reverse proxies or IP tunnels to maintain.


[Start free](https://app.blaxel.ai/)


## **Frequently asked questions**


### **Do all sandbox providers with custom domains automate TLS certificate provisioning?**


No. Modal documents automated TLS for custom domains, including wildcard subdomain certificate issuance. Blaxel automates the same lifecycle once a domain is registered. E2B's custom domain guide shows a self-managed path using Caddy's ACME DNS challenge with Cloudflare API tokens. The operational gap between automated and self-managed TLS grows with each customer domain you onboard.


### **How does proxy secrets injection differ from environment variable injection?**


Environment variables place credentials inside the agent's process memory, where` /proc/self/environ` exposes them on Linux. Proxy secrets injection routes outbound requests through a proxy that adds credentials server-side before forwarding upstream. The agent process never holds the raw key. The difference matters under prompt injection attacks. A model that inspects its environment finds nothing useful when proxy injection is in place.


### **Why do enterprise customers ask for static egress IPs alongside custom domains?**


Custom domains handle inbound branding and DNS control. Static egress IPs handle outbound allowlisting. Enterprise firewalls run on the default-deny policy defined in the NIST SP 800-41 framework. Any change to your sandbox's outbound IP causes the firewall to drop the connection silently. Both controls matter when enterprise procurement includes inbound and outbound network review in the same security assessment.


### **Can sandbox providers with custom domains route multiple domains to the same sandbox instance?**


Routing behavior varies by provider. Modal supports multiple domain names on the same web endpoint. Blaxel auto-provisions wildcard subdomains when a domain is registered. Sandbox platforms serving many enterprise customers should verify three sandbox-level capabilities: per-customer isolation, wildcard routing, and multi-domain assignment. Platform-level deployment routing won't cover those cases.
