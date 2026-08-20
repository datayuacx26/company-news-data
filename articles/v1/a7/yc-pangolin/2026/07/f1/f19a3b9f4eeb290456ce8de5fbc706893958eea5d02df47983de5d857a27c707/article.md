---
schema_version: "1.0.0"
document_id: "f19a3b9f4eeb290456ce8de5fbc706893958eea5d02df47983de5d857a27c707"
company_key: "yc-pangolin"
company: "Pangolin"
source_id: "yc-pangolin-news-import-841387734568"
canonical_url: "https://pangolin.net/news/1-21-release"
published_at: "2026-07-20T00:00:00+00:00"
first_seen_at: "2026-07-22T08:11:44.188163+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:b5d200248c6076f4dbd0b744f2cde6112f507d5a41b1b734db4e8af08df68c15"
---

# Pangolin 1.21: Same Network Detection

Pangolin 1.21 is here. This is a smaller release, but it ships a few changes that matter day to day: clients and sites can connect directly when they share a local network, share links and access tokens pick up identity and session options, and the pending sites provisioning flow is more complete. Let's walk through it.


## Release Highlights


### Same Network Detection


When a Pangolin client and a site are already on the same local network, there is little reason for that traffic to leave the LAN and bounce through a relay. 1.21 adds same-network detection so those peers find each other on the LAN and form a direct peer-to-peer connection.


Packets stay on the local network. They are not routed out through the internet or your Pangolin server, and the connection skips the relay path. That usually means lower latency and less unnecessary egress through your router or ISP, which is especially useful when someone is on-site with the client while the site connector is already on that same network.


This requires updated clients and sites. Older versions will keep using the previous connection behavior.


Read more about[same-network detection and NAT traversal](https://docs.pangolin.net/manage/clients/nat-traversal) in the docs.


### Pending Sites and Provisioning


Site provisioning keys already let you ship connectors into a pending state for admin review before they go live. 1.21 extends that workflow to the resources those sites create.


When a site is pending, resources created for it from a provisioning blueprint are pending too. Pending resources stay hidden from the main resources table and are disabled by default, so end users cannot reach them while the site is still waiting for approval. Admins can still inspect and edit those resources from the site itself: open Resources on the site row, or use the site edit page. That makes it possible to review configuration, temporarily enable a resource, and verify access before promoting the whole setup.


Approving a pending site clears the pending state on its associated resources and enables them together. Rejecting a pending site deletes those pending resources along with the site, so a rejected provisioning run does not leave orphaned resource records behind.


Read more about[site provisioning and pending sites](https://docs.pangolin.net/manage/sites/site-provisioning) in the docs.


### Share Links and Access Tokens


Share links have always had a quieter counterpart: access tokens. Every share link is backed by an access token under the hood. When you create a share link, you can see the access token ID and value and pass them via a query parameter or headers for programmatic access to a resource. That pattern has been available for a while.


1.21 adds two improvements on top of that.


First, a share link can now be associated with a specific account user. When that link or access token is used, the associated user shows up in the logs, and Pangolin attaches the` Remote-*` user headers with that identity so upstream apps can treat the request as coming from a real user.


Second, you can enable persistent sessions on an access token. When the token is passed via a query parameter or header, Pangolin can exchange it for a session cookie so you do not have to attach the token on every subsequent request. Previously, access tokens behaved more like API keys and had to be included on each call.


Read more about[shareable links and access tokens](https://docs.pangolin.net/manage/access-control/links) in the docs.


### General Improvements and Bug Fixes


A few smaller additions made it into 1.21 as well:


1. **Enabled toggle for private resources** . Private resources can now be enabled or disabled without deleting them, including via blueprints.
2. **Remember last used identity provider** . Pangolin remembers the last IdP you used and marks it accordingly on the login page.


As always, this release also includes various UI improvements and bug fixes throughout the product.
