---
schema_version: "1.0.0"
document_id: "0fa825dfc30db540a2177755df48720bac56bdb70cf8e7cef35e8b4e04d4b250"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/infisical-gateway"
published_at: "2025-04-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:9b6787a5ae8e4aab6cf139b46ff514d28c9fd996dd42b00826f4d6a89e67b939"
---

# Infisical Gateway: Securely Connect Infisical to Anything, Anywhere

Managed secrets services like Infisical are designed to help you securely store and manage sensitive information without worrying about the underlying infrastructure. However, it's not uncommon to need to connect these services to private resources, like as databases or internal APIs, living within private networks and firewalled off from the public internet — for good (security) reasons.


This creates a dilemma: how do you leverage services like Infisical's full capabilities without punching holes in your firewall?


Traditionally, the answer involved complex solutions: setting up bastion hosts, wrestling with VPN tunnels, or relying on manual, error-prone processes. These approaches not only add operational overhead and complexity, but can even create security gaps.


At Infisical, we believe security and simplicity should go hand-in-hand. That's why we built the **Infisical Gateway** .


## Meet the Infisical Gateway: A Secure Bridge to Private Resources


The Infisical Gateway is a lightweight component you deploy inside your private network. It acts as a secure, outbound-only bridge, allowing your Infisical instance (Cloud or Dedicated) to seamlessly and securely interact with your private resources without ever exposing them to the internet. This method keeps your resources fully protected from external access while enabling Infisical to securely interact with resources like databases.


## How Does It Work?


### Architecture Overview


Here is a high-level overview of the Infisical Gateway architecture:


The two key elements of this architecture are:


-


**the Gateway** : Deployed by the customer in their private network (e.g., EC2 instance, Droplet) using the Infisical CLI (` infisical gateway --identity-token` ). It initiates outbound connections only.


-


**The Relay Server** : a publicly accessible server managed by Infisical which uses shared secrets with the Infisical Platform for authentication, avoiding direct communication needs between the Gateway and the Infisical Cloud. It uses the **TURN protocol** to make the Gateway discoverable by Infisical (can be geographically distributable for performance).


> the TURN Protocol enables peer-to-peer communication where one peer (the Gateway) is behind a NAT/firewall. This protocol eliminates the need to expose an IP address and allows IP restrictions which add a layer of security.


The communication between the Gateway and Infisical Cloud is E2E encrypted using **mTLS** (Mutual TLS) via the **QUIC** protocol.


> the QUIC Protocol is a modern transport protocol (UDP-based but with TCP-like reliability and security, plus TLS encryption). Used for the actual data transfer between Infisical Cloud and the Gateway via the Relay. It was chosen for speed and better handling of connections compared to raw TCP or insecure UDP.


A Certificate Authority (CA), managed within Infisical, issues unique certificates for Gateways (server certs) and Infisical Cloud (client certs) per organization. Both parties present certificates signed internal CAs. This ensures Infisical Cloud is talking to the legitimate Gateway (and vice-versa) and encrypts the traffic so even the Relay server cannot decipher it.


### Booting Up the Gateway


The Gateway is designed to be easy to set up and use. Here’s how it works:


1. Gateway starts in the private network, authenticates with Infisical Cloud using an identity token (HTTPS).
2. Cloud provides temporary credentials and the URL for a Relay Server.
3. Gateway connects outbound to the Relay Server, authenticates
4. Gateway gets an allocated IP:Port on the Relay. It maintains a persistent (UDP/DTLS) connection.
5. Gateway tells Infisical Cloud of its Relay IP:Port address.
6. Cloud issues a unique mTLS server certificate to the Gateway.
7. Gateway starts a QUIC listener on the Relay connection, secured with its mTLS certificate.


## Let's See it in Action: The Case of the Private Database


Imagine "Acme Corp." They use Infisical Cloud to manage secrets for their applications. Their critical Orders Database runs PostgreSQL and holds sensitive customer information. For security and compliance, this database lives in a private VPC, completely inaccessible from the public internet.


Acme Corp wants to use Infisical's dynamic secrets feature to generate short-lived database credentials for their Order Processing Service. How can Infisical Cloud, sitting on the public internet, securely connect to this isolated database to create and manage these credentials?


*Read also:[Dynamic Secrets: Generate Credentials On-Demand](https://infisical.com/blog/from-static-to-dynamic-db-credentials)*


Enter Infisical Gateway:


1.


**2-min Setup** : An engineer at Acme Corp first creates a dedicated machine identity for the Gateway via Organization Settings > Access Control. Then, they install the Infisical CLI on a small instance within the same private VPC as the database. For production deployment, they run:` sudo infisical gateway install --token <identity-token> --domain <infisical-domain>` followed by` sudo systemctl start infisical-gateway` . That's it. The Gateway is running as a secure systemd service.


2.


**Secure Connection** : The Gateway automatically establishes a secure, encrypted outbound connection to Infisical's infrastructure via our Relay servers. Crucially, no inbound ports need to be opened in Acme Corp's firewall. The Gateway ensures that all communication between Infisical and the private resources **is fully end-to-end encrypted** , meaning only the platform and Gateway can decrypt the transmitted information (not the Relay).


3.


**Transparent Ops** : After verifying the Gateway is working in the **Organization Access Control** > **Gateways** tab, Acme Corp links the Gateway to the relevant project. When configuring the dynamic secret in the Infisical dashboard, they simply select their newly registered Gateway. When the Order Processing Service requests a credential, Infisical securely sends the request through the Gateway tunnel directly to the private Orders Database. Infisical can now create the user, grant permissions, and return the temporary credentials – just as if it were running inside the private network.


4.


**Zero management overhead** : Infisical can continue to use this secure channel to automatically rotate credentials or revoke access as needed, all while the database remains safely shielded from the public internet.


Read the documentation:[Gateway - Infisical](https://infisical.com/docs/documentation/platform/gateways/overview#gateway)


## When to Choose the Infisical Gateway?


The Gateway architecture is especially valuable in the following scenarios:


-


When you need to programmatically manage credentials or secrets **across network boundaries** (e.g., multi-cloud or hybrid deployments) without complex network peering arrangements.


-


When you need to adhere to a **zero-inbound security model** . For strict egress-only network policies, the Gateway's outbound-only TURN-based connectivity pattern eliminates the need to expose internal resources or open inbound firewall ports.


-


When you need to **reduce network complexity** : the Gateway provides a lightweight alternative that requires minimal maintenance compared to solutions like VPN tunnels or SSH bastion hosts.


-


When you need **mTLS** . The Gateway implements mutual TLS with a dedicated PKI infrastructure, ensuring both client and server authenticate each other.


-


For **regulatory compliance** in environments where network segmentation and resource isolation are auditable requirements for PCI-DSS, HIPAA, or SOC2.


## Start Connecting


The Infisical Gateway allows you to leverage the full power of Infisical's secret management capabilities across your entire infrastructure – public and private.


Ready to securely connect Infisical to your private resources?


- [Check out the Infisical Gateway Documentation](https://infisical.com/docs/documentation/platform/gateways/overview)
- [Get started with Infisical Cloud](https://infisical.com/sign-up)
- Questions?[Join our Slack community!](https://infisical.com/slack)


Securely managing secrets everywhere just got a whole lot easier.


> Note that Gateway is a paid feature available under the Enterprise Tier for Infisical Cloud users.
> Self-hosted Infisical users should contactsales@infisical.com to purchase an enterprise license.
