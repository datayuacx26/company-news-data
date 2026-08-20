---
schema_version: "1.0.0"
document_id: "69ba04671c7b91056b7fbd5aa2debc7c1e0ea4840d14088d2f715f359a1394b3"
company_key: "yc-pangolin"
company: "Pangolin"
source_id: "yc-pangolin-news-import-841387734568"
canonical_url: "https://pangolin.net/news/1-19-release"
published_at: "2026-06-11T00:00:00+00:00"
first_seen_at: "2026-07-22T08:11:44.188163+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:32788effb1fa86c96145ab4618fd9e1df68f0887c58338286ebfefd5a00d02a2"
---

# Pangolin 1.19: Browser Remote Access — SSH, RDP, VNC & More

Pangolin 1.19 is here. This release brings browser-based remote access to SSH, RDP, and VNC, makes Pangolin SSH dramatically easier to set up, adds automatic updates for site connectors, and ships a handful of organizational improvements that make large deployments easier to manage. Let's walk through it.


## Release highlights


### SSH, RDP, and VNC in the Browser


You no longer need a separate SSH client, remote desktop app, or VNC viewer to reach your infrastructure. Pangolin now supports web-based remote access over SSH, RDP, and VNC as first-class public resource protocols. Assign a domain, point it at a site connector, and users get a full interactive session in any modern browser after completing Pangolin authentication.


For SSH in the browser, that means a real web-based terminal with password, key, or Pangolin identity authentication, depending on how you configure the resource. For RDP in the browser, users get a full Windows remote desktop with clipboard support and file transfers. No Remote Desktop client to install. For browser-based VNC, a remote display rendered right in the tab. No Pangolin client or VPN is required on the user's machine.


This uses the same site-resource model as your HTTP resources. You don't need to run a Pangolin site connector on the target machine. Install Newt on a host that can reach your network, select which sites can route to the resource, and point it at any SSH server, Windows box, or VNC display on that network, just like you've always done with HTTPS.


Each protocol gets its own public FQDN, and you manage your SSH, RDP, and VNC remote access resources alongside your other public resources in one place. Identity-aware access rules, SSO, and geo-blocking apply the same way they do for HTTP.


This makes Pangolin an **Apache Guacamole alternative** for browser-based remote access over SSH, RDP, and VNC with site tunneling built in and stronger authentication support through SSO, identity providers, and granular access rules.


Read more about[SSH](https://docs.pangolin.net/manage/resources/public/ssh) ,[RDP](https://docs.pangolin.net/manage/resources/public/rdp) , and[VNC](https://docs.pangolin.net/manage/resources/public/vnc) in the docs.


### Improved Pangolin SSH


When we shipped native SSH in[Pangolin 1.16](https://pangolin.net/news/1-16-0-release) , the focus was certificate-based authentication, just-in-time user provisioning, and tight integration with OpenSSH. It worked well, but getting there meant configuring an auth daemon, editing` sshd_config` , trusting a certificate authority, and in many cases standing up a bastion pattern for multi-server environments. Powerful, yes, but a fair bit of host setup before your first connection.


Pangolin SSH mode changes that entirely. Instead of routing commands over the network to an OpenSSH server, Pangolin SSH executes sessions directly on the host through the site connector. There's no SSH server to install, no auth daemon to configure, and no` sshd_config` to touch. Select **Pangolin SSH** in the dashboard, make sure Newt runs as root on the machine you want to access, and you're done.


This is now the default when you create an SSH resource. Manual authentication works out of the box with existing host credentials. If you want Pangolin identities provisioned automatically, switch to automated provisioning, still without touching OpenSSH.


Pangolin SSH works on both public and private SSH resources. On a public resource, users get a browser-based terminal at the resource FQDN. On a private resource, connect with the Pangolin client and SSH over the private tunnel:


```text
$   pangolin ssh prod-app.internal


```


The CLI also now supports SCP for copying files over the same private tunnel:


```text
$   pangolin scp ./config.yml prod-app.internal:/etc/app/


```


Same mode, same setup—whether users reach the session through a browser or the CLI.


Standard SSH Server mode is still there when you need it, like reaching a legacy OpenSSH host on the network or running the bastion-and-auth-daemon setup from 1.16. But for the common case, SSH into the machine running your site connector, Pangolin SSH is the path of least resistance.


Read more about[SSH access](https://docs.pangolin.net/manage/ssh) in the docs, or see the[How to SSH with Pangolin](https://pangolin.net/news/how-to-ssh-with-pangolin) guide for browser and private CLI setup.


### Automatic Site Updates


Keeping site connectors up to date across a fleet is tedious. You ship a new Pangolin release, then you SSH into each edge device, pull the latest binary, restart Newt, and hope you didn't miss one.


Automatic updates handle that for you. When enabled, Newt periodically checks for a newer version, downloads it, and restarts itself. The site reconnects on the new version without manual intervention. Pangolin waits 24 hours after a release is published before sites pull it, giving early issues time to surface before your whole fleet updates.


You can enable automatic updates at the organization level for all sites, or toggle it per site. Turn it on globally and disable it on specific sites that need to stay pinned. Turn it off globally and enable it only where you want hands-off updates.


Automatic updates work with binary installations. If you run Newt in Docker or Kubernetes, updates are handled by your orchestration platform as usual.


Read more about[automatic site updates](https://docs.pangolin.net/manage/sites/auto-update) in the docs.


### Labels


As your Pangolin deployment grows, finding the right site or resource in a long table gets old fast. Labels fix that.


Labels are plain string tags you attach to sites, machine clients, and resources. Tag a production site with` prod` , a resource with` customer-1` , a client with` warehouse-1` . Use whatever naming convention fits your workflow. Once attached, you can search and filter by label in the table views for each entity type.


Labels are shared across entity types, which makes cross-referencing easy. Tag the site, the clients, and the resources for a location with the same label and you can filter each table to see everything related to that location.


Add labels inline from any entity table, or manage them organization-wide from the labels page.


Read more about[labels](https://docs.pangolin.net/manage/labels) in the docs.


### Resource Policies


If you've configured public resources before, you know the drill: set up authentication, assign users and roles, configure geo-blocking, repeat for every resource. Resource policies let you define those settings once and attach them to multiple public resources.


A resource policy holds the same authentication and access rule settings you'd configure on an individual resource: SSO, identity providers, PIN codes, user and role assignments, geo-blocking, ASN blocking, IP allow lists, and more. Attach a policy to a resource and it inherits the baseline. Multiple resources can share the same policy, so a team-wide login requirement or geo-block applies everywhere with a single edit.


Policies are additive. A shared policy provides the base layer, and individual resources can add settings on top. Deny all countries in the policy, then add an allow rule for a specific country on one resource. The resource-specific rule sits on top without replacing the shared baseline.


Read more about[resource policies](https://docs.pangolin.net/manage/resources/public/resource-policies) in the docs.


### General Improvements and Bug Fixes


As always, 1.19 also includes various UI improvements and bug fixes throughout the product.


## Documentation & Community


### Helm Chart Documentation


We've added docs for deploying Pangolin and Newt on Kubernetes with Helm, covering repository setup, values files, upgrades, rollbacks, and OCI-based chart installs.


Read more about[Helm installation](https://docs.pangolin.net/self-host/manual/kubernetes/helm) in the docs.


### Community Blueprints


The Community Blueprints repository is a shared library of ready-to-use Docker Compose templates for popular self-hosted apps—Grafana, Jellyfin, Immich, and more—already wired to expose services through Pangolin.


Read more about[Community Blueprints](https://docs.pangolin.net/manage/community-blueprints-repo) in the docs.


## Looking Forward


Browser-based remote access over SSH, RDP, and VNC closes a long-standing gap between public HTTP resources and private terminal access. Whether you need a web-based SSH terminal, remote desktop in the browser, or VNC remote access without a viewer, Pangolin handles it in one platform. Combined with the simpler Pangolin SSH setup path, we're making it easier than ever to get people into their infrastructure without juggling clients, VPNs, and configs. See[How to SSH with Pangolin](https://pangolin.net/news/how-to-ssh-with-pangolin) for a getting-started walkthrough.
