---
schema_version: "1.0.0"
document_id: "bd8783ded03160ef8b8297d316bb9014eb166e9016e9b4b3bdeeba370f546751"
company_key: "yc-pangolin"
company: "Pangolin"
source_id: "yc-pangolin-news-import-841387734568"
canonical_url: "https://pangolin.net/news/1-18-release"
published_at: "2026-04-28T00:00:00+00:00"
first_seen_at: "2026-07-22T08:11:44.188163+00:00"
fetched_at: "2026-07-28T21:25:35.830406+00:00"
content_hash: "sha256:f5214b30e0c159e50a753653445b8466a93ac3c8ef1e931d72931ffd04600f8c"
---

# Pangolin 1.18: HTTPS Private Resources, Multi-Site, & Alerts

Pangolin 1.18 is a big one. This release adds HTTPS support for private resources, multi-site high-availability routing, uptime tracking, a flexible alerting system, wildcard resources, and more. Let's walk through everything.


## Release highlights


### HTTPS on Private Resources


Private HTTP is a new kind of private resource designed for web workloads. It works like a public resource in that it gets a real domain name on your Pangolin-managed domain and traffic flows through a reverse proxy with valid TLS, but it's only reachable when the user has an active Pangolin client connection. Nothing is exposed on the public internet.


When a connected user opens the URL in their browser, Pangolin resolves the name through the tunnel, the site-side reverse proxy terminates TLS using a certificate provisioned by the control plane, and the request is forwarded to your backend. The scheme and destination port are both configurable. If you've been approximating this with aliases and non-standard ports, private HTTP is the cleaner answer!


Read more about[HTTPs on private resources](https://docs.pangolin.net/manage/resources/private/private-http) in the docs.


### Multi-Site Routing (HA) on Private Resources


Private resources now support multiple sites. Attach more than one site connector to a resource and Pangolin routes client traffic through whichever path is best at the time, weighing factors like latency and availability. If a site goes offline, clients automatically fail over to the next available site with no manual reconfiguration needed.


A common pattern is redundant connectors into the same network. Install a Pangolin site on two servers in the same LAN, attach both to your private resource, and you have a resilient path in. One connector goes down and users stay connected through the other.


The one requirement is that every site you attach must have routable access to the resource's destination. Pangolin assumes any site in the list is a valid path to the same backend, so confirm reachability before adding a site. Expect a short gap of a few seconds during failover while the downed site is registered and routing changes propagate to clients.


Read more about[multi-site routing on private resources](https://docs.pangolin.net/manage/resources/private/multi-site-routing) in the docs.


### Uptime Tracking


Sites and resources now track uptime. You'll see uptime history on site and resource detail pages, giving you a quick at-a-glance view of recent availability. This also serves as the jumping-off point for creating alert rules. More on that below!


### Standalone Health Checks


Pangolin now supports standalone health checks that aren't tied to any resource. Pick a site to run the probe from, give it a target, choose HTTP or TCP, configure your timing and thresholds, and Pangolin continuously checks whether that endpoint is reachable from the site's network.


This is useful for anything you want to monitor but haven't modeled as a Pangolin resource such as a network printer, an IP camera, a PLC, a legacy server. HTTP checks issue a full request and validate the response; TCP checks simply confirm a connection can be established on a given port.


Read more about[health checks](https://docs.pangolin.net/manage/alerting/health-checks) in the docs.


### Alert Rules


Alert rules let you subscribe to state changes across sites, resources, and health checks and automatically deliver notifications when something happens. Setup involves three steps: choose a source (what to watch), a trigger (which change should fire the rule), and one or more actions (what to do).


Actions include email to users, roles, or arbitrary addresses; webhooks that POST a JSON payload to any URL; and native integrations with PagerDuty, Opsgenie, ServiceNow, and incident.io. You can stack multiple actions on the same rule.


You can create rules from the Alert rules page under Alerting, or jump directly from a site or resource detail page using the Create alert rule shortcut near the uptime graph.


Read more about[alert rules](https://docs.pangolin.net/manage/alerting/alert-rules) in the docs.


### Wildcard Resources


Public resources now support wildcard subdomains. Set the subdomain field to * and Pangolin routes every hostname at that level through the same resource and tunnel. Access rules and authentication apply across all matched hostnames, and the original Host header is preserved so downstream systems can continue routing as expected.


Wildcards require TLS certificates that cover *.your-level, which means DNS-01 validation. HTTP-01 can only prove a single exact hostname. For self-hosted Pangolin, configure Traefik and Let's Encrypt for DNS-01 and set up wildcard DNS records. For Pangolin Cloud, use a domain delegation and Pangolin handles the certificates automatically.


Read more about[wildcard resources](https://docs.pangolin.net/manage/resources/public/wildcard-resources) in the docs.


### General Improvements and Bug Fixes


A handful of smaller but worthwhile additions made it into 1.18 as well:


1.


**Import an identity provider across organizations** . Organization-level identity providers can now be shared across organizations. From the Identity Providers table, click Add Identity Provider and choose Import to see providers from other organizations where you're an administrator. Auto-provisioning settings are configured separately per organization since each has its own roles, but the underlying provider configuration is shared.


2.


**Quickly see resources associated with a site** . On the sites table, clicking the resource count text or opening the three-dot row menu now takes you directly to the resources table with a filter already applied for that site. The site edit page also now shows a simplified list of resources associated with that site.


3.


**Reject pending sites** . Admins can now reject sites from the Pending Sites tab rather than only being able to approve them.


As always, this release also includes various other UI improvements and bug fixes throughout the product.


## Looking Forward


1.18 brings features that connect to each other in meaningful ways: health checks feed into alerting, uptime feeds into alerting, multi-site routing feeds into high availability. We're excited to see how you put it all together!


Give us a star:[https://github.com/fosrl/pangolin](https://github.com/fosrl/pangolin)


Stay tuned!
