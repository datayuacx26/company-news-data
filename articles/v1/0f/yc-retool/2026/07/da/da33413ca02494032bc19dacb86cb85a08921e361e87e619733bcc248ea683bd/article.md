---
schema_version: "1.0.0"
document_id: "da33413ca02494032bc19dacb86cb85a08921e361e87e619733bcc248ea683bd"
company_key: "yc-retool"
company: "Retool"
source_id: "yc-retool-news-import-762698831de2"
canonical_url: "https://retool.com/blog/ai-model-registry"
published_at: "2026-07-30T00:00:00+00:00"
first_seen_at: "2026-08-01T08:01:44.979396+00:00"
fetched_at: "2026-08-01T08:01:46.860344+00:00"
content_hash: "sha256:f3d0c61343728911c8e19519f3def240f9ac2d34a4605978254558e7a0c1eb2e"
---

# Why we built an AI model registry

01 The shape of the problem02 What we built03 Strict producer, lenient parser04 Rate tracking in the registry05 Required, not optional06 Append-only history07 What this looks like for Retool users08 What this looks like for Retool users09 Lessons we learned for future migrations


*This work was a collaboration with Annie Jiao and Claire Lin, who co-led the model registry that made this possible and shaped this post.*


A new Claude model drops on a Tuesday. Within the week, it’s in our codebase and live in our cloud deployment. For a customer running self-hosted on a quarterly upgrade cadence, that same model is still ten weeks from being usable.


In the same week, another provider updates an existing model. Our record of that provider’s rate updates, but a customer’s usage analytics still compute at the old rate because nothing about their self-hosted deployment has changed.


These were two separate frustrations that had been recurring since we integrated AI into the Retool platform. They were the same problem in different costumes, and it took us longer to notice than it should have. Here’s how we solved both by creating an AI model registry.


## The shape of the problem


We ship our platform two ways: as a cloud service we run, and as a self-hosted product that enterprise customers run in their own infrastructure,often for compliance, network isolation, or data residency reasons. Self-hosted customers control their own upgrade cadence, weekly, quarterly, or once a year, depending on the customer. Anything bundled into our binary inherits whichever cadence each customer happens to be on.


Both stories are about configuration that needs to change faster than software releases.` What AI models are available` and` what each provider charges per token` . Both move at the speed of providers (which can drop new models daily), not at the cadence of our binary releases. But because both lived in the codebase, both inherited the release cadence whether we wanted them to or not.


The provider pricing rates had also become more expressive than our schema could describe. They now vary by speed, cache behavior, context window, and other dimensions the original design didn’t anticipate.


## What we built


This is a governance problem before it’s an engineering one: keeping model definitions accurate across every deployment, every rate change, every provider update, without waiting for a binary release. To address this challenge, we built a model registry—a single JSON document that defines every AI model: AI provider, name, capabilities, lifecycle status, and provider rates, served from a Retool-hosted URL, refreshed continuously by every deployment.


Backend instances keep a validated copy in memory and refresh from the hosted URL on a short, jittered interval. If a fetch fails, the backend falls back to its last successful refresh; if no fetched copies are available, it falls back to a snapshot bundled with the build. There’s no scenario where the registry is “down”, only one where it’s “less fresh.” Now, for customers The result for customers: new models arrive weeks later than they should, and internal cost tracking drifts when the provider changes rates.


## Strict producer, lenient parser


Most schemas we write are strict, the parser is the gatekeeper. We did the opposite. Our schema is strict at *publish* time and lenient at *parse* time. A new field can ship in the JSON document without breaking older backends, because older backends silently strip what they don’t recognize.


This sounds backwards until you think about what fails when. Self-hosted deployments lags, sometimes by months, and a newer registry document will land on an older backend eventually. With a strict parser, that payload takes down AI features for the deployment until the customer upgrades. With a lenient parser, the same payload parses cleanly; the deployment just doesn’t see the new fields, and everything else keeps working. We optimized for “no deployment crashes on a newer payload” over “no deployment silently misses a new field.”


The schema is also under version control for backward compatibility. When we need a breaking change, we publish it at a new path, and older backends keep reading their compatible version. Lenient-within-version, hard-cut-across-version: each handles a different failure mode.


## Rate tracking in the registry


Once the registry existed as the source of truth for` what a model is` , we asked: why aren’t the provider’s rates an integral part of that definition?


The answer turned out to be a single TypeScript field:


```text
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
'gpt-4o': {
id: 'gpt-4o',
displayName: 'GPT-4o',
provider: 'openAI',
// …
// Provider rate history, in USD per million tokens.
pricing: {
history: [{
effectiveFrom: '2024-08-06',
effectiveUntil: null,
currency: 'USD',
input: 2.5,
output: 10.0,
}],
},
},
```


```text
TypeScript
```


## Required, not optional


Adding a model entry without provider pricing data fails compilation. The forcing function for “no model without a rate decision” lives in the type system, before code review, before CI. By the time the file even parses, the author has had to think about what the provider charges.


The engineers maintaining the existing pricing system were nervous—every model definition would suddenly fail to compile if rate data wasn't included, and that felt heavy-handed. We argued it through and landed on the strictness being the point, making it impossible for authors to forget the steps that lead to incorrect usage analytics.


In the old system, that runtime signal was the *primary* defense. It paged us. In the new one, it’s a backstop expected to be silent. The failure mode that used to feed it can’t get through the type checker anymore. Any future firing will be a real, surprising bug worth investigating, which is what alerts are supposed to be.


## Append-only history


When a provider raises rates, we cap the existing entry’s expiry date and add a new one, so backfilled usage events still resolve to the rate that was active when they happened. We also reserved a tiers field for context-window-based rate variations. Some providers now charge differently for very long inputs, which we’ll wire up in v1.1 without a schema-version bump.


## What this looks like for Retool users


Now, new models reach every deployment within minutes. Provider rate changes propagate the same way, so internal cost tracking matches the actual provider bill. Model deprecations land cleanly across every feature that consumes a model. The architecture is invisible to customers, but what they get is a frictionless experience of incorporating new models into their instances.


## What this looks like for Retool users


## Lessons we learned for future migrations


- **Configuration coupled to release cycles is a smell.** Whenever something needs to change faster than your binary ships, it doesn't belong in the binary. The version of this we lived with for years was harder to see because we'd been thinking of it as two separate problems.
- **Compile-time enforcement is a different category of guarantee than CI enforcement.** CI catches things; the type system makes them not exist. For load-bearing governance invariants, e.g: "no model without a rate decision" being one, it's the difference between a check and a guarantee. Every rule we’d write in a governance runbook, we now ask whether it could instead be a field.
- **Lenient parsers, strict producers—when deployments lag.** The conventional wisdom is the opposite. The conventional wisdom assumes producers and consumers ship together. When they don't, when an old consumer might encounter a new producer, invert the strictness.


If solving this kind of problem sounds interesting, our engineering team is hiring![Check out our open roles.](http://retool.com/careers)


light Reader
