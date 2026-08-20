---
schema_version: "1.0.0"
document_id: "7e04d264757ee3e4ba7b29476dfec8e7ab216910d97a9299dca1845248d57e8c"
company_key: "yc-anakin"
company: "Anakin"
source_id: "yc-anakin-news-import-edbd07d03db6"
canonical_url: "https://anakin.io/blog/authenticated-scraping-behind-the-login"
published_at: "2026-07-09T00:00:00+00:00"
first_seen_at: "2026-08-05T00:49:53.361744+00:00"
fetched_at: "2026-08-05T03:48:37.411401+00:00"
content_hash: "sha256:75986f57ecaa310fcc207d27325ed6159e06278ec35d307c1a43bb2e29cd4151"
---

# The Best Data Is Behind a Login: Authenticated Scraping for AI Agents

Ask an AI agent to pull your order history and watch what happens.


It will find the storefront. It will read the product page, the shipping FAQ, the returns policy - every word written for a stranger. Then it will hit the sign-in wall and stop, and hand you back a summary of a marketing page you didn't ask about.


The public web is the brochure. The data that actually runs a business - what you paid, what you owe, what your supplier quoted you specifically, what shipped and what didn't - is on the other side of a login. Any agent that can't authenticate is reading the brochure and calling it research.


So the question isn't whether agents need to log in. It's what you have to hand over to let them. Authenticated scraping - pulling the data that sits behind a sign-in - lives or dies on that one decision.


## Three bad answers


**Give the agent your password.** This is the common one, and it's worse than it looks. A credential an agent can reach tends to end up in a prompt - and a prompt is a string that gets logged, retried, cached, and shipped to a model provider. Careful plumbing can keep it out of the model's context, but that's a property of how someone wired it up, not of the design. You are not handing over access to one site. Most people reuse credentials, and a password is a master key: it opens the account, and it also opens the *settings* for that account. An agent that can read your orders can also change your email and lock you out. Nothing about "read my order history" requires that much authority.


**Drive a real browser.** Spin up Chrome, type into the form, click through. It works, sometimes. It's also slow, expensive, and brittle in a specific way that matters: it breaks when the site changes its markup, which sites do constantly and without warning. And it doesn't actually solve the problem - a browser still has to get the password from somewhere. You've added a rendering engine and a bill, and you're back at the same question.


**Don't log in at all.** Honest, and it's what most agent stacks quietly do. It also means the agent is useful for research and useless for work.


None of these is a real answer, because all three are answering the wrong question.


## Your agent doesn't need your password


It needs a session.


When you sign in to a site, you hand over a password exactly once. What you get back is a session - a set of cookies, sometimes a bearer token - that the site accepts as proof you already logged in. Every click after that is authenticated by the session. Your password is not sent again. It has done its job.


That distinction is the whole thing, because a session is a **strictly better object to hold than a password** :


- **It's scoped to one site.** A session cookie for one supplier portal is meaningless everywhere else. A reused password is not.
- **It expires on its own.** Sessions die in hours or days. Passwords live until someone changes them.
- **It's revocable by the site.** Sign out everywhere, and it's dead - no coordination with anyone.
- **It mostly can't escalate.** Most sites re-prompt for the password before letting you change the password, the email, or the payout account - so a session usually buys read access and not much more. Not universally: some portals will happily change account details on a session alone. Worth checking on any site that matters to you.


> A password is the key to the building. A session is a badge that opens one floor and stops working Friday.


So: get the password out of the system as fast as possible, and keep only the session. That's the model.


## What actually happens at login


Concretely, in Wire, a login is five steps - and the credential is gone by the third:


# Step What happens


1 Read Credential used once, in memory


2 Sign in Site issues a session


3 Encrypt AES-256-GCM, ciphertext stored


4 Discard Credential gone - no disk, no log


5 Run Worker receives cookies only


That last step is the one people miss, and it's structural rather than a matter of policy. The component that logs in and the component that runs your tasks are separate. Credentials only ever exist inside the first one, briefly. The scraper on the other side has no code path to a password because it is never sent one.


Sessions are also kept alive as they're used: the runtime writes cookies back after each action, so a session the site keeps refreshing stays fresh. When it does finally lapse, the next task fails with an explicit` AUTH_EXPIRED` rather than returning half-empty data and letting you find out later.


## But you still typed the password once


Which is the honest gap in everything above.


The password never persists. It's still true that at some point, someone pasted it into a form belonging to a company that isn't the one it belongs to. For a lot of teams that's fine. For anyone whose credentials live under a policy - rotated on a schedule, owned by IT, never shared outside a vault - it isn't, and no amount of "we discard it immediately" fixes the moment of handover.


**Identity Sources** removes the handover. You connect a vault you already control - 1Password or Azure Key Vault today - and point an identity at a specific item in it. At login time Wire reads that item, in memory, uses it, and drops it. The password is never typed into our forms and never appears in an API request body. What Wire stores is a *reference* : "the item named X, in the vault named Y." The secret stays where your policy already put it.


The practical consequence is that rotation stops being an event. Wire always reads the current version of the secret, so when your password policy fires and the value in the vault changes, the next login picks up the new one. Nobody updates anything on our side. Nobody re-enters anything.


> Worth being precise, because it's easy to overclaim: **re-authentication is on demand, not a background job.** No worker is quietly logging into your accounts on a schedule. When a session expires, the next login request re-reads the vault and mints a new one. What you never do is re-enter a password.


## What Wire holds, and what it doesn't


**Stored**


- The encrypted session - cookies and tokens (AES-256-GCM)
- An encrypted, scoped, revocable vault token
- A **reference** to a vault item - not its contents
- Connection metadata - vault names, status, timestamps


**Never stored**


- Your site password, in any form
- Your vault's master password
- The credential beyond one in-memory login
- Anything replayable outside the scope you granted


The asymmetry is deliberate. A database holding sessions is a meaningfully smaller prize than one holding passwords: the contents are per-site, self-expiring, revocable by the site owner, and mostly can't be escalated into account takeover. There is no password table to breach because there is no password table.


There's a second consequence that matters more than it first appears. Because the secret is read from your vault rather than ours, the record of every read is written by your provider, into your audit trail, on infrastructure we don't touch. Verifying what we accessed doesn't mean trusting a log we generated about ourselves - it means opening your own.


## Turning it off


Access you can't withdraw isn't access you granted; it's access you lost. So there are five separate levers, with different blast radii - *revoke in your vault, disconnect the source, disconnect and delete identities, delete one identity, delete just the session.*[The next post walks through exactly what each one removes](https://anakin.io/blog/revoke-wire-access-what-each-deletes) .


Deletion is immediate and it's yours to trigger - a delete removes the row, not a flag on it. Nothing expires on a schedule you didn't set.


## What doesn't work


An honest list, because the failure modes are more useful than the feature list:


- **MFA-protected accounts.** If the site demands a code from an authenticator app or an SMS, automated login can't complete. Wire reports` MFA_REQUIRED` rather than guessing.
- **Captcha challenges.** Sometimes transient, sometimes permanent for that account and IP. You get` CAPTCHA_REQUIRED` , not a retry loop.
- **Locked accounts.** If the site has locked it, that's between you and the site.` ACCOUNT_LOCKED` .
- **Sites that change their login flow.** They do.` LOGIN_PAGE_CHANGED` means our sign-in integration needs updating, which is our job, not yours.


Every one of these returns a specific code instead of a generic failure, because "login failed" is not an actionable thing to hand an agent - and because a vendor who won't tell you what breaks is telling you something.


## The short version


The valuable web is authenticated, and the instinct to solve authenticated scraping by handing over a password is the wrong instinct. Passwords are master keys with no expiry and no scope. Sessions are narrow, temporary, and revocable, and they're all an agent ever actually needed.


Use the password once, in memory, in a component that does nothing else. Keep the session. Better still, keep the password in a vault you control and let it be read at the moment of use, so it's never handed over at all.


> You give us a scoped, revocable token to a vault you own. We read one credential at a time, in memory, to log in - keep the session, never the password - and you can cut us off in one click.


**Try it.** Wire already covers a long list of sites that sit behind a login. If yours is one of them, the sign-in work is done.


→[Browse the catalog](https://anakin.io/products/wire) ·[Get started](https://anakin.io/)


---


[Back to blog](https://anakin.io/blog)
