---
schema_version: "1.0.0"
document_id: "57549f15f3da4867adbc70af08b2f803100f2482f1a3062cd49e4ae4f212280c"
company_key: "yc-infisical"
company: "Infisical"
source_id: "yc-infisical-rss-e9c2aac341e3"
canonical_url: "https://infisical.com/blog/ssh-certificates-guide"
published_at: "2025-03-10T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:31.088886+00:00"
fetched_at: "2026-07-28T20:58:14.920102+00:00"
content_hash: "sha256:8115f9f43454f130b291f2fa19cdeaa03fe16328d5bef762eede2abd2a6112d6"
---

# SSH Certificates: A Complete Guide

When it comes to securing access to your Linux servers, the choice of SSH authentication method can make a world of difference. With options ranging from simple passwords to complex certificate-based systems, it's essential to understand the tradeoffs between convenience and security. In this blog post, we'll embark on a journey through the three most common SSH authentication methods: **passwords** , **SSH keys** , and **SSH certificates** .


As we explore each method, we'll discuss their unique strengths and weaknesses, shedding light on the challenges that come with balancing simplicity and robustness. However, our ultimate goal is to convince you that **SSH certificates** are the superior choice for managing access to your Linux servers.


But we won't just leave you with theoretical knowledge. We'll also introduce you to Infisical, a powerful tool that simplifies the process of setting up and managing SSH certificates. By the end of this post, you'll not only be equipped with a **deep understanding of SSH authentication methods** but also have the tools and knowledge necessary to **implement SSH certificates in your own environment** .


So, whether you're a seasoned system administrator looking to enhance your security practices or a newcomer to the world of SSH, this post will be your guide to unlocking the full potential of SSH authentication. Get ready to discover a better way to secure your Linux servers!


## Types of SSH Authentication


### Password Authentication


Password authentication is the oldest and most common way to authenticate to an SSH server. It's enabled by default, and it uses the existing passwords for the accounts set up on the SSH host. There really isn't much more to say about how this works; if you've used SSH, you've done this before!


Although password authentication is about as easy as it gets for SSH credentials, it's also **the most insecure way to protect your SSH server** . Passwords are long-lived credentials, which means once your password is leaked, it can be abused until you notice or change your password. How easy is it for a password to be compromised, though?


Unfortunately, **passwords are hard to secure** . Passwords are extremely prone to re-use and typically resemble words or phrases related to the user's life. This makes passwords both **guessable** and **discoverable** . Additinally, people generally do a bad job of rotating passwords on a regular basis. It's hard to remember new passwords all the time, especially if they're complex enough to be secure.


Despite its risks, password authentication will continue to be the standard until there is an alternative that is **at least** as easy to use and set up. As you'll see in the coming sections, it does take a little work to get away from passwords. Before moving on, let's review the pros and cons of password-based authentication for SSH:


Pros:


- zero configuration on the server
- zero files on the client
- simple and intuitive


Cons:


- vulnerable to brute-force attacks
- difficult to manage password complexity and rotation
- susceptible to password reuse with related and unrelated systems
- passwords are long-lived and tend to work for a very long time


### SSH Keys


The next most common way to access an SSH server is by using public key authentication, commonly referred to as SSH key auth. SSH keys fix the "guessability" problem that passwords have by using a **cryptographically unguessable private key** as your SSH credential. From a usability perspective, an SSH key is just a file that you pass as an argument to` ssh` or add to your[SSH agent](https://www.ssh.com/academy/ssh/agent) .


SSH keys are pretty easy to set up. All you have to do is to copy your SSH key's corresponding public key to the` ~/.ssh/authorized_keys` file on the server you want to access. You'll have to do this on every server you want to access, though, which leads us to **the biggest challenge of using SSH keys: rotating them** .


If your SSH private key gets stolen or leaked, you have to log into every server that SSH key can access and remove the public key from each` authorized_keys` file. Not only is this time consuming, it's also hard to keep track of every place your SSH key is authorized to log into. The odds that something gets missed are pretty high, making a leaked SSH key a very big deal.


One thing you can do to mitigate the risk of a leaked SSH key is to protect your keys with a password. You might see the irony in that, though. Now, we have another password we have to remember, which brings back the usability overhead we were trying to get away from with password authentication.


Luckily, there are tools that make the usability and setup of SSH keys a lot easier. For example, password managers like KeePassXC integrate with your SSH agent, importing all of your SSH keys when you unlock your protected vault.


On the setup side of things, some Linux server distros let you import your SSH public keys from your GitHub when you install the server. This means you don't have to use password authentication to edit your` authorized_keys` file -- you can just start using your SSH key right away!


By taking advantage of tools like the ones mentioned above, you can use **SSH keys as a more secure replacement for password authentication** in most situations. Despite their ease of setup and usability, SSH keys are long-lived and difficult to rotate... We can still do a lot better. Check out the summary below to review the pros and cons of SSH keys before moving on:


Pros:


- prevents brute-force attacks
- relatively simple to set up
- existing tools import public keys and integrate with the SSH agent


Cons:


- need to manually add your public key to each SSH server
- SSH private key file needs to be protected and available
- still requires a password on the private key if you want to protect against theft
- SSH keys are long-lived and difficult to rotate


### SSH Certificates


SSH certificates address the challenges of deploying and rotating SSH credentials at scale. This is how SSH certificates work at a high level:


- A Linux server trusts an SSH Certificate Authority (CA) to grant access to the server over SSH
- The SSH CA is a single location where access is configured for many Linux servers
- Clients ask the SSH CA for access to a Linux server, and the SSH CA decides whether that client is authorized
- If a client is authorized, a short-lived SSH certificate is issued to the client for time-bound access to the Linux server


By centrally managing SSH access to multiple Linux servers, SSH CAs make it easy to grant **just-in-time access** to SSH servers and **revoke access for certain clients** if needed. The only downside to SSH certificate authentication is that the process of setting up the SSH CA and configuring access is much more complex up front than it is for other types of SSH credentials.


After a brief review of these pros and cons below, we will get into an overall comparison between the different types of SSH credentials and show you how SSH certificates work in practice.


Pros:


- better scalability than SSH keys
- revoking access is centrally managed
- SSH certificates are short-lived and time-bound


Cons:


- requires SSH CA to be set up and accessible
- client authorization is more complex


## Comparing Authentication Methods


Password authentication works out of the box for SSH servers, but it really should be avoided when possible. Passwords are just too weak and too prone to bad practices to provide secure access to your Linux servers. SSH keys are a better alternative to passwords that are still pretty easy to set up and use. They provide better security, but unfortunately, that security just doesn't scale well. If you manage many servers and need to revoke keys, SSH keys become very difficult to handle.


**In the end, SSH certificates offer the best security and scalability. **These improvements come at the cost of some added complexity, but with the right tools to manage your SSH certificates, it's no big deal.


The most difficult parts of certificate-based SSH authentication are setting up the CA and authorizing client. Luckily, Infisical has made these things easy for you. In the next section, we'll explore how you can utilize Infisical to manage your SSH certificates.


## Using Infisical for SSH Certificate Management


[Infisical SSH](https://infisical.com/docs/documentation/platform/ssh) manages access to Linux servers by handling three key components of SSH certificate authentication:


**[SSH CA](https://infisical.com/docs/documentation/platform/ssh#guide-to-configuring-infisical-ssh)** : Infisical acts as the SSH Certificate Authority that is trusted to issue SSH certificates to clients


**[Certificate Templates](https://infisical.com/docs/documentation/platform/pki/certificates)** : Templates are pre-configured access policies that define which users can access which systems and for how long


**SSH Certificates** : Clients can authenticate to Infisical to request an SSH certificate from one of the pre-configured templates


Together, these three components provide secure and centrally managed access to Linux servers. There are a few steps involved in setting them up, but once they are in place, using them is a breeze.


### Trusting an Infisical SSH CA on the SSH server


For SSH certificate authentication, all of the authorization logic is handled by Infisical. On your Linux servers, all you need to do is configure the SSH daemon to trust your Infisical CA. The first thing you'll need to do is log into Infisical, navigate to the SSH tab, and create a new project if you don't have one already.


In the project, go to the Certificate Authorities tab and create a new SSH CA.


Next, click on the newly created CA to go into the CA details. On the left side where it shows the public key, click on the "Save public key" button to export it to a file.


Once you've got the CA's public key, transfer it to the hosts you want to configure and place it at` /etc/ssh/infisical-ca.pub` . Edit each host's` /etc/ssh/sshd_config` file and add the following lines:


```text
TrustedUserCAKeys /etc/ssh/infisical-ca.pub
PubkeyAcceptedKeyTypes=+ssh-rsa,ssh-rsa-cert-v01@openssh.com


```


Lastly, restart the SSH daemon with` sudo systemctl restart sshd` .


### Creating an SSH certificate template in Infisical


Now that Infisical is trusted to delegate access to the SSH servers, we can create our access policies in the form of **Certificate Templates** . Back in the CA details page, click on the "+" button in the top right of the Certificate Templates box.


Next comes the part that requires the most thinking. You'll need to decide which user accounts and servers a certificate is allowed to be issued for. You also need to configure the lifetime of each certificate, which is the part that prevents stolen or leaked certificates from being reused again and again.


After you save the configuration, your certificate template is ready to use! If you have questions about how to set up an SSH template with Infisical, refer to the[official documentation](https://infisical.com/docs/documentation/platform/ssh) .


### Using SSH certificates to access a server


Now, all that's left is to use a certificate template to access your SSH server! You'll need the[Infisical CLI tool](https://infisical.com/docs/cli/overview) for this. First, log into Infisical with` infisical login` . Then, request an SSH certificate and and load it into the SSH agent:


```text
infisical ssh issue-credentials --certificateTemplateId=<template-id> --principals=<username> --addToAgent`.


```


And that's it! Now you can` ssh username@hostname` and you should be logged in!


The` infisical ssh issue-credentials ...` command is what you will use to get a new certificate each time your current one expires. I recommend setting an alias for this command so you don't need to type it out each time.


## Conclusion


There is no one-size-fits-all approach when it comes to SSH credentials. There will be times when you need to use what works out of the box, and there will be times when you need to have more secure ways to access your Linux servers. Hopefully, this article has provided you with a better understanding of SSH certificates, why they're needed, and how to use them.


As we saw, Infisical makes SSH certificate management much easier to set up and use. If you haven't already set up Infisical in your own homelab or corporate network, check out my blog post on[how to self-host Infisical](https://infisical.com/blog/self-hosting-infisical-homelab) ! Once you have Infisical set up, I hope you come back to this guide and try out SSH certificates.
