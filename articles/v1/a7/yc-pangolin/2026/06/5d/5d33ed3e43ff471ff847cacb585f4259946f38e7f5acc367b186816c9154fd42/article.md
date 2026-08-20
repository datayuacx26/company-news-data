---
schema_version: "1.0.0"
document_id: "5d33ed3e43ff471ff847cacb585f4259946f38e7f5acc367b186816c9154fd42"
company_key: "yc-pangolin"
company: "Pangolin"
source_id: "yc-pangolin-news-import-841387734568"
canonical_url: "https://pangolin.net/news/browser-based-rdp-remote-access"
published_at: "2026-06-14T00:00:00+00:00"
first_seen_at: "2026-07-22T08:11:44.188163+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:69d32ca65311499f778dea0689cbf28b142a54be9431a587b9d1f4d1476c59bf"
---

# RDP in the Browser: Remote Desktop Without Installing a Client

Remote desktop access usually means installing something. Microsoft Remote Desktop on macOS, a third-party tool for cross-platform support, or a managed client pushed through IT. Contractors on personal laptops, helpdesk staff on shared machines, and anyone working from a browser-only environment hit the same wall. You need software before you can see the desktop.


In-browser RDP removes that step. The user opens a URL, authenticates, and gets a full Windows session rendered in the tab, with clipboard, file transfers, and the rest of standard RDP behavior included. A modern web browser is the only thing required on their side.


## What You Get


Pangolin lets you publish Windows desktops as **public resources** : URLs that render a complete Remote Desktop Protocol client in the browser. Users visit the address, sign in, and connect to a Windows host on your network. The session supports interactive desktop control, clipboard copy and paste, and file transfer.


## Why Tunneling Matters


Windows machines on a private network are not directly reachable from a user's browser on the internet. Traditional approaches expose RDP through a VPN, a jump host, or open firewall rules. Each adds operational overhead and widens the attack surface.


Pangolin uses **outbound tunneling** to reach private desktops safely. A **site connector** runs on a machine inside your network and maintains an outbound tunnel to Pangolin. When a user opens the RDP URL, their browser session travels through that tunnel to the Windows host. The desktop stays on the private network; nothing needs a public IP or an open inbound port.


The connector only needs outbound internet access, which is why this pattern works from cloud VPCs, office networks, and environments behind NAT. It does not need to run on the Windows machine itself. As long as it is on the same network and can reach the desktop, one connector can serve many machines. Install a second connector on the same network if you want a redundant route for high availability. Read more about[sites and connectors](https://docs.pangolin.net/manage/sites/understanding-sites) and[installing a site connector](https://docs.pangolin.net/manage/sites/install-site) .


## How a Session Works


1. You assign a URL to the RDP resource in the Pangolin dashboard.
2. The user completes[public resource authentication](https://docs.pangolin.net/manage/resources/public/authentication) in the browser.
3. Pangolin renders the RDP session and sends traffic through the site connector tunnel to the Windows host on port 3389 (or the port you configure).


You specify which Windows machine to connect to (host and port).


## Authentication and Identity Providers


Access is gated in two stages. First, Pangolin verifies the user at the URL: platform login, SSO through an[identity provider](https://docs.pangolin.net/manage/identity-providers/add-an-idp) like Google Workspace or Microsoft Entra ID, geo-blocking, role assignments, or other[access rules](https://docs.pangolin.net/manage/resources/public/authentication) you configure.


Second, after passing that check, the user enters their Windows username and password in the browser-rendered RDP client. Pangolin controls who can reach the URL; Windows credentials control what they can do on the desktop.


Connecting your existing IdP means helpdesk staff and contractors sign in with the same accounts they already use elsewhere in the organization.


## When Browser-Based RDP Fits


This pattern works well when you need to give someone desktop access without pushing client software. Helpdesk troubleshooting a user's machine, a vendor supporting an application on a Windows server, or a contractor who cannot install Remote Desktop on a corporate-managed laptop are all common cases where a URL is easier than a client deployment.


For organizations that have relied on standalone remote-desktop tools for occasional access, clientless RDP through a managed HTTPS endpoint is a practical[TeamViewer alternative](https://docs.pangolin.net/manage/resources/public/rdp) : browser-based remote desktop with outbound tunneling and identity-aware access rules, rather than a separate agent on every machine.


## FAQ


**What is browser-based RDP?**


*Browser-based RDP renders a full Windows remote desktop session inside a web browser. Users visit a URL, authenticate, and interact with the desktop directly in the tab. Clipboard, file transfer, and standard RDP features work without installing Microsoft Remote Desktop or another client.*


**Do users need Microsoft Remote Desktop or another RDP client installed?**


*Users only need a modern web browser. Pangolin renders a full RDP session in the tab, including clipboard support and file transfer. Windows credentials are entered in the browser-rendered client after Pangolin authentication.*


**Do I need to expose RDP port 3389 to the internet?**


*The Windows desktop stays on your private network. A site connector inside that network maintains an outbound tunnel to Pangolin, so inbound firewall rules and a public IP on the desktop are unnecessary.*


**How does authentication work for browser-based RDP?**


*Pangolin verifies the user at the URL first through platform login, SSO via an[identity provider](https://docs.pangolin.net/manage/identity-providers/add-an-idp) , or other[access rules](https://docs.pangolin.net/manage/resources/public/authentication) . After that check passes, the user enters their Windows username and password in the browser-rendered RDP client.*


**Does browser-based RDP support clipboard and file transfer?**


*Yes. Pangolin renders a full RDP client in the browser with interactive desktop control, clipboard copy and paste, and file transfer, matching the features you would expect from a native Remote Desktop client.*


**Can contractors use browser-based RDP on locked-down laptops?**


*Yes. Because the session runs entirely in a browser, contractors and vendors do not need to install Remote Desktop or receive admin rights on their machine. You control access through Pangolin authentication, identity providers, and access rules at the URL layer.*


**Is Pangolin open source?**


*Yes. Pangolin is open source and self-hostable. You can deploy the entire platform yourself, use[Pangolin Cloud](https://app.pangolin.net/auth/signup) , or mix hosted and self-hosted components. Browser-based RDP is available in Pangolin Cloud and Enterprise Edition.*


**How does browser-based RDP compare to TeamViewer?**


*TeamViewer installs an agent on the remote machine and a client on the user's device. Pangolin takes a different approach: users open a URL, authenticate through your identity layer, and get a desktop session in the browser. The Windows host stays on your private network, reached through an outbound tunnel from a site connector rather than a persistent third-party agent.*


## See Also


- [How to SSH with Pangolin](https://pangolin.net/news/how-to-ssh-with-pangolin) : browser and private CLI access, PAM provisioning, and getting started
- [SSH in the Browser](https://pangolin.net/news/browser-based-ssh-remote-access) : web-based terminal access without an SSH client
- [VNC in the Browser](https://pangolin.net/news/browser-based-vnc-remote-access) : remote display access without a viewer
- [How Pangolin Works](https://pangolin.net/news/how-pangolin-works) : architecture overview for sites, resources, and policy
- [Pangolin 1.19 release](https://pangolin.net/news/1-19-release) : where browser-based SSH, RDP, and VNC shipped
- [RDP public resource documentation](https://docs.pangolin.net/manage/resources/public/rdp)
