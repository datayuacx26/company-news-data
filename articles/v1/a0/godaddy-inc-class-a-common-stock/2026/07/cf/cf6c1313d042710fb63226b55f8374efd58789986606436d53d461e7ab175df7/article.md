---
schema_version: "1.0.0"
document_id: "cf6c1313d042710fb63226b55f8374efd58789986606436d53d461e7ab175df7"
company_key: "godaddy-inc-class-a-common-stock"
company: "GoDaddy Inc."
source_id: "godaddy-inc-class-a-common-stock-news-import-cf537cccbea7"
canonical_url: "https://www.godaddy.com/resources/news/introducing-the-godaddy-developer-platform-domain-apis-for-developers-and-their-agents"
published_at: "2026-07-14T15:50:04+00:00"
first_seen_at: "2026-07-22T09:50:37.003516+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:478b2a515f21f2a985268563952af08e2beb7afdfe39c7df2ff0150bf5d58770"
---

# Introducing the GoDaddy Developer Platform: Domain APIs for Developers and Their Agents

## Key takeaways


- GoDaddy launched a beta Developer Platform starting with domains, exposing the full lifecycle (search, register, DNS) as documented API calls plus an alpha CLI, so a coding agent can go from "does this name exist" to "traffic resolves" without a browser step.
- Every call uses scoped auth (OAuth or a Personal Access Token) that you can limit to exactly the work it needs and revoke individually, so a CI job updating DNS never gets purchase permission unless you hand it over.
- Purchasing is deliberately hard to misuse: a quote-then-execute flow locks in the price, an idempotency key kills double-charges, and a consent object ties every registration to a real human approval, with no card field anywhere in the request body.


## Introduction


Almost everything in a modern deploy is programmable. Compute is an API call, TLS certificates renew themselves, CI runs from a file in the repo. But when the project needs a name, most of us still handle that part by hand: open a browser, search, buy, then click through a DNS panel to point the new domain at a server. It is simple enough, but it is a context switch that pulls you out of the build.


Coding agents raise the stakes on that switch. An agent can scaffold the app, provision the infrastructure, and wire the deploy, but a browser step stops it cold. It needs the whole loop as documented calls, from finding a name to pointing it at the app, with secure purchasing. And it needs that where the domains and the businesses already live.


Today we're opening the beta of the new GoDaddy Developer Platform, starting with domains: a rebuilt API surface, a new credential model, a CLI, and[docs](https://developer.godaddy.com/en/docs/api-users) that cover the domain lifecycle from "does this name exist" through "traffic resolves to my app." Search, price, register and configure DNS from your terminal, your CI pipeline, or your coding agent.


## The whole loop, from the CLI


Before we get into what's new, here's the end-to-end domain loop in the new CLI, from install to a resolving name.


```text
# Install the alpha CLI and sign in through your browser
curl -fsSL https://github.com/godaddy/cli/releases/download/alpha/install.sh | bash
gddy auth login


# Find a name (suggestions only return names you can actually register)
gddy domain suggest "lemonade stand software" --tlds com --tlds dev --limit 5
gddy domain available lemonstand.dev


# Review TLD agreements, then quote the exact registration.
# Quoting is free; it locks the price and settings and returns a quoteToken.
gddy domain agreements --tld dev
gddy domain quote lemonstand.dev


# Purchases are real money, so registration requires both --agree and --confirm.
# Use the quoteToken returned by the quote command, within its ~10-minute lifetime.
gddy domain purchase --quote-token <quoteToken> --agree --confirm


# Point it at your app. "set" replaces the records for this type and name,
# so re-running it is safe.
gddy dns set lemonstand.dev --type A --name @ --data 203.0.113.10 --ttl 600
```


Each command maps to a documented REST endpoint, so the same flow works from your own code or an agent's tool calls. The CLI itself is alpha:` gddy` tracks a rolling release, re-running the installer updates it, and it will sometimes break.


## What we built


With this beta, we are launching domain APIs for the workflows developers hit first: finding a name, registering it, and managing DNS. Domains came first because they sit directly in the path from idea to live project, and GoDaddy already runs that infrastructure at global scale.


**A new v3 Domains API.** v3 is the new surface we built for this launch: availability checks (` POST /v3/domains/check-availability` ), natural-language suggestions (` GET /v3/domains/suggestions` ), and a quote-then-execute registration flow designed for callers that might be scripts or agents rather than people at a checkout page. Not everything has moved to v3 yet. Transfers, renewals, and contacts still run on the existing stable endpoints, behind the same token and base URL (` https://api.godaddy.com` ). v3 will expand with new capabilities and endpoints throughout the beta.


**Scoped authentication.** Every Domains API call uses scoped authentication, either an OAuth access token or a Personal Access Token generated from the[developer dashboard](https://developer.godaddy.com/personal-access-token) . Tokens can be limited to the work they need: read domain data, register domains, update DNS records, or manage other parts of the lifecycle. That matters when the caller is a CI job, a deployment tool, or a coding agent instead of a person at a keyboard. A CI job that updates DNS for certificate renewal does not need permission to register domains. An agent can search for names with read access, then use DNS access when it is time to point traffic somewhere, without ever receiving purchase permission unless you grant it. If a token leaks, revoke that token without rotating everything else.


**Open access.** We've removed restrictions that used to gate these APIs: any GoDaddy account can generate a token and start calling. Rate limits still apply, and they're machine-readable: a` 429` response tells your client exactly how long to back off before retrying. If your workload needs more headroom, tell us and we'll work through it with you.


**Errors with contracts.** Every 4xx and 5xx returns the same envelope: a stable machine-readable` code` , a human-readable` message` , and per-field` fields\[\]` details on validation failures. Match on` code` , never on` message` . Your error handling shouldn't break because we improved a sentence.


**Docs for humans and machines.** The documentation site serves every page as markdown (` /llms.mdx/` plus the page path), publishes the full doc set as one plain-text file at[/llms-full.txt](https://developer.godaddy.com/llms-full.txt) , and ships[OpenAPI specs](https://developer.godaddy.com/en/docs/references/rest) per version namespace. The quickstart's first suggestion is to hand[domains-v3.json](https://developer.godaddy.com/openapi/domains-v3.json) to your LLM as context, so your agent reads the same docs you do.


## How purchases stay safe


An API that spends your money needs clearer expectations — explicit permissions, limits, and confirmation — than an API that just lists DNS records. Agent-driven purchasing is new enough that "trust us" isn't an answer.


v3 separates pricing from execution.` POST /v3/domains/registration-quotes` returns a` quoteToken` with a short expiry and the exact price, and registration executes against that token. If the` period` in the execute call doesn't match the quote, you get` QUOTE_MISMATCH` instead of a surprise on your invoice. The execute call takes the token, a per-attempt` Idempotency-Key` , and a consent object:


```text
curl -s -X POST "https://api.godaddy.com/v3/domains/registrations" \
-H "Authorization: Bearer $GODADDY_PAT" \
-H "Content-Type: application/json" \
-H "Idempotency-Key: $(uuidgen)" \
-d '{
"quoteToken": "qt_abc123...",
"domain": "lemonstand.dev",
"period": 1,
"consent": {
"agreedAt": "2026-07-02T10:30:00.000Z",
"agreementTypes": ["DNRA"],
}
}'
```


The idempotency key is a UUID you generate per attempt. If the request times out, retry with the same key and you get the same registration, not a second one. Registration is asynchronous: poll` GET /v3/domains/registrations/` until the status reaches` COMPLETED` or` FAILED` .


The consent object records which registration agreement types were accepted and when. These values should reflect the actual acceptance flow, not hardcoded placeholders from an automation script. That lets an agent prepare the purchase while keeping the API tied to a legitimate consent record for the required terms.


Notice what the request body doesn't contain: payment details. Registration draws from the payment profile on your GoDaddy account; there is no field for a card number anywhere in the API. If billing isn't set up, the quote fails with` NO_PAYMENT_PROFILE` before any registration logic runs.


## Agents as first-class users


Our design goal was an agent completing a real domain job end to end with the same APIs, docs, and guardrails as a person typing curl.


Give your agent a brief ("a short .dev domain for a lemonade-stand SaaS, under $20 a year"). It can call` GET /v3/domains/suggestions` , check the shortlist for availability and pricing, request a quote, and prepare the registration. The purchase step still depends on an explicit customer approval event captured in the consent record. From there, the agent can submit the registration with an idempotency key, poll until the operation reaches a terminal status, and set the DNS records. Every step uses a token that you scoped and can revoke, and the agent learned from public documentation presented in an LLM-friendly format.


## Talk to the team


This is a beta, and things will change as it matures. The team that built the APIs, the CLI, and the portal is working directly with launch partners and early customers, and we're in[Discord](https://social.godaddy/godaddy-discord-dev) answering questions ourselves. If something gets in your way or if you have ideas to make it better, let us know.


## What comes next


Domains is the first product on the platform, but not the last. Within domains, the next work is moving more of the lifecycle to v3, then going deeper into operations, where GoDaddy runs the infrastructure: configuring, diagnosing, and repairing DNS, certificates, and email routing through the same APIs, so a broken record set or a failed renewal is something you or your agent can fix with a scoped API call. The same platform pieces (tokens, the CLI, machine-readable docs) then extend to more of what a business runs on: hosting, email, commerce.


## Try it out


The[quickstart](https://developer.godaddy.com/en/docs/api-users/quickstart) takes you from token to first API call in a few minutes, and[CLI setup](https://developer.godaddy.com/en/docs/api-users/cli-setup) covers the terminal workflow above. If something doesn't work the way you'd expect, or you need something that isn't there yet, tell us in[Discord](https://social.godaddy/godaddy-discord-dev) . Your feedback is what the beta is for.


We can't wait to see what you build.


Photo by[Fotis Fotopoulos](https://unsplash.com/@ffstop) on[Unsplash](https://unsplash.com/photos/time-lapse-photography-of-a-person-in-a-tunnel-2Ykzp_dFbb4)
