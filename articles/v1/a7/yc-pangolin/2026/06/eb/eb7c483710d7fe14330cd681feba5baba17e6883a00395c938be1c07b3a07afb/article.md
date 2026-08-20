---
schema_version: "1.0.0"
document_id: "eb7c483710d7fe14330cd681feba5baba17e6883a00395c938be1c07b3a07afb"
company_key: "yc-pangolin"
company: "Pangolin"
source_id: "yc-pangolin-news-import-841387734568"
canonical_url: "https://pangolin.net/news/browser-based-ssh-remote-access"
published_at: "2026-06-14T00:00:00+00:00"
first_seen_at: "2026-07-22T08:11:44.188163+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:f91ef0a5540bdf237cfca2617b43ff6df9ce0dd124c96016f65be3f977090dbd"
---

# SSH in the Browser: Web-Based Terminal Access Without a Client

Most teams still reach servers the old-fashioned way: install an SSH client, distribute keys, maybe route through a bastion, and hope the person connecting has the right config on their laptop. That works fine for engineers who live in a terminal. It is less ideal for occasional access, contractors on locked-down machines, or anyone who just needs to run a few commands without setting up tooling first.


Browser-based SSH sidesteps that friction. Instead of opening PuTTY or a local terminal, the user visits a URL, completes authentication, and gets a full interactive shell rendered in the tab. Everything happens in the browser, so there is nothing to install on the user's side.


## What You Get


With Pangolin, you publish SSH as a **public resource** : a URL that renders a real terminal in the browser. Users visit that address, sign in, and land in a session on a host in your network. Depending on configuration, they may enter a second set of credentials (password, key, or platform identity) or proceed directly into the shell.


## Why Tunneling Matters


Private servers are not usually reachable from the public internet. Opening SSH to the world creates an attack surface most teams want to avoid.


Pangolin uses **outbound tunneling** instead. You install a lightweight **site connector** on a machine inside your network (an office LAN, cloud VPC, data center, or home lab). The connector initiates an outbound tunnel to Pangolin and stays connected. When a user opens the SSH URL in their browser, Pangolin routes the session through that tunnel to the target host.


The target machine stays private. You do not need to open inbound firewall ports or assign it a public IP. The site connector only needs outbound internet access, which makes this work from behind NAT and typical corporate firewalls.


The connector does not need to run on every server you want to reach. As long as it sits on the same network and can route to your SSH hosts, one connector can serve many machines. For high availability, install a second connector on the same network as a redundant path. Learn more about[how sites and connectors work](https://docs.pangolin.net/manage/sites/understanding-sites) .


## How a Session Works


1. You assign a URL to the SSH resource in the Pangolin dashboard.
2. The user opens that URL and completes[public resource authentication](https://docs.pangolin.net/manage/resources/public/authentication) .
3. Pangolin renders the terminal and sends traffic through the site connector tunnel to the backend host.


You point the resource at the SSH server you want to reach (host and port).


## Authentication and Identity Providers


Before the terminal loads, Pangolin checks who is connecting. You can require a platform login, connect an[identity provider](https://docs.pangolin.net/manage/identity-providers/add-an-idp) such as Google Workspace or Microsoft Entra ID for SSO, and apply access rules based on user, role, or context (geo-blocking, time windows, and more). These checks happen at the URL layer, before any SSH session begins.


If you already use an IdP for workforce access, the same identity store can gate your SSH URLs. See[public resource authentication](https://docs.pangolin.net/manage/resources/public/authentication) for the full set of options.


## Setup in Brief


Install a[site connector](https://docs.pangolin.net/manage/sites/install-site) on a host that can reach your network, create the SSH public resource, and assign it a URL. Pangolin can run sessions directly on the connector host or route to another SSH server on the network. Configuration options for both paths are covered in the[how to SSH with Pangolin guide](https://pangolin.net/news/how-to-ssh-with-pangolin) and the[SSH access docs](https://docs.pangolin.net/manage/ssh) .


## Practical Notes


Browser-based SSH uses the same identity and policy framework as other public resources, so you manage access consistently across web apps, terminals, and remote desktops.


It also works on phones and tablets. A web-based terminal in mobile Safari or Chrome is enough to check logs, restart a service, or run` htop` from the couch. A dedicated SSH app is not required.


If you have been evaluating standalone web gateways for terminal access, Pangolin covers similar ground as an[Apache Guacamole alternative](https://docs.pangolin.net/manage/resources/public/ssh) : in-browser SSH with outbound tunneling and identity-aware access rules, rather than a separate gateway to deploy and maintain.


## FAQ


**What is browser-based SSH?**


*Browser-based SSH renders a full interactive terminal in a web browser. Instead of installing an SSH client, users visit a URL, authenticate, and get a shell session on a remote host. The session is proxied through an outbound tunnel from a site connector on your network.*


**Do I need an SSH client like PuTTY or OpenSSH to connect through the browser?**


*Users only need a modern web browser. Pangolin renders a full interactive terminal at a public URL after authentication. The SSH session is proxied through a site connector to the backend host.*


**Do I need to open SSH ports on my firewall for browser-based access?**


*The SSH host stays on your private network. A site connector inside that network maintains an outbound tunnel to Pangolin, so you do not need to expose port 22 to the internet or assign the server a public IP.*


**How does authentication work for browser-based SSH?**


*Access is checked in two stages depending on configuration. First, the user passes Pangolin authentication at the URL: platform login, SSO through an[identity provider](https://docs.pangolin.net/manage/identity-providers/add-an-idp) , or other[access rules](https://docs.pangolin.net/manage/resources/public/authentication) . Second, they may enter SSH credentials (password, key, or platform identity) before the terminal loads.*


**Can one site connector reach multiple SSH servers?**


*Yes. The site connector only needs to sit on the same network as the servers you want to reach. One connector can serve many hosts. Install a second connector on the same network for a redundant path.*


**Can I use browser-based SSH on a phone or tablet?**


*Yes. Any modern mobile browser can render the terminal. This is useful for quick checks like reading logs, restarting a service, or running monitoring tools from a phone.*


**Is Pangolin open source?**


*Yes. Pangolin is open source and self-hostable. You can run the full platform yourself, use[Pangolin Cloud](https://app.pangolin.net/auth/signup) , or combine a hosted control plane with self-hosted components. Browser-based SSH is available in Pangolin Cloud and Enterprise Edition.*


**What is a good Apache Guacamole alternative for browser-based SSH?**


*Apache Guacamole provides in-browser access to SSH, RDP, and VNC through a standalone gateway. Pangolin offers similar clientless terminal access as part of a broader remote access platform, with outbound tunneling through site connectors, identity provider integration, and unified management alongside web apps and other protocols.*


## See Also


- [How to SSH with Pangolin](https://pangolin.net/news/how-to-ssh-with-pangolin) : browser and private CLI access, PAM provisioning, and getting started
- [RDP in the Browser](https://pangolin.net/news/browser-based-rdp-remote-access) : remote desktop without installing a client
- [VNC in the Browser](https://pangolin.net/news/browser-based-vnc-remote-access) : remote display access without a viewer
- [How Pangolin Works](https://pangolin.net/news/how-pangolin-works) : architecture overview for sites, resources, and policy
- [Pangolin 1.19 release](https://pangolin.net/news/1-19-release) : where browser-based SSH, RDP, and VNC shipped
- [SSH public resource documentation](https://docs.pangolin.net/manage/resources/public/ssh)
