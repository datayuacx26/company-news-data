---
schema_version: "1.0.0"
document_id: "93b9d48755deb1457ad0f694e7011c1767f8cba8f8b1997f3b837a20bb80df75"
company_key: "yc-heroic-labs"
company: "Heroic Labs"
source_id: "yc-heroic-labs-news-import-3563bf1285ab"
canonical_url: "https://heroiclabs.com/blog/nakama-console-improvements/"
published_at: "2026-01-21T00:00:00+00:00"
first_seen_at: "2026-07-21T22:49:46.144164+00:00"
fetched_at: "2026-07-28T22:23:18.090789+00:00"
content_hash: "sha256:44ba56a630dce0fc70015476307e944c8de048c19bab5011e090543cf4d8b8e9"
---

# More Nakama Console Updates: More Power for Your Team

A few months ago, we revealed a completely[redesigned Nakama Console](https://heroiclabs.com/blog/announcing-nakama-console-3) with a cleaner UI and many quality-of-life improvements. We recently rolled out another round of significant updates that make the console more useful for everyone on your team, from backend engineers debugging production issues to support staff helping players.


These changes came directly from speaking with our users who needed to allow team members to manage player issues without requiring deep technical knowledge. Rather than building a one-off solution, we asked ourselves: how can we make the console work better for everyone? The result is a set of enhancements that benefits the entire community.


## Fine-Grained Permissions


The console now supports a full-fledged access control system. Create accounts with precisely scoped permissions, giving team members access only to what they need. A customer support specialist might be able to view player data and grant items, but not modify server configuration. A QA engineer could have read-only access to specific environments. This brings Nakama’s console in line with the permission models in Satori and Heroic Cloud, so you can confidently grant console access to non-developers on your team.


Create accounts with precisely scoped permissions for your team.


## Built-in Hiro Integration


Nakama now knows when Hiro is running on your server, which unlocks a new category of console functionality. Instead of digging through raw JSON in storage collections, see player state rendered in proper form views that understand your Hiro configuration.


View and manage player state with native Hiro support.


For example, with Hiro Inventory, you can view items as they’re meant to be displayed: names, quantities, and properties. You can grant specific items to players from a dropdown that knows what items exist in your game. For Progression, you can view a player’s unlocks and pre-conditions in a more ergonomic view.


This update currently covers Inventory and Progression, with more Hiro systems coming based on demand. The goal is to make player states as accessible and actionable as game configuration.


## Audit Logging


Every action taken through the console or the console API is now logged to a searchable audit trail. You can filter by user, resource type, or time range to see exactly who did what and when. Did someone accidentally delete a player’s inventory? Now you’ll know. This is especially valuable when multiple people are working in the console, or when you need to track down the source of an unexpected change.


Searchable audit trail for all console actions.


## Account Export and Import


We’ve improved account exporting and added the ability to import accounts across deployments. This is a big deal for debugging. Say a player reports a bug that only happens with their specific progression state. You can export their account from production, import it into your development environment on top of a test account, and reproduce the issue with their exact data. No more manually recreating complex player states.


Export and import accounts across deployments for easier debugging.


## Open Source, Community Benefits


All of these improvements are part of the open-source Nakama console. When someone has a specific need or feature request, we look for ways to benefit everyone. That’s the model we’ve followed for ten years, and it’s why sponsoring work or contributing PRs has an outsized impact.


If you have workflow needs that aren’t covered,[open an issue on GitHub](https://github.com/heroiclabs/nakama/issues) . Your feedback shapes what we build next.
