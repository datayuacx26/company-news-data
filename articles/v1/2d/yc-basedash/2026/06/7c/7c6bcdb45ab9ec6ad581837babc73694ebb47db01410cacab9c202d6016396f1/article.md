---
schema_version: "1.0.0"
document_id: "7c6bcdb45ab9ec6ad581837babc73694ebb47db01410cacab9c202d6016396f1"
company_key: "yc-basedash"
company: "Basedash"
source_id: "yc-basedash-rss-86d6e075e8cf"
canonical_url: "https://www.basedash.com/blog/how-to-share-dashboards-outside-your-company-securely/"
published_at: "2026-06-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:59.901198+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:ab4eba74ea87be5acaa296763e00b4dbc9c805ac3b234d70406316dc5305d647"
---

# How to share dashboards outside your company securely

Sharing a dashboard inside your company is easy: everyone is behind the same login, and your BI tool already knows who they are. Sharing with someone outside your company is a different problem. A client, an investor, a partner, or a contractor needs to see specific numbers, but they do not have an account, you do not want them to see anyone else’s data, and you cannot leak the whole dataset to the open internet.


This guide is for operators, analysts, and engineers who need to get a dashboard in front of an external viewer without creating a security incident. It covers the five common sharing methods, the security tradeoffs of each, a decision matrix for picking one, and the controls you should put in place before you send anything.


## What “sharing externally” actually means


External sharing breaks down into two questions that people often conflate:


1. **Authentication** : how does the viewer prove who they are? Options range from “no login at all” (anyone with the link) to a full account with single sign-on.
2. **Authorization** : once they are in, what data can they see? This is where row-level scoping matters. An external viewer should almost never see the same underlying query result as your internal team.


A safe external share gets both right. The most common mistakes come from solving authentication (a hard-to-guess link) and assuming it also solved authorization (it did not, because the link still returns the full dataset).


## The five ways to share a dashboard externally


### 1. Public link (anyone with the URL)


A public or “shareable” link renders a dashboard to anyone who opens it, with no login. Most BI tools support this. Metabase calls them[public links](https://www.metabase.com/docs/latest/questions/sharing/public-links) , and Power BI’s equivalent is[Publish to web](https://learn.microsoft.com/en-us/power-bi/collaborate-share/service-publish-to-web) .


Public links are the fastest option and the riskiest. The data behind the link is effectively public. Microsoft’s own documentation warns that Publish to web exposes the report and its data to “anyone on the internet” with no authentication, and that you should never use it for confidential information. Search engines can index these URLs, and the link works until you revoke it.


Use public links only for data that could appear on your homepage without consequence.


### 2. Tokenized or expiring link


A step up from a raw public link is a link with a secret token that you can expire, password-protect, or scope. The viewer still does not log in, but the link can be set to die after a date, require a passphrase, or carry a parameter that filters the data to one account.


This is the sweet spot for one-off shares with people you do not want to onboard: a single client report, a metric snapshot for a partner, a board figure for an advisor. The security depends entirely on whether the token also constrains the query. A link that expires but still returns every customer’s data is not safe.


### 3. Guest or external-viewer account


Here the viewer gets a real, limited account: an email invite, a magic link, or an SSO login through their own identity provider. The BI tool knows who they are on every request, which means you can apply[row-level security](https://www.basedash.com/blog/data-governance-for-ai-powered-bi-row-level-security-access-controls-and-compliance) so each guest sees only their slice, log their access, and revoke them individually.


Guest accounts are the right default for ongoing relationships: a client who checks their numbers weekly, a vendor with a standing report, a fractional exec. They cost a little setup but give you identity, scoping, and an audit trail.


### 4. Embedded analytics


Embedding puts the dashboard inside an application you control, usually your own product or a customer portal. Your app authenticates the user, then passes a signed token to the BI tool that says who they are and what they can see. The viewer never touches the BI tool directly.


Embedding is the most secure and the most work. It is the correct pattern when many external users each need their own scoped view at scale, for example customer-facing analytics inside a SaaS product. The hard part is data isolation, which we cover in detail in[multi-tenant analytics architecture](https://www.basedash.com/blog/multi-tenant-analytics-architecture-how-to-isolate-customer-data-in-embedded-dashboards) and the[embedded analytics for SaaS guide](https://www.basedash.com/blog/embedded-analytics-for-saas-the-complete-guide-to-adding-dashboards-to-your-product) .


### 5. Static export (PDF, image, or scheduled email)


Sometimes the safest share is not a live dashboard at all. A PDF, a PNG, or a scheduled email digest sends a snapshot with no live connection to your data. The viewer cannot drill into anything you did not put in the export, and there is no URL to leak or query to abuse.


Exports are ideal for board packs, investor updates, and client recaps where the numbers are a point-in-time story rather than a tool to explore. The tradeoff is that the data is stale the moment it is sent, and a forwarded PDF is impossible to revoke.


## A decision matrix for picking a method


Match the method to the audience and the sensitivity of the data, not to whatever is fastest.


Method Login required Data scoping Revocable Audit trail Best for


Public link No None (full result) Yes (kills link) No Truly public metrics, marketing stats


Tokenized / expiring link No Only if parameterized Yes Limited One-off client or partner snapshots


Guest account Yes Row-level per user Per user Yes Ongoing client and vendor reporting


Embedded Yes (via your app) Row-level per tenant Per user Yes Customer-facing analytics at scale


Static export No Whatever you include No (already sent) Send log only Board decks, investor updates


A simple way to choose:


- **One person, one time, low sensitivity** : tokenized link with an expiry.
- **One person, ongoing** : guest account with row-level scoping.
- **Many external users, each scoped** : embedding.
- **A snapshot for a meeting** : static export.
- **Data you would put on a billboard** : public link, and even then, prefer a scoped one.


## How row-level scoping works for external viewers


The single most important control for external sharing is making sure the query only returns the rows that viewer is allowed to see. There are three common ways tools enforce this:


1. **Parameterized links** : the secret token encodes a filter, for example` account_id=42` , and the server applies it before running the query. The viewer cannot change it because the parameter is signed. This is what makes a tokenized link safe.
2. **Row-level security policies** : the dashboard runs as the logged-in user, and a policy on the dataset filters rows based on that user’s attributes. Tableau implements this with user filters and entitlement tables; most warehouses support it through row access policies. See our[BI permission model guide](https://www.basedash.com/blog/how-to-design-a-bi-permission-model-for-a-saas-team) for how to structure these.
3. **Signed embed tokens** : your application decides what the user can see and bakes that into the token it hands the BI tool, so isolation is enforced before the query runs.


If a sharing method cannot do at least one of these, treat it as “shows everything” and only use it for data that is safe to show everyone.


## A pre-send security checklist


Run through this before any external share goes out:


- **Scope the data.** Confirm the viewer sees only their rows, not the full table. Test with a real example account.
- **Set an expiry.** Default to the shortest useful lifetime. A quarter, a month, or a single meeting.
- **Strip hidden columns.** Tooltips, drill-throughs, and underlying tables often expose fields you did not mean to share. Check what is reachable, not just what is visible.
- **Disable export and drill if not needed.** A “download CSV” button can hand over the raw dataset behind a curated chart.
- **Turn off search indexing.** Add` noindex` or use a tool that blocks crawlers on shared links so reports do not show up in Google.
- **Log and review access.** Prefer methods that record who viewed what. Revisit active shares on a schedule and revoke stale ones.
- **Avoid PII by default.** If the external viewer does not need names, emails, or IDs, do not include them. Aggregate or mask instead.
- **Use a real account for sensitive data.** If the numbers matter, accept the small friction of a guest login over an anonymous link.


## When not to share a live dashboard at all


Live access is not always the right call. Send a static export instead when:


- The data is highly sensitive and the viewer only needs a few numbers.
- The recipient might forward the link, and you cannot control who they forward it to.
- You want the numbers frozen as of a specific date, for example a board figure or a contractual report.
- The relationship is one-off and not worth provisioning access for.


A point-in-time PDF you can stand behind often beats a live link you have to monitor.


## How Basedash handles external sharing


Basedash is an AI-native BI tool that connects directly to your production database or warehouse, so external sharing is part of the same workflow as building the dashboard. You can share a view with a tokenized link, invite an external viewer as a scoped guest, or embed a dashboard into your own product with a signed token that enforces row-level isolation per tenant. Because non-technical teammates can ask follow-up questions in natural language, a client or partner can explore their scoped data without anyone writing new SQL. For teams that want to put analytics directly in front of customers,[Basedash embedding](https://www.basedash.com/blog/introducing-basedash-embedding) handles the authentication and per-tenant scoping described above.


The general rule holds regardless of tool: pick the lightest method that gets both authentication and authorization right for that specific audience.


## FAQ


**Are public dashboard links safe to use?** Only for data you would publish openly. A public link has no authentication and usually returns the full query result, so anyone with the URL sees everything. Microsoft and Metabase both document this behavior explicitly. For anything confidential, use a scoped link, a guest account, or embedding.


**How do I share a dashboard with someone who does not have an account?** Use a tokenized link with an expiry and, critically, a parameter that filters the data to that viewer. The viewer opens it in a browser with no login, but only sees the rows the signed token allows.


**What is the safest way to give a client ongoing access to their data?** A guest account with row-level security. The tool authenticates the client on every request and a policy limits them to their own rows, which gives you per-user revocation and an audit trail.


**How do I stop a shared dashboard from showing up in Google?** Use a tool that blocks crawlers on shared links or applies a` noindex` directive, and prefer authenticated access over public links. Public links can be discovered and indexed if the URL leaks.


**Should investors get a live dashboard or a PDF?** For most updates, a static export is cleaner. It freezes the numbers as of the reporting date and cannot be abused as a live query. Reserve live access for investors who genuinely want to explore, and scope it with a guest account.


**Can external viewers see data I did not intend to share?** Yes, if you are not careful. Drill-throughs, tooltips, “view underlying data” options, and CSV export can all expose fields beyond the chart. Disable what you do not need and test every reachable path before sharing.
