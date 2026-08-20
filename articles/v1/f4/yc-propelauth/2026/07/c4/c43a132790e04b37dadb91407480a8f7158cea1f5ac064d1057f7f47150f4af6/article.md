---
schema_version: "1.0.0"
document_id: "c43a132790e04b37dadb91407480a8f7158cea1f5ac064d1057f7f47150f4af6"
company_key: "yc-propelauth"
company: "PropelAuth"
source_id: "yc-propelauth-news-import-52b3c4ba8ae4"
canonical_url: "https://www.propelauth.com/post/user-impersonation-updates"
published_at: "2026-07-21T00:00:00+00:00"
first_seen_at: "2026-07-25T20:01:02.470883+00:00"
fetched_at: "2026-07-28T21:21:02.928028+00:00"
content_hash: "sha256:7401a787d0bc7f8413ba59fa5e26efb07a02d31467f50256e973e235b58ed968"
---

# User Impersonation Updates: Blocklist, direct links and more

Impersonation is one of those features that's incredibly useful right up until someone asks "wait, who approved that?" It's great for debugging, support, and demos, but underneath all of that convenience, you're handing an employee the ability to log in as a customer. That's a sensitive action, and your controls should match it.


Today, we're announcing three new additions to **User Impersonation** : Organization Blocklists, Impersonatable Organizations, and direct Impersonation Links.


## Block an Organization From Being Impersonated #


Some customers won't want their accounts impersonated under any circumstances, no matter who's asking or why.


Up until now, honoring that meant documenting it somewhere like an internal note, a support ticket, or a CRM field, and hoping every employee remembered to check it before starting a session. That's a fragile way to enforce a hard rule.


**Organization Blocklists** let you enforce it directly instead. Add an organization to the blocklist, and no user belonging to it can be impersonated, full stop. It's not a reminder anymore. It's a restriction the system actually respects, which means you can honor customer-specific commitments without relying on anyone remembering to check first.


## Impersonatable Organizations #


A common pattern we kept seeing: support and customer success employees are each responsible for a specific slice of your customer base, and they only need impersonation access within that slice.


**Impersonatable Organizations** lets you set a per-employee allowlist of organizations. If a CSM owns five enterprise accounts, you can scope their impersonation access to exactly those five, with no broader access, and no need to route every request through an approval flow.


This option respects whether or not the user can Always Impersonate or needs to[Request Access](https://docs.propelauth.com/overview/user-management/user-impersonation?ref=propelauth.mymidnight.blog#impersonation-requests) ahead of a session. If they need to Request Access, they will only be able to create access requests for organizations on their list.


It's the difference between "this person can impersonate anyone" and "this person can impersonate the customers they actually work with," without adding operational overhead for either your team or theirs.


## Link Directly to an Impersonation Workflow #


Impersonation usually starts from somewhere else, like a Slack alert, a support ticket, or an internal admin tool, and previously, that meant opening the dashboard, finding the right project, and searching for the user before you could even start.


Now you can generate a direct link that jumps straight into the impersonation workflow for a specific user, project, and environment:


```text
https://app.propelauth.com/impersonate-user?email=user@example.com&project=your_project&env=prod
```


This doesn't skip any of your existing controls. The employee still lands on a confirmation page to verify they're impersonating the right account, and if they don't already have permission, they'll still need to request it. It just removes the busywork of getting there, which makes it a natural fit for Slack support bots, admin dashboards, and incident tooling that already has the user's info on hand.


## Putting It Together #


These three features are additive. You don't need all of them to get value from impersonation, and you can layer them in as your team and customer base grow. A small team might just need alerts and an audit log. A larger one might want an allowlist per employee, blocklisted organizations that should never be touched, and a faster path into the workflow when a request comes in.


Either way, the goal is the same: your team keeps the speed impersonation gives them, and you keep a clear, enforceable record of who has access, why, and when it was used.


## Learn More #


You can find the full rundown of impersonation permissions, blocklists, and session controls in the[PropelAuth User Impersonation docs](https://docs.propelauth.com/overview/user-management/user-impersonation?ref=propelauth.mymidnight.blog) .
