---
schema_version: "1.0.0"
document_id: "18fbd646c835157160a1b6451ab36963c112dd5cf2a706b24c57e274d5499e73"
company_key: "yc-firezone"
company: "Firezone"
source_id: "yc-firezone-news-import-75fbee21a5d6"
canonical_url: "https://www.firezone.dev/blog/mar-2024-update"
published_at: "2024-03-01T00:00:00+00:00"
first_seen_at: "2026-07-21T20:06:48.154812+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:892bddc748f9afcdf460725a26e9e48b4d74b31fe540b2375c32f8bf4b943274"
---

# March 2024 Update

*This is the second post of our semi-monthly product newsletter aimed at providing regular updates on the Firezone product.[Subscribe to future updates](https://www.firezone.dev/product/newsletter)* .


Another month, another product update! We've got a lot to cover in this update, so let's dive right in.


## In this update


This update sees the release of Firezone[1.0.0-pre.9](https://github.com/firezone/firezone/releases/tag/1.0.0-pre.9) , containing dozens of bug fixes, improvements, and a few new features. Here's a summary of what's new:


- TheWindows andLinux clients are now available for beta testing.
- Directory sync isnow available for Microsoft Entra ID and Okta.
- An all-newconnectivity engine that establishes faster and more reliable connections.


Continue reading below for more details.


### Windows Client beta


The Firezone Windows client is now available for beta testing!


You'll need Windows 10 or higher and an x86-64 CPU to run the client.[See the docs](https://www.firezone.dev/kb/install/windows) for more information and download links, or use[this direct link](https://www.firezone.dev/dl/firezone-client-gui-windows/latest/x86_64) to get started right away.


**Note** : Be sure to click **Allow** when prompted by the User Account Control dialog. If that dialog does not appear, you may need to manually run Firezone as an administrator by right-clicking the Firezone icon and select **Run as administrator** .


### Linux Client beta


The Firezone Linux client is also available for beta testing!


This initial release operates exclusively in headless mode, which means you'll need a[Service Account](https://www.firezone.dev/kb/concepts/actors#service-accounts) token to authenticate it.


[Read the docs](https://www.firezone.dev/kb/install/linux) for more instructions and download links for your architecture.


Here's an quick example of how to connect to Firezone with it:


```text
# sudo is needed to manage DNS and open a tun interface
sudo   ./firezone-linux-x64 --token <your-service-account-token>


```


Interested in the Linux GUI client? It's coming! Track its progress on our[public roadmap](https://github.com/orgs/firezone/projects/9/views/1?pane=issue&itemId=44218273) . The GUI client will allow regular users to authenticate instead of requiring a Service Account to do so.


### Directory sync for Entra ID and Okta


We've also added support for directory sync with Microsoft Entra ID and Okta.


Similar to the existing Google Workspace directory sync, this allows businesses to automatically sync their users and groups from Entra ID and Okta into Firezone, making it easier to manage policies to control access to resources.


How it works:


1. Every few minutes, Firezone requests user and group information from Entra ID and Okta using their respective identity APIs.
2. New users and groups are automatically added to Firezone, existing users and groups will be updated with the latest information, and deleted users and groups will be **disabled** in Firezone *but not deleted* (to preserve historical access logs).
3. Any affected policies are updated immediately to reflect the changes.


This means whenever a user is deleted or removed from a Group, their access to affected resources in Firezone is automatically revoked within a few minutes.


#### What about nested groups?


Have a nested org structure? We've got you covered -- Firezone handles that too.


Let's say you had the following group membership structure in your identity provider:


```text
Everyone:
-    steve@company.com
Support:
-    patrick@company.com
Engineering:
-    bob@company.com
-    alice@company.com
Devops:
-    john@company.com


```


You would then see the following group memberships in Firezone after a directory sync:


```text
Group:Everyone:
-    steve@company.com
-    patrick@company.com
-    bob@company.com
-    alice@company.com
-    john@company.com
Group:Engineering:
-    bob@company.com
-    alice@company.com
-    john@company.com
Group:Support:
-    patrick@company.com
Group:DevOps:
-    john@company.com


```


By syncing nested group memberships (also known as *transitive memberships* ), Firezone allows you to assign broad policies that include all of the members of a group, not only the direct ones, just like you'd expect. This means fewer groups are needed to define your access controls, and fewer policies you need to manage.


[Read more](https://www.firezone.dev/kb/directory-sync) about how directory sync works in Firezone.


### New connectivity engine


We've also overhauled the connectivity engine that powers all NAT traversal in Firezone to establish connections faster and more reliably.


As you may recall, Firezone 1.0 features automatic NAT holepunching, which means you don't need to open any ports on your firewall to use Firezone. This is achieved by implementing a collection of industry-standard techniques known collectively as[ICE](https://www.rfc-editor.org/info/rfc8445/) . Details of how ICE works are beyond the scope of this update, but the important thing to know is that it's a battle-tested method for establishing peer-to-peer connections in even the most challenging network environments.


Our first implementation, while functional, suffered from several architectural issues that made it difficult to maintain and extend. Our new implementation, aptly named "[snownet](https://github.com/firezone/firezone/tree/main/rust/libs/connlib/snownet) " (sorry, we couldn't resist), is a ground-up rewrite that addresses these issues and provides a solid foundation for future improvements.


In most cases, initial connections to resources are now established in about a second or less. Once the connection is established, Firezone gets out of the way so your Client and Gateway talk directly to each other with no overhead.


Best of all, this works even if your Client and Gateway are in the same LAN -- Firezone figures out the best way to connect automatically. No need to "disable your VPN" when you come into the office, or muck around with routes when you leave. Firezone just works.


And if a connection fails, Firezone will automatically establish a new one within a few seconds. This all happens behind the scenes, without any user intervention.


### Conclusion


Like what you see and want to give Firezone a try?[Sign up now](https://app.firezone.dev/sign_up) and get started with up to 6 users for free.


Want to see Firezone in action?[Request a demo](https://www.firezone.dev/contact/sales) if you'd like a first-hand look at how Firezone can help your organization.


That's all for this update!
