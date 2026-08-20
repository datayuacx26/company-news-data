---
schema_version: "1.0.0"
document_id: "1be405474360d9ba8125cb3a31b16bf9dcea8550c4e9f3ecfb640453ed1d4490"
company_key: "yc-firezone"
company: "Firezone"
source_id: "yc-firezone-news-import-75fbee21a5d6"
canonical_url: "https://www.firezone.dev/blog/sep-2024-update"
published_at: "2024-09-02T00:00:00+00:00"
first_seen_at: "2026-07-24T08:10:10.796715+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:a63392af20df1233287fdea078a751d21050cd283a40843c3fe379fbbfb26c00"
---

# September 2024 Update

## In this update:


- **New feature:** Internet Resources
- **New feature:** REST API Beta


- **New feature:** Improved wildcard matching for DNS Resources
- **Blog post:**[sans-IO: The secret to effective Rust for network service](https://www.firezone.dev/blog/sans-io)


### Internet Resources


Up until today, Firezone has operated what's known as a "split-tunnel" architecture. That means that only traffic destined for your protected resources is routed through Firezone, while all other traffic goes directly to the internet. This architecture works great for routing traffic to IPs, CIDRs, and DNS Resources, but what if you want to protect your workforce as they work from public cafes, airports, and other untrusted networks? What if the internet itself is a resource you want to protect?


Well, now you can. Today we are excited to announce the launch of the Internet Resource, which allows you to route all of your internet-bound traffic through Firezone as well. This means that you can now apply the same security policies to all of your traffic, not just the traffic destined for your protected resources.


Like any other Resource in Firezone, you can apply Policies to the Internet Resource to control who can access them. When you grant access to the Internet Resource, it'll appear at the top of the Resources list in the Firezone Client, just like any other Resource. Unlike other Resources, however, the Internet Resource can be enabled or disabled directly from the Client.


Internet Resources are supported in Clients and Gateways **v1.3.0** or later, and are available to accounts on the` Team` and` Enterprise` plans. There's no need to create them manually -- simply select the Internet Resource when creating a new policy in order to use it.


### REST API


Firezone now has a[REST API](https://www.firezone.dev/kb/reference/rest-api) , available in closed beta. The API allows you to manage all of the same configuration you can in the admin portal, but programmatically.


We've published an OpenAPI spec for it at[https://rest-api.firezone.dev/swaggerui](https://rest-api.firezone.dev/swaggerui) you can use to interact with the API right from your browser. You can even generate a native client library to use the API in your own applications using[Swagger Codegen](https://swagger.io/open-source/swagger-codegen/) .


To use the REST API, head to the` Settings -> API Clients` page in your account, request access, and we'll get you set up.


### Improved wildcard matching for DNS Resources


We've improved how DNS Resources can be matched for routing. They now support a more powerful syntax for matching names, similar to how glob pattern matching works in Unix shells.


Now, you can use wildcards to match single characters, parts of a subdomain, and even multiple subdomains. This opens the door to a lot of new possibilities, like:


- ` us-west-?.company.com` to match things like` us-west-1.company.com` ,` us-west-2.company.com` , and so on
- ` **.google.com` to match all subdomains under` google.com` recursively
- ` aws*.amazon.com` to match AWS services at` aws.amazon.com` ,` aws1.amazon.com` , and so on *but not*` www.amazon.com` .


Improved wildcard matching requires Client and Gateway **v1.2.0** or later.


## End


That's all for now.[Sign up](https://app.firezone.dev/sign_up) for a free starter account to try out all of the above. If you're interested in using Firezone for your organization,[contact us](https://www.firezone.dev/contact/sales) for a customized demo.
