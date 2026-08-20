---
schema_version: "1.0.0"
document_id: "debb500b3da705039e7ab261799c5a6f8bc5ffbeddd51ed333c46933cc775720"
company_key: "yc-golinks"
company: "GoLinks"
source_id: "yc-golinks-news-import-00b983687ae2"
canonical_url: "https://www.golinks.com/blog/go-link-naming-conventions/"
published_at: "2026-06-25T21:35:04+00:00"
first_seen_at: "2026-07-27T02:47:04.931568+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:8f974db61bd176601ebcdd2466e993a563a259fd99db7a34ef666708dd2adb55"
---

# Go Link Naming Conventions That Actually Drive Adoption

A great go link is one anyone on your team can guess, say out loud, and find without a search. Naming conventions are what make that possible at scale — turning a collection of links into a shared language for the whole organization.


## Why Naming Conventions Matter More Than You Think


Go links only solve the information-finding problem if people can actually use them — and that starts with the name. When a link is named well, it becomes muscle memory. Teammates type it from memory, mention it in meetings, paste it in Slack, and embed it in docs without ever opening a link directory. When it’s named poorly, none of that happens.


The stakes are real:


- Employees spend an average of[3.6 hours every day searching for information](https://www.coveo.com/en/company/news-releases/2022/fruitless-searching-irrelevant-information-inefficient-tools-contribute-to-great-resignation)
- [47% of digital workers](https://futurecio.tech/digital-workers-still-struggle-to-find-information/) struggle to find information needed to do their jobs
- For companies with 1,000+ employees, that friction costs[over $3M in lost productivity annually](https://www.mckinsey.com/industries/technology-media-and-telecommunications/our-insights/the-social-economy)


Go links are built to solve this problem. Strong naming conventions make them even more effective — and drive results across the organization.


## The Core Rules for Go Link Names


- **Keep it short and guessable.** Aim for one to three words. If a new teammate couldn’t guess the link on their first try, it’s too long or too specific.
- **Lowercase only.** The platform enforces this.` go/salesprocess` , not` go/SalesProcess` .
- **No spaces.** Use hyphens or underscores to separate words.` go/all-hands` ,` go/all_hands` , and` go/allhands` all resolve identically — the platform treats them as equivalent so verbal sharing doesn’t break links.
- **Allowed characters:** letters, numbers, hyphens, underscores, and any ASCII character supported by most languages. No emojis, no other special characters beyond` -` and` _` .
- **` go/d` is reserved** for the GoLinks directory and cannot be used as a link name.
- **Use nouns, not actions.**` go/roadmap` , not` go/viewroadmap` .` go/expenses` , not` go/submitexpenses` .
- **No abbreviations unless they’re universal.**` go/hr` is fine.` go/pnlrprt` is not.
- **Multi-language is supported.** Global teams can create go links in any language. The convention principles still apply.


For the full authoritative reference, see[GoLinks’ naming conventions help doc](https://www.golinks.com/help/golinks-naming-conventions/) .


## Team-Based Naming Patterns


For larger organizations, prefixes are the clearest way to scope links by team or function.


Team Prefix Example links


Engineering` eng-`` go/eng-oncall` ,` go/eng-runbook` ,` go/eng-prs`


Sales` sales-`` go/sales-deck` ,` go/sales-crm` ,` go/sales-pipeline`


HR / People` hr-`` go/hr-pto` ,` go/hr-benefits` ,` go/hr-handbook`


Marketing` mktg-`` go/mktg-brief` ,` go/mktg-calendar`


Customer Success` cs-`` go/cs-tickets` ,` go/cs-escalations` ,` go/cs-playbooks`


Finance *(none)*` go/expenses` ,` go/payroll` ,` go/budget`


Company-wide *(none)*` go/allhands` ,` go/roadmap`


Company-wide links — the ones everyone uses — generally don’t need a prefix. The prefix earns its place when the same concept (e.g.,` go/deck` could mean a sales deck, a product deck, or an all-hands deck depending on the team) would otherwise mean different things across teams.


## Geo-Links: Naming for Global Teams


Geo-links automatically redirect users based on their country or state — so` go/benefits` takes a US employee to the US benefits portal and a UK employee to the UK version, with no extra links and no extra maintenance.


Keep the base name consistent and let geo-routing handle the rest. Don’t create` go/benefits-uk` ,` go/benefits-us` , and` go/benefits-de` as separate links — this is exactly the problem geo-links are designed to eliminate.


Common use cases: regional HR policies, localized legal docs, country-specific onboarding materials.


## Variable Go Links: Naming for Dynamic Resources


Variable go links let part of the path act as a dynamic input.` go/ticket/\[ID\]` , for example, routes directly to the corresponding support ticket when a user types` go/ticket/12345` .


The base name should still follow core conventions — short, lowercase, guessable. The variable part, indicated with bracket notation, handles the dynamic piece. Common use cases include support tickets, Jira issues, employee profiles, and order lookups.


## How to Handle Edge Cases


### Updating destinations, not names


When a destination URL changes, update the link in GoLinks — the name stays the same.` go/roadmap` always points to the current version, with no retraining required.


### Aliases


A single go link can have multiple names. Set` go/mail` as the primary link, then add` go/m` ,` go/gmail` , and` go/mailbox` as aliases. Update the destination once and all aliases follow automatically. Best practice: set up aliases at creation time, especially during company onboarding. See the[GoLinks aliases help doc](https://www.golinks.com/help/golinks-aliases/) for setup details.


### Private go links


For personal-use links that shouldn’t appear in the shared namespace — bookmarks, works in progress, links not ready to share — use private go links. They’re visible and searchable only by the creator, keeping your personal resources organized without cluttering the team library.


### Person-specific links


Use` go/john-bio` for individual profiles or` go/team-bios` when the destination covers the whole team. Scale the name to the scope of the destination.


## Common Naming Mistakes (And How to Fix Them)


Most naming problems come down to a handful of recurring habits that erode adoption.


- **Overly specific names no one can guess.**` go/q3-2023-board-deck-final-v2` is a filing system, not a go link. Use` go/board-deck` and update the destination URL when a new version is ready.
- **Duplicate links instead of aliases.** When teammates create multiple links to the same destination, every URL change requires updating each one individually. Aliases update all at once. Consolidate duplicates and use aliases from the start.
- **“Quick” links with no convention that never get cleaned up.** If it’s worth creating, it’s worth naming correctly. Sloppy links compound over time.


## Building a Naming Convention Your Team Will Actually Follow


The rules are only as good as the rollout. A naming convention that lives in a doc no one reads isn’t a convention. These are the practices that turn naming standards into team habit.


- **Document the convention in a go link itself.** A` go/link-guide` pointing to your naming standards doc puts the reference exactly where people will find it.
- **Embed go links in daily workflows** — standups, meeting agendas, Slack threads, recurring docs. Adoption follows visibility. The more teammates encounter well-named links in context, the faster the convention becomes habit.
- **Tie the convention to onboarding.** New hires who learn the pattern on day one adopt it naturally. The[Manager’s Guide to GoLinks Adoption](https://www.golinks.com/blog/the-managers-guide-to-golinks-adoption/) covers this in detail, including how to make go links part of the new hire experience from the start.
- **Use GoLinks analytics** to audit unused or duplicated links, and run a quarterly cleanup: archive stale links, standardize inconsistent ones.


The payoff is measurable. Based on GoLinks customer data, teams with healthy go link adoption see:


- Roughly **3 hours of productive time gained** per active user per week
- A **20% decrease in onboarding time**
- Approximately **$50K in annual savings** per 100 employees


## Naming Is the Foundation


A naming convention is a team agreement, not a technical constraint. Get the names right — short, lowercase, predictable — and go links become the kind of shared language that scales with your organization.


Start simple: pick a prefix system, document it in` go/link-guide` , and share it on day one.


#### Access and share resources instantly with GoLinks


[Try for free](https://www.golinks.io/signup.php?utm_source=blog&utm_medium=blog&utm_campaign=blog-cta&utm_content=free-trial)
