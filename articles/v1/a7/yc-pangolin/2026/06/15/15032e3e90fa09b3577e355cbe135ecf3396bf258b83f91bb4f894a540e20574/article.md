---
schema_version: "1.0.0"
document_id: "15032e3e90fa09b3577e355cbe135ecf3396bf258b83f91bb4f894a540e20574"
company_key: "yc-pangolin"
company: "Pangolin"
source_id: "yc-pangolin-news-import-841387734568"
canonical_url: "https://pangolin.net/news/how-to-ssh-with-pangolin"
published_at: "2026-06-14T00:00:00+00:00"
first_seen_at: "2026-07-22T08:11:44.188163+00:00"
fetched_at: "2026-07-28T21:44:22.050751+00:00"
content_hash: "sha256:62dec5ec4d7a9e51fca49c56d5699f7d0afcc476c2af22baba16f2ab66c927b2"
---

# How to SSH with Pangolin: Browser Access and Private CLI

SSH is still how most teams reach servers, but the workflow around it has not kept up. You install a client, distribute keys, maybe route through a bastion, or connect a VPN that reaches far beyond the one host you need. Pangolin offers a different path: reach private infrastructure without exposing SSH publicly, with identity and policy enforced before anyone gets a shell.


You can connect in two ways. Use a **browser terminal** at a URL for clientless access, or use the **Pangolin CLI** over a peer-to-peer connection for a VPN-style workflow scoped to specific hosts. Both paths use the same site connector and policy model. The difference is how traffic flows and how users authenticate on the way in.


## Two Ways to Connect


In the Browser Private CLI (VPN-Style)


Who it's for Contractors, on-call from any device, locked-down laptops Engineers who live in a terminal


How users connect Visit a URL, sign in, get a web terminal Install the Pangolin client, run` pangolin ssh <alias>`


Client required Web browser only Pangolin client and CLI


Access layer Login page, SSO, and access rules at the URL Identity from the peer-to-peer connection plus resource policy


**In the browser:** Pangolin renders a full interactive terminal at a public URL. Users complete authentication in the tab and land in a session on a host in your network. See the[public SSH documentation](https://docs.pangolin.net/manage/resources/public/ssh) for configuration details.


**Private CLI:** Users connect with the Pangolin client, then run` pangolin ssh <alias>` to open a session over a peer-to-peer WireGuard path to the site. Access is scoped to authorized resources rather than the whole network. See the[private SSH documentation](https://docs.pangolin.net/manage/resources/private/ssh) .


## How Pangolin Reaches Your Servers


Private servers are usually unreachable from the public internet. Opening SSH to the world creates an attack surface most teams want to avoid. Pangolin keeps hosts on your private network and uses two different connectivity models depending on how users connect.


**Public resources (browser SSH)** use **outbound tunneling** . You install a lightweight **site connector** on a machine inside your network. The connector initiates an outbound tunnel to Pangolin and stays connected. When a user opens a browser terminal, Pangolin routes the session through that tunnel to the target host. The connector only needs outbound internet access, which makes this work from behind NAT and typical corporate firewalls.


**Private resources (CLI SSH)** use a **peer-to-peer connection** . The user runs the Pangolin client, which establishes a direct WireGuard path to the site connector on the network. Pangolin[punches through NATs and firewalls](https://pangolin.net/news/nat-holepunching) to build that link without opening inbound ports. SSH traffic travels over the peer-to-peer path to authorized hosts. It works like a scoped VPN: users reach only the resources they are allowed to access, not the full corporate network.


In both cases, the target machine stays private. You do not need to open inbound firewall ports on the SSH host or assign it a public IP.


One connector on a network can reach many SSH hosts, as long as it can route to them. For high availability, install a second connector on the same network as a redundant path.


## Getting Started: The Default Path


Pangolin 1.19 made SSH dramatically easier to set up. The dashboard now defaults to **Pangolin SSH** mode with **manual authentication** , which works out of the box for most teams.


1. Create a[Pangolin Cloud](https://app.pangolin.net/auth/signup) account or use your self-hosted control plane.
2. [Install a site connector](https://docs.pangolin.net/manage/sites/install-site) on the machine you want to reach. For Pangolin SSH mode, run the connector as root on that host.
3. Create an SSH resource in the dashboard. Choose a **public resource** for browser access or a **private resource** for CLI access.
4. Connect. For browser access, visit the resource URL. For CLI access, run` pangolin up` and then` pangolin ssh <alias>` .


With these defaults, users authenticate with credentials that already exist on the host. There is nothing to change in` sshd_config` and nothing extra to install beyond the site connector.


For private CLI access, the workflow looks like this:


```text
pangolin ssh prod-app.internal


```


You can also copy files over the same peer-to-peer connection:


```text
pangolin scp ./config.yml prod-app.internal:/etc/app/


```


## Automatic User Provisioning with PAM


Manual credentials work for many setups, but teams managing dozens of servers often want something better: access tied to organization identity, with accounts created on demand instead of pre-staged on every machine.


When you enable **automated provisioning** , Pangolin creates or updates the Linux user account just before the session starts. Your organization identity maps to a local username, derived from the part of your email before the` @` . If that name is already taken, Pangolin adds a numeric suffix until it is unique. Roles control sudo level, Unix groups, and home directory setup, so permissions follow the same lifecycle as your workforce identity.


This happens automatically through **PAM** (Privileged Access Management). PAM is the mechanism that **pushes the user onto the host at login time** . Pangolin provisions the account, home directory, and group membership before the shell opens. You skip manual account creation across a fleet of servers.


With **Pangolin SSH** and automated provisioning, this works without reconfiguring OpenSSH. Run the site connector as root and switch authentication to automated provisioning in the dashboard.


With **Standard SSH Server** mode, which routes to an existing OpenSSH host on the network, provisioning still uses PAM but requires additional host setup. That path is common when the site connector sits on a bastion and reaches multiple servers behind it. The[SSH access documentation](https://docs.pangolin.net/manage/ssh) covers each configuration combination.


For Standard SSH Server mode with automated provisioning, Pangolin also issues **short-lived SSH certificates** valid for five minutes. That window is enough to establish the session, but short enough that a compromised credential has limited value. Once the session is open, it can stay connected. Access revocation happens through Pangolin policy rather than hunting down static keys on individual servers.


## Browser SSH in Practice


Browser SSH is built for people who need shell access without installing anything. A contractor on a managed laptop, an operator checking logs from a phone, or an admin running a quick command from a machine where they cannot install PuTTY or OpenSSH.


The user visits the resource URL, passes Pangolin authentication (platform login, SSO, or access rules), and may enter a second set of host credentials depending on configuration. Pangolin renders a full terminal in the tab and routes the session through the site connector's outbound tunnel.


For a deeper look at browser-based terminal access, including mobile use and identity provider integration, see[SSH in the Browser](https://pangolin.net/news/browser-based-ssh-remote-access) .


## Private CLI in Practice


Private CLI access works like a scoped VPN for SSH. The user installs the Pangolin client, connects with` pangolin up` , and establishes a peer-to-peer WireGuard path to the site. From there, they reach only the hosts they are authorized to access. SSH traffic travels over that direct connection, not across the full corporate network.


This fits engineers who prefer a local terminal, use SSH daily, and want file transfer with` pangolin scp` . The Pangolin desktop app can provide the tunnel while the CLI handles the SSH session itself.


Grant access through resource policy and ensure TCP 22 is allowed in port restrictions for private resources. Pangolin checks identity from the active client connection before the session starts.


## When You Need the Advanced Path


**Standard SSH Server** mode is there when Pangolin SSH mode is not the right fit. Use it to reach an existing OpenSSH host elsewhere on the network, connect to legacy infrastructure that already runs its own SSH server, or set up a bastion pattern where one site connector reaches many target machines.


That path can involve certificate authorities, auth daemon placement, and` sshd_config` changes on target hosts. It is powerful for multi-server environments, but it is also more setup than the default. The[SSH access documentation](https://docs.pangolin.net/manage/ssh) walks through each valid configuration.


## Why Teams Choose This Over Traditional SSH


- **Identity-tied access.** Permissions follow organization identity and resource policy, not scattered static keys that outlive the people who issued them.
- **Scoped connectivity.** Browser SSH routes through an outbound tunnel; private CLI connects peer-to-peer to the site. Users reach specific authorized hosts instead of inheriting broad network access from a VPN.
- **Just-in-time users.** PAM pushes accounts onto hosts at login time, so you are not pre-provisioning users across every server in the fleet.
- **Browser option for occasional access.** Engineers use the CLI; everyone else gets a URL and a terminal in the tab.


For the broader case for modernizing SSH across an organization, see[Modernizing Enterprise SSH Access](https://pangolin.net/news/modernizing-enterprise-ssh-access) .


## FAQ


**How do I SSH with Pangolin?**


*Install a site connector on a host inside your network, create an SSH resource in the Pangolin dashboard, and connect. For browser access, visit the resource URL and sign in. For CLI access, connect with the Pangolin client and run` pangolin ssh <alias>` .*


**Can I SSH from a browser without installing an SSH client?**


*Yes. Pangolin renders a full interactive terminal at a public URL. Users visit the address, complete authentication, and get a shell session on a remote host. Only a modern web browser is required on the user's machine.*


**Do I need a VPN to SSH with Pangolin?**


*A traditional VPN is not required. Public SSH routes through an outbound tunnel from a site connector inside your network. Private CLI access uses the Pangolin client to establish a peer-to-peer WireGuard path to the site, scoped to authorized hosts rather than the full network.*


**What is the difference between public and private SSH in Pangolin?**


*Public SSH renders a terminal in the browser at a URL and routes traffic through the site connector's outbound tunnel. Users authenticate at the URL layer with login, SSO, or access rules. Private SSH uses the Pangolin CLI over a peer-to-peer WireGuard connection: users run` pangolin ssh <alias>` after connecting with the Pangolin client. Both use the same SSH configuration in the dashboard; the difference is connectivity model, how users connect, and which authentication layer gates access first.*


**How does Pangolin automatically create users on Linux servers?**


*When automated provisioning is enabled, Pangolin maps your organization identity to a local Linux username and creates or updates the account just before the session starts. Roles control sudo level, Unix groups, and home directory setup. With Pangolin SSH mode, this works through the site connector without reconfiguring OpenSSH.*


**What is PAM in Pangolin SSH?**


*PAM stands for Privileged Access Management. Pangolin uses PAM to push users onto the host at login time: creating the account, home directory, and group membership before the shell opens. This is how just-in-time user provisioning works across your fleet.*


**Do I need to open port 22 on my firewall?**


*For browser SSH, a site connector maintains an outbound tunnel to Pangolin, so you do not need to expose port 22 to the internet or assign the server a public IP. For private CLI access, the Pangolin client connects peer-to-peer to the site over WireGuard. TCP 22 must be allowed in the resource's port restrictions so SSH can reach the target host over that path.*


**Do I need to edit sshd_config to use Pangolin SSH?**


*Not for the default setup. Pangolin SSH mode with manual authentication works without OpenSSH reconfiguration. If you use Standard SSH Server mode with automated provisioning, the target host's SSH server needs additional configuration. See the[SSH access documentation](https://docs.pangolin.net/manage/ssh) for that path.*


**How long are Pangolin SSH credentials valid?**


*In Standard SSH Server mode with automated provisioning, SSH certificates are valid for five minutes from the moment they are issued. That is enough time to establish the connection. Once the session is open, it can stay connected. Pangolin SSH mode uses its own authentication layer rather than short-lived SSH certificates.*


**Can I copy files over Pangolin SSH?**


*Yes. The Pangolin CLI supports` pangolin scp` for copying files over the same peer-to-peer connection used by` pangolin ssh` . For example:` pangolin scp ./config.yml prod-app.internal:/etc/app/` .*


## See Also


- [How Pangolin Punches Through NATs and Firewalls](https://pangolin.net/news/nat-holepunching) : how peer-to-peer connections work across NAT and firewalls
- [SSH in the Browser](https://pangolin.net/news/browser-based-ssh-remote-access) : web-based terminal access without a client
- [Pangolin 1.19 release](https://pangolin.net/news/1-19-release) : where browser SSH and simplified Pangolin SSH mode shipped
- [How Pangolin Works](https://pangolin.net/news/how-pangolin-works) : architecture overview for sites, resources, and policy
- [SSH access documentation](https://docs.pangolin.net/manage/ssh) : full configuration reference
