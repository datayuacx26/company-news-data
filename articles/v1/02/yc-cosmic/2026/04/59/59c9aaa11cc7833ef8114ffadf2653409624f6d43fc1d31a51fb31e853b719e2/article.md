---
schema_version: "1.0.0"
document_id: "59c9aaa11cc7833ef8114ffadf2653409624f6d43fc1d31a51fb31e853b719e2"
company_key: "yc-cosmic"
company: "Cosmic"
source_id: "yc-cosmic-atom-acd624fed976"
canonical_url: "https://www.cosmicjs.com/blog/cosmic-rundown-wordpress-backdoors-stacked-prs-mdn-drops-react"
published_at: "2026-04-14T00:00:00+00:00"
first_seen_at: "2026-07-20T23:23:40.519323+00:00"
fetched_at: "2026-07-28T22:00:13.771809+00:00"
content_hash: "sha256:496b675206e5034acef599548c5a6dc49c0ba14b015a71d53ae4c0a114707061"
---

# Cosmic Rundown: WordPress Backdoors, Stacked PRs, and MDN Drops React

## 30 WordPress Plugins, One Backdoor


Someone acquired 30 WordPress plugins and[planted backdoors in all of them](https://anchor.host/someone-bought-30-wordpress-plugins-and-planted-a-backdoor-in-all-of-them/) . The attack vector is straightforward: buy abandoned or neglected plugins with existing install bases, push a malicious update, and wait for automatic updates to do the rest.


This isn't a new tactic, but the scale is notable. The[Hacker News discussion](https://news.ycombinator.com/item?id=47755629) raises the obvious question: how do you vet plugin ownership changes? WordPress.org doesn't currently notify users when a plugin changes hands. The community is calling for better transparency around plugin acquisitions and more aggressive code review for post-acquisition updates.


For teams still running WordPress, this is a reminder that plugin sprawl creates attack surface. Every plugin is a dependency you're trusting with your site's security.


## GitHub Ships Stacked PRs


After years of community requests and third-party workarounds, GitHub officially launched[Stacked PRs](https://github.github.com/gh-stack/) . The feature lets you chain dependent pull requests together, making it easier to break large changes into reviewable chunks without blocking on merge order.


The[discussion](https://news.ycombinator.com/item?id=47757495) is extensive. Developers who've used Graphite, ghstack, or similar tools are comparing notes on how GitHub's implementation stacks up. Early reports suggest it handles the basics well: automatic rebasing, visual dependency graphs, and cleaner review workflows.


For teams practicing trunk-based development or working on large features, this removes a significant friction point. No more manually rebasing stacked branches or explaining to reviewers why PR #3 depends on PR #2 which depends on PR #1.


## MDN Drops React for Web Components


Mozilla published a deep dive on[rebuilding MDN's frontend without React](https://developer.mozilla.org/en-US/blog/mdn-front-end-deep-dive/) . The new architecture uses web components and vanilla JavaScript.


The reasoning is practical rather than ideological. MDN is largely static content with pockets of interactivity. React's runtime overhead didn't justify itself for the actual user experience. Web components provide encapsulation where needed without shipping a framework.


This isn't an argument that React is bad. It's an argument that tool selection should match the problem. Documentation sites, marketing pages, and content-heavy applications often don't need the full capabilities of a component framework.


## DaVinci Resolve Adds Photo Editing


Blackmagic announced[photo editing capabilities in DaVinci Resolve](https://www.blackmagicdesign.com/products/davinciresolve/photo) . The video editing suite now handles RAW photo processing with the same color science tools video editors already trust.


The[discussion](https://news.ycombinator.com/item?id=47760529) centers on whether this threatens Lightroom's position. Resolve is already free for most users. Adding photo editing to an already-capable video toolchain creates an interesting value proposition for creators who work across both mediums.


## Google Tackles Back Button Hijacking


Google Search is rolling out a[new spam policy targeting back button hijacking](https://developers.google.com/search/blog/2026/04/back-button-hijacking) . Sites that trap users by manipulating browser history will face ranking penalties.


This dark pattern has plagued the web for years. You click a search result, try to go back, and find yourself stuck in an infinite loop of the same page. Google's enforcement mechanism remains unclear, but the policy change signals that this behavior will have SEO consequences.


## Quick Hits


**Jujutsu gains momentum.** Steve Klabnik's[jj tutorial](https://steveklabnik.github.io/jujutsu-tutorial/introduction/what-is-jj-and-why-should-i-care.html) hit the front page. The Git-compatible version control system continues attracting developers frustrated with Git's complexity.


**Renewables pass natural gas.** For the first time,[US renewables generated more power than natural gas](https://e360.yale.edu/digest/us-renewables-natural-gas-coal) . The crossover happened in March 2026.


**Backblaze drops cloud folder backups.** Backblaze quietly[stopped backing up OneDrive and Dropbox folders](https://rareese.com/posts/backblaze/) . Users discovered their cloud-synced files weren't being backed up as expected.


**Introspective diffusion models.** A new paper on[introspective diffusion language models](https://introspective-diffusion.github.io/) proposes an alternative to autoregressive generation. The approach lets models "think" before committing to output tokens.


**Formal verification has limits.** A developer[found a bug in code that Lean proved correct](https://kirancodes.me/posts/log-who-watches-the-watchers.html) . The issue wasn't with Lean itself but with the specification. You can prove code matches a spec, but proving the spec matches intent is a different problem.


## What This Means for Content Teams


The WordPress backdoor story underscores why decoupled architectures matter. When your content lives in a headless CMS with a clean API, your frontend choices don't introduce CMS-level vulnerabilities. Your content infrastructure stays stable even as you swap out rendering layers.


MDN's move away from React reinforces the same principle. The best architecture is the simplest one that solves your actual problem. A headless CMS that delivers content over API lets you make that choice per-project, without rebuilding your content layer.


---


**Building something?**[Start free with Cosmic](https://app.cosmicjs.com/signup) and keep your content infrastructure clean.
