---
schema_version: "1.0.0"
document_id: "b8a35d483aadea1bc681a5efc87980db9add0bf7f18ca56af2655083582dec1a"
company_key: "yc-pangolin"
company: "Pangolin"
source_id: "yc-pangolin-news-import-841387734568"
canonical_url: "https://pangolin.net/news/browser-based-vnc-remote-access"
published_at: "2026-06-14T00:00:00+00:00"
first_seen_at: "2026-07-22T08:11:44.188163+00:00"
fetched_at: "2026-07-28T21:54:03.866440+00:00"
content_hash: "sha256:0c3635ecb5537cbc1f0ac45beefb465ea8498b2f5f8a94b580a78d44b1f223ab"
---

# VNC in the Browser: Remote Display Access Without a Viewer

VNC has been around long enough that most people assume you need a viewer. RealVNC, TigerVNC, TightVNC: pick one, install it, configure the connection, and hope the firewall rules cooperate. That is manageable for a team that lives with VNC daily. It is less manageable when the person connecting is a contractor, a field technician, or someone who needs to reach a headless box or industrial UI once and move on.


Web-based VNC changes the starting point. Instead of distributing viewer software, you give the user a URL. They authenticate, and the remote display renders in the browser. The user's machine only needs a web browser.


## What You Get


Pangolin lets you publish VNC displays as **public resources** : URLs that render a full VNC client in the browser. Users visit the address, sign in, and get an interactive remote display session on a host in your network.


## Why Tunneling Matters


VNC servers typically listen on a private network. Exposing them directly to the internet is risky, and many environments simply cannot open the required ports.


Pangolin uses **outbound tunneling** to bridge that gap. A **site connector** installed on a machine inside your network maintains an outbound tunnel to Pangolin. When a user opens the VNC URL, their browser session travels through that tunnel to the VNC server. The display stays on the private network without inbound firewall rules or a public IP.


The connector only needs outbound internet access, which makes this workable from industrial networks, cloud environments, and sites behind NAT. It does not need to run on the same machine as the VNC server. As long as it is on the same network and can reach the display, one connector can serve many hosts. A second connector on the same network gives you a redundant path for high availability. Learn more in the[sites overview](https://docs.pangolin.net/manage/sites/understanding-sites) .


## How a Session Works


1. You assign a URL to the VNC resource in the Pangolin dashboard.
2. The user completes[public resource authentication](https://docs.pangolin.net/manage/resources/public/authentication) in the browser.
3. Pangolin renders the VNC session and sends traffic through the site connector tunnel to the VNC server.


You specify the VNC server host and port (commonly` 5900` , or` 5900 + display number` for multi-display setups).


If the VNC server requires credentials, the user enters them in the browser-rendered client after passing Pangolin authentication.


## Authentication and Identity Providers


VNC public resources support the same[authentication and access rules](https://docs.pangolin.net/manage/resources/public/authentication) as other public resources. You can require platform login, connect an[identity provider](https://docs.pangolin.net/manage/identity-providers/add-an-idp) for SSO with Google Workspace, Microsoft Entra ID, or any OAuth2/OIDC system, and apply rules based on user, role, or context.


That means a contractor who should only reach one display gets a URL with rules attached, rather than broad network access. The identity layer that protects your web apps and SSH terminals applies to VNC as well.


## Unified Protocol Management


One practical advantage of running VNC alongside SSH and RDP as public resources is that everything lives in the same dashboard. You manage your VNC resources next to your SSH terminals and RDP desktops in one place. For a full overview of SSH setup, including browser and private CLI paths, see[How to SSH with Pangolin](https://pangolin.net/news/how-to-ssh-with-pangolin) .


If you have been running a separate web gateway for protocol access, Pangolin handles SSH, RDP, and VNC together. That is an alternative to RealVNC or TigerVNC viewers for day-to-day access, and a simpler operational model than maintaining a dedicated[Guacamole alternative](https://docs.pangolin.net/manage/resources/public/vnc) as a standalone deployment. To get started,[install a site connector](https://docs.pangolin.net/manage/sites/install-site) on a host that can reach your VNC server.


## FAQ


**What is browser-based VNC?**


*Browser-based VNC renders a remote display inside a web browser. Instead of installing a VNC viewer, users visit a URL, authenticate, and view and control the remote screen directly in the tab. The session is proxied through an outbound tunnel from a site connector on your network.*


**Do I need a VNC viewer like RealVNC or TigerVNC to connect through the browser?**


*Users only need a modern web browser. Pangolin renders a full VNC client at a public URL. If the VNC server requires credentials, the user enters them in the browser after passing Pangolin authentication.*


**What port does VNC typically use?**


*VNC servers commonly listen on port 5900, or 5900 plus the display number for multi-display setups (5901 for display :1, and so on). You configure the host and port when creating the VNC public resource in Pangolin.*


**Do I need to expose VNC to the internet for remote access?**


*The VNC server stays on your private network. A site connector inside that network maintains an outbound tunnel to Pangolin, so you do not need to open inbound firewall ports or assign the server a public IP.*


**When is browser-based VNC a better fit than SSH?**


*SSH gives you a text terminal, which is ideal for servers and command-line work. VNC gives you a graphical remote display, which is better for headless boxes with a desktop environment, industrial HMIs, legacy applications with a GUI, or any system where you need to see and interact with a screen rather than a shell.*


**Does the site connector need to run on the same machine as the VNC server?**


*The site connector only needs to be on the same network and able to reach the VNC server. One connector can serve many hosts on that network. Install a second connector on the same network if you want a redundant path for high availability.*


**Can I manage SSH, RDP, and VNC resources in one dashboard?**


*Yes. Pangolin treats SSH, RDP, and VNC as public resources alongside your web applications. You manage them in the same dashboard, apply shared access policies, and monitor health from one place.*


**Is Pangolin open source?**


*Yes. Pangolin is open source and self-hostable. You can run the full platform on your own infrastructure, use[Pangolin Cloud](https://app.pangolin.net/auth/signup) , or combine both. Browser-based VNC is available in Pangolin Cloud and Enterprise Edition.*


**What is a good Apache Guacamole alternative for browser-based VNC?**


*Apache Guacamole provides in-browser VNC, SSH, and RDP through a dedicated gateway you deploy and manage. Pangolin offers clientless VNC access as part of a unified platform with site connectors, identity provider integration, and shared access policies across web apps, terminals, and remote desktops.*


## See Also


- [How to SSH with Pangolin](https://pangolin.net/news/how-to-ssh-with-pangolin) : browser and private CLI access, PAM provisioning, and getting started
- [SSH in the Browser](https://pangolin.net/news/browser-based-ssh-remote-access) : web-based terminal access without an SSH client
- [RDP in the Browser](https://pangolin.net/news/browser-based-rdp-remote-access) : remote desktop without installing a client
- [How Pangolin Works](https://pangolin.net/news/how-pangolin-works) : architecture overview for sites, resources, and policy
- [Pangolin 1.19 release](https://pangolin.net/news/1-19-release) : where browser-based SSH, RDP, and VNC shipped
- [VNC public resource documentation](https://docs.pangolin.net/manage/resources/public/vnc)
