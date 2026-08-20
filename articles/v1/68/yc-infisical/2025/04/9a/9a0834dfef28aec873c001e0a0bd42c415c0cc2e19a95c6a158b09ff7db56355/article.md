---
schema_version: "1.0.0"
document_id: "9a0834dfef28aec873c001e0a0bd42c415c0cc2e19a95c6a158b09ff7db56355"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/ssh-keys-dont-scale"
published_at: "2025-04-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:4d29b8c404d7c9c380386484d1e9514c9f4ff04fbce500f9b149917a86015c32"
---

# SSH Keys Don't Scale. SSH Certificates Do

SSH access is ubiquitous. It's how engineers, scripts, and platforms across the world remotely administer Linux systems. Whether you're running a small development server or managing a global fleet, you almost certainly rely on SSH in some form or another.


Traditionally, most teams use SSH public key authentication by creating long-lived key pairs, distributing public keys to host machines, and managing user access through manual distribution and cleanup. While this pattern works for smaller deployments, it fails to scale with complex infrastructure as more people need access to different machines. What feels simple quickly turns into mounting operational and security risk: key management sprawl, unclear access boundaries, and brittle tooling that's difficult to maintain.


Fortunately, it's possible to set up a more scalable approach to SSH using SSH certificate-based authentication, something that's already common at companies like Meta, Uber, Google, and more. Hopefully, this makes for an interesting read and is useful for others who may find themselves needing to streamline SSH access one day on a larger scale.


## Why not SSH public key authentication?


Like it or not, SSH public key authentication gets tricky the moment you introduce more than a handful of users and hosts into the equation. What starts as generating, installing, and tracking a few SSH key pairs across users and machines quickly becomes an operational nightmare with dire security consequences. More often than not, organizations end up dealing with key sprawl when dozens, if not hundreds, of keys become scattered across infrastructure.


Without a centralized approach to managing keys, it becomes difficult for administrators to answer questions around observability and security like who has and should have access to what. Even once those questions are answered, managing access like provisioning and de-provisioning users to select machines becomes another hassle to deal with.


I elaborate more on these challenges below:


- Since SSH public key authentication requires users to have SSH keys and for each host to be registered with the public keys of any users that should be allowed access to the host, SSH key management becomes an operational problem of how to efficiently and securely streamline key generation, distribution, and installation across users and hosts. In the best case, teams write runbooks, scripts, and adopt brittle tooling to streamline the process but, more often than not, it ends up evolving into significant operational overhead when onboarding or offboarding any users from the system.
- Beyond key management logistics, key sprawl also introduces complexities around observability, particularly when answering questions around which users have access to which hosts, especially in the absence of a central control plane. As a result, access is often over-provisioned without proper oversight, with former employees and contractors retaining lingering access simply because no one knew to remove their keys from every host.
- Finally, SSH public key authentication encourages poor security hygiene when it comes to secure key distribution and key reuse. More specifically, engineers end up copying SSH keys from one device to another, sending them over insecure channels, and reusing them across environments as a form of convenience. Here, key rotation seldom happens, increasing the blast radius of a lost or stolen key that retains permanent access to hosts until removed.


Amongst a many more reasons, I hope it's clear that SSH key-based authentication just doesn't scale from an operational or security standpoint and that a better model is needed that blends centralized management, observability, and considers security with minimal implementation overhead.


Enter, SSH certificate-based authentication.


## What is an SSH certificate?


An SSH certificate is a short-lived, cryptographically signed document that proves a user or host's identity for the purpose of SSH authentication. Unlike SSH public key authentication where static public keys are distributed across infrastructure, SSH certificate-based authentication introduces a centralized trust model where designated certificate authorities (CA) sign a user and host public keys to produce a short-lived SSH certificates vouching for their identity - We'll explain this further in a bit.


In SSH certificate-based authentication, instead of placing individual user keys on every server, you configure hosts to trust the users' CA. From there, any user with a valid certificate signed by that CA can gain access, subject to whatever rules you define. SSH certificates come with built-in expiration, identity information (like principals or roles), and can be revoked or reissued at any time, making them far more flexible and secure than long-lived SSH keys.


*Read also:[SSH Certificates: A Complete Guide](https://infisical.com/blog/ssh-certificates-guide)*


## Why SSH certificate-based authentication?


The beauty of SSH certificate-based authentication is that it can used as a building block toward a more desirable SSH experience that we believe everyone should adopt.


With some upfront engineering effort, it's possible to set up a workflow and dedicated service powered by SSH certificate-based authentication to issue users short-lived SSH certificates that can be used to access hosts upon request. The advantage of this approach is that you no longer need to manage static, long-lived SSH keys because they're replaced with short-lived SSH certificates that are issued upon request for those who are recognized by the service. Here, the access control model shifts from a decentralized one to a centralized one where the operational burden of provisioning and de-provisioning users, hosts, and access between can be done through the dedicated service with now full access control visibility over all users and their access to infrastructure.


In our view, this is how modern, scalable SSH access should work: centralized, auditable, and built around short-lived credentials.


## How does SSH certificate-based authentication work?


As mentioned, implementing an SSH certificate-based authentication scheme does require some upfront engineering effort, configuration for each user and host in the system, and understanding of underlying cryptographic primitives and concepts involved. Given the learning curve, we discuss how SSH certificate-based authentication works and how you can set it up yourself.


Foremost, it's important to understand that SSH certificate-based authentication relies on SSH key pairs (usually Ed25519) with some key pairs designated as certificate authorities (CAs) to sign certificates and others generated by users and hosts to receive certificates to be used as part of subsequent SSH connection; the difference though is that the key pairs require little to no management once the upfront work to set up the system is complete.


In a typical setup, an administrator may set up two CAs as part of the dedicated service where one is responsible for issuing user certificates while the other for signing host certificates. Each user is pre-configured to trust certificates issued for the hosts; conversely, each host is also set up to trust certificates issued to the users. This mutual trust forms the foundation of the SSH certificate-based authentication model.


To ensure that users only connect to trusted hosts, each user's machine is configured to trust the host CA. This is done by adding the host CA's public key to the known_hosts file using a special @cert-authority marker.


For example:


```text
@cert-authority *.example.com ssh-ed25519 AAAAC3Nza...


```


This line instructs the SSH client on the user's side to trust host certificates issued by the host CA for any host under *.example.com .


To ensure that hosts only allow access for users with valid SSH certificates and restrict logins to intended Unix accounts, a few key changes must be made on each host.


- To validate incoming SSH certificates, the public key of the trusted user CA must be installed onto each host and the sshd_config file should be updated to include the TrustedUserCAKeys configuration directive such as` TrustedUserCAKeys /path/to/user-ca.pub` .
- To control what users can log in as, each host must also be configured with one or more authorized_principal files, specifying which principals (i.e. identities like usernames, roles, or group names) included on incoming SSH certificates are allowed to log in under which local Unix user accounts (e.g. ec2-user, root , etc.). This lets administrators define who can access what, independent of static SSH keys.


All in all, once both users and hosts are configured, a secure SSH certificate-based connection is ready to be established.


Going forward, the typical user workflow for SSH-ing looks like:


- When a user wants to access a host, they make an authorized request to the dedicated service, containing the user CA, to obtain an SSH certificate. The CA verifies the user's identity and issues a short-lived certificate that includes one or more principals that represent the user's identity.
- Once the certificate is issued, it's loaded into the user's SSH agent, a background process that securely holds private keys and certificates in memory so the user doesn't have to manually provide credentials for every connection. Next, the user initiates an SSH connection from their local machine with something like` ssh ec2-user@host.example.com` .
- On the host, the SSH daemon, a background process that handles SSH connections, verifies that the user's certificate was signed by the trusted user CA (as configured via the TrustedUserCAKeys directive). It then looks up the authorized_principals file associated with the login user (e.g. ec2-user ) to check whether any of the principals listed in the user's certificate are allowed to log in under that account.
- Back on the user's machine, the SSH client verifies the identity of the host by checking the host's SSH certificate against the trusted host CA stored in the user's known_hosts file, protecting against man-in the middle attacks by ensuring the host is legitimate.


If both checks on the user's machine and on the host pass, then the certificate is valid and access is granted.


Simple enough, right?


In all seriousness, at this point, you're probably wondering if setting up SSH certificate-based authentication is worth all the engineering hassle. Between running your own CAs, configuring users and hosts to trust them, managing principal permissions, and wiring all of it into your existing infrastructure; the setup for a scalable SSH solution can get complicated fast.


## Enter, Infisical SSH


[Infisical SSH](https://infisical.com/docs/documentation/platform/ssh) , an extension of the Infisical platform, gives you a secure, scalable control plane to enable SSH certificate-based access for your team and infrastructure.


With Infisical SSH, we've worked hard to abstract away all the operational complexity required to set up SSH certificated-based access, from running a CA to issuing certs and configuring trust, so you don't have to stitch it all together yourself. We've simplified the setup so much that all you have to care about is registering users and hosts onto the platform and specifying who should have access to what.


The general workflow consists of three steps:


1. Registering a host with Infisical using the Infisical CLI via the` infisical ssh add-host` command.
2. Configuring Infisical to grant users time-bound access to the hosts through Infisical's role-based access control framework.
3. With that, users need only to use the Infisical CLI to run the` infisical ssh connect` command on their machines and select which host they want to connect to initiate an SSH connection.


Under the hood, Infisical helps set up a two-CA scheme, perform the required trust configuration on the host and user, and issue and load an SSH certificate into the SSH agent to be used in the SSH connection; our tools work together with OpenSSH to streamline this complex workflow so administrators can manage and audit SSH access through Infisical and users can SSH into hosts with one CLI command.


For more information about Infisical SSH, check out the documentation[here](https://infisical.com/docs/documentation/platform/ssh) .


## Is This the Future of SSH?


SSH certificates are already standard practice at companies like Meta, Uber, and Google - and it's only a matter of time before more teams adopt the model. If you're building for scale, security, or sanity, it's worth considering the switch now before your SSH access becomes unmanageable.


Whether you implement it yourself or use a solution like[Infisical SSH](https://infisical.com/docs/documentation/platform/ssh) , what's clear is this: SSH keys don't scale. SSH certificates do.
