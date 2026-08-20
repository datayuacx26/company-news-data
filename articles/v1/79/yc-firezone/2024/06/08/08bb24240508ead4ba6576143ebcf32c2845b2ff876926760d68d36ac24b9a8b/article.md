---
schema_version: "1.0.0"
document_id: "08bb24240508ead4ba6576143ebcf32c2845b2ff876926760d68d36ac24b9a8b"
company_key: "yc-firezone"
company: "Firezone"
source_id: "yc-firezone-news-import-75fbee21a5d6"
canonical_url: "https://www.firezone.dev/blog/jun-2024-update"
published_at: "2024-06-21T00:00:00+00:00"
first_seen_at: "2026-07-24T08:10:10.796715+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:bf34b65cc12863568c1dff97aa55e1f4bbac866492a6a1b6284b574076e8816d"
---

# June 2024 Update

## In this update:


- **New feature:**Conditional access policies
- **Blog post:**[Using Tauri for a cross-platform security app](https://www.firezone.dev/blog/using-tauri)
- **Blog post:**[Improving reliability for DNS Resources](https://www.firezone.dev/blog/improving-reliability-for-dns-resources)
- New[support page](https://www.firezone.dev/support) for getting help with Firezone
- New[changelog](https://www.firezone.dev/changelog) for tracking all releases we ship


### Conditional access policies


[Policies](https://www.firezone.dev/kb/common-workflows) form the basis of Firezone's access model, and in June they got a major upgrade.


In the zero-trust security model, it's all about making access as granular as possible to ensure only the right people can access the right Resources at the right time. Policy conditions continue that theme by allowing you to restrict access to **four** powerful access conditions:


- Client location
- Client IP address
- Identity provider using for authentication
- Time of day


These are evaluated at the time access is attempted by Firezone's[Policy Engine](https://www.firezone.dev/kb/architecture/core-components#policy-engine) . If a Client switches networks, for example, access is re-evaluated immediately based on the new conditions.


Read more about how each one works below.


#### Client location


Use Client location to restrict access to a Resource based on the country where the Client is located. This is useful for region-locking certain Resources or preventing Clients in certain countries from accessing sensitive systems.


How does it work?


The Client's location is determined by its IP address using[Google's Regional external Application Load Balancer API](https://docs.cloud.google.com/load-balancing/docs/https) . The load balancer reports this data to Firezone's control plane API based on the remote IP address of the Client's control plane connection. If the Client switches IPs, the control plane connection is re-established and the location is updated, invalidating all previously authorized Policies.


We deliberately avoided using Client-provided location data such as device GPS. These can be trivially spoofed by a malicious Client.


#### Client IP


Another way to restrict access to a Resource is by the Client's IP or CIDR address.


You can use it as an allowlist, for example, to allow employees to access a Resource from only a particular branch office.


Or, you can use it as a denylist to block access from malicious IP addresses or networks you explicitly **don't** want to allow access to your Resources.


#### Identity provider


If you've[configured an identity provider](https://www.firezone.dev/kb/authenticate) , you can also require users to have signed into that identity provider in order to access the Resource in question.


This functions well for MFA enforcement -- restrict access to an identity provider with MFA enabled to enforce MFA across your workforce for all Firezone-managed Resources.


And since Firezone supports adding multiple identity providers to your account, you can configure multiple SSO applications in your identity provider -- each with their own set of authentication requirements -- for even more flexibility over access to your Resources.


#### Time of day


Finally, you can restrict access to a Resource based on the time of day.


The time of day is determined by the locality defined in the Policy and not by the Client, so it's important to ensure your Policy's locality is set correctly to match the time zone you want to enforce access based on.


Time-based access policies open the door for interesting use cases. For example:


- Have a cron job that runs at 3 AM and that needs access to a Resource? Set up a Policy that only allows access between 3 AM and 4 AM to restrict access outside of the hours the job runs.
- Want to prevent employees from accessing certain Resources outside of business hours? Set up a Policy that only allows access between 9 AM and 5 PM on weekdays.


By locking down access to Resources based on the time of day, you add another tool to your security arsenal to prevent unauthorized access to your Resources.


### Blog posts


- [Using Tauri for a cross-platform security app](https://www.firezone.dev/blog/using-tauri) : Our Windows and Linux clients are built using Tauri, a Rust-based framework for building cross-platform desktop apps. In this post, we share our experience using Tauri and some tips for getting started with it for your own projects.
- [Improving reliability for DNS Resources](https://www.firezone.dev/blog/improving-reliability-for-dns-resources) : We've made some changes to how we handle DNS Resources to make them more reliable and resilient to network outages. Read this to learn more about how this change may affect your Firezone deployment.


## Wrapping up


That's all for this month's update! Subscribe to our[newsletter](https://www.firezone.dev/product/newsletter) below to get these updates once a month in your inbox. If you want a personalized demo to understand how Firezone can help secure your organization,[fill out the form here](https://www.firezone.dev/contact/sales) and we'll be touch.
