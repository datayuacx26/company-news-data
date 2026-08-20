---
schema_version: "1.0.0"
document_id: "06c6e72631e193cbf68065956d45cf74f4c1ec251d99f6db123e0c53732bea5d"
company_key: "yc-intuned"
company: "Intuned"
source_id: "yc-intuned-news-import-ba3d72b83236"
canonical_url: "https://intunedhq.com/blog/selector-forge"
published_at: "2026-06-19T00:00:00+00:00"
first_seen_at: "2026-07-25T09:50:45.873485+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:6b4f38066c40e36d964cdbd95f67562db1dbc506c7e6c2078acba51207703160"
---

# Introducing Selector Forge: an AI selector builder for the browser

TL;DR: Today, we are releasing[Selector Forge](https://selectorforge.ai/) , a browser extension to create reliable CSS and XPath selectors using AI. It's available today on[Chrome Web Store](https://chromewebstore.google.com/detail/selector-forge-ai-selecto/lbendfnlmhdakbeblajoffkfmafbfaha) and[Firefox](https://addons.mozilla.org/en-US/firefox/addon/selector-forge/) . And it's also open source on[GitHub](https://github.com/Intuned/selector-forge) ! Give it a try and let us know what you think.


## Introduction


At Intuned's core is a coding agent that writes and maintains browser automations. Early on, the code it wrote was already good. The selectors were the weak spot, and not in the way you'd expect. They *looked* right. A bad selector and a good one are nearly identical on the page; the difference only shows up later, when one of them quietly starts matching the wrong element. The agent couldn't see that coming any better than a person can.


And fixing them was not easier. The errors were clear (sometimes), but agents in general can't land on a working selector without rounds of trial and error against the page. Each test is another tool call, dumping useless output into their context. Better prompting barely helped.


To deal with this, we ended up splitting them: the agent decides *what* it wants to grab, and a separate tool decides *how* to grab it. That tool is now the AI engine behind Selector Forge, a browser extension to create reliable selectors: point it at an element and you get a selector that works.


## How does it work? How is it different than what's out there?


DevTools' Copy Selector and the pickers on the Chrome Store all work the same way: walk up the page and grab whatever's there. They can't tell a meaningful anchor from a throwaway one, so they keep everything:


```text
#block-symsoft-content > div > main > div.field.field--name-dynamic-block-fieldnode-solicitation-fields-block.field--type-ds.field--label-hidden.field__item > div > div > div > div > div > div > div > dl >   div  :  nth-child  (  1  )   > dd
```


The worst parts were the throwaway stuff baked right in: a random id like` widget-12i4a3` , or styling classes like` .flex` ,` .text-12` ,` .font-bold` , the Tailwind soup that's all over the modern web. Those describe how the element *looked* the day you clicked it, not what it is. The next deploy changes the id, a restyle drops the class, and the selector is dead while the element hasn't moved an inch.


A good selector keeps only what means something:


**Anchor to what a human chose.**` data-testid="login-button"` is real;` widget-12i4a3` is generated and` .font-bold` is decoration. Telling them apart is most of the job.


**Anchor to the label and you can trust the result.** This is the one I care about most. That element above sits next to a label that says "Solicitation Type," so anchor to the label:


```text
//dt[normalize-space()='Solicitation Type']/following-sibling::dd
```


The result is that we know what comes out: the value beside "Solicitation Type," not "whatever's in the first row today." DevTools' Copy Selector still returns something after the page changes, just the wrong thing, with no warning. This one returns the right value or nothing at all. When you're feeding data somewhere, that's the whole point.


**Keep it minimal.** No positions, no long chains, no extra classes. On Hacker News the right selector for a story link is just` .titleline > a` , buried in the nine-hop chain Copy Selector gives you.


**Make it work across pages.** A selector for "the price" should work on every product page, not the one you're on. Check it against a few, which is also what makes lists work: pick one item and the question becomes "what do these have in common," not "what positions were they in today."


Those four are the whole job, and they're exactly what[Selector Forge](https://chromewebstore.google.com/detail/selector-forge-ai-selecto/lbendfnlmhdakbeblajoffkfmafbfaha) does on every pick, using AI.


## The story: from internal tool to extension


The engine fixed the agent. Our Solution Engineers, the ones building customer automations every day, saw it working there and asked for it themselves, so we wrapped it in a small extension. They know what a reliable selector is worth, and it became their first move whenever they work a page directly: debugging a run, scoping a new site, checking a case the agent flagged. That's when we decided to ship it.


## Try it out


- [Selector Forge on the Chrome Web Store](https://chromewebstore.google.com/detail/selector-forge-ai-selecto/lbendfnlmhdakbeblajoffkfmafbfaha)
- [Selector Forge on Firefox](https://addons.mozilla.org/en-US/firefox/addon/selector-forge/)
- [Selector Forge on Github](https://github.com/Intuned/selector-forge)
