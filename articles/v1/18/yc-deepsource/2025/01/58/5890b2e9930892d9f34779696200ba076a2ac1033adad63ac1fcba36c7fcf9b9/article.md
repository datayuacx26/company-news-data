---
schema_version: "1.0.0"
document_id: "5890b2e9930892d9f34779696200ba076a2ac1033adad63ac1fcba36c7fcf9b9"
company_key: "yc-deepsource"
company: "DeepSource"
source_id: "yc-deepsource-news-import-7d7bc2aa4aff"
canonical_url: "https://deepsource.com/blog/why-deepsource-sca"
published_at: "2025-01-14T00:00:00+00:00"
first_seen_at: "2026-07-21T16:01:30.648789+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:73a4afc775165ea4ca4cd46d1d880351b1d2859de5dafe4c2e547f71892e46f8"
---

# Why we're building DeepSource SCA

Last updated on Jan 14, 2025


Modern software is built on trust. Every time a developer adds an open-source dependency to their project, they trust not just that package’s code, but the entire chain of dependencies it brings. And they *do* bring a lot of dependencies.


An average JavaScript package depends on 377 additional packages, and 10% of JavaScript packages depend on over 1,400 third-party libraries ([ref](https://i.blackhat.com/USA-20/Wednesday/us-20-Edwards-The-Devils-In-The-Dependency-Data-Driven-Software-Composition-Analysis.pdf) ). The picture looks similar in other programming language ecosystems, if not as bad. A vulnerability anywhere in this chain can compromise your application. This is why securing the software supply chain is critical to your security posture — making Software Composition Analysis (SCA) fundamental to modern software security.


Sadly, though, what began as a critical need—helping teams understand and fix vulnerabilities in their dependencies—has evolved into a complex ecosystem of tools that often create more friction than they remove. What started as a simple security practice has become a tax on development. Teams integrate these tools hoping to ship secure code but end up with a constant stream of alerts and update notifications that are difficult to act on. Developers end up spending more time managing their security tools than actually improving security.


During our research, we heard the same complaints repeatedly.


- A security consultant who's helped dozens of startups told us: **"Most tools just show you vulnerabilities without doing the hard part—helping you fix them safely."**
- A customer managing a large monorepo had to spend two weeks building their own CLI because existing tools couldn't handle their workflow.
- A security engineer from a public company described their frustration with naive remediation suggestions: **"The package upgrades they suggested to fix vulnerabilities would break our code. We couldn't trust them."**
- Perhaps most tellingly, one customer running security for a major enterprise called the current approach **"probably the worst model I've seen in the industry,"** noting that scanning their repositories would cost hundreds of thousands of dollars under current pricing models. Transparent pricing be damned.


We believe there's a better way.


At DeepSource, we've spent the last five years building static analysis tools to for first-party code quality and security. We've learned that the key to making security tools useful isn't just finding problems—it's understanding code well enough to know which problems actually matter. This expertise in static analysis gives us a unique advantage in building a fundamentally better SCA product.


Here’s what we’re doing differently:


### Actually understanding your code


Most SCA tools just match your dependencies against a vulnerability database. We go deeper. We analyze how you actually use these dependencies using AST-based reachability analysis. A vulnerability in an unused module isn’t as critical. As one customer told us, reducing noise by a factor of 10 would be a game-changer. We think we can do better.


### Multi-variate auto-remediation


Most tools take a simplistic approach to updates: they just tell you to upgrade to the latest version. Ask anyone who’s building software with users, and they’ll tell you this isn’t feasible in most cases. One of our customers told us their automated update PRs would "sit in QA forever" because they couldn't trust them.


To solve this, we've built what we call multi-variate auto-remediation. For each vulnerability, we analyze every possible upgrade path, not just the latest version. We show you all your options: maybe a patch version fixes the vulnerability with minimal risk, or perhaps a minor version upgrade fixes multiple issues at once. For each path, we assess the risk of breaking changes and show you exactly what would be affected. It's like having a map of all possible routes, with clear warnings about where the traffic jams are.


### Zero-CI setup


Modern development is complex. Teams use monorepos, have sophisticated CI/CD pipelines, and work in sprints. Security tools need to fit into this reality. As one customer put it, "Existing SCA products really try to force them into some sort of workflow that only exists in a utopian world."


We’ve designed DeepSource SCA to be truly zero config. No CI setup required, or webhooks to be configured. Enabling scanning takes one click. Incremental scans on pull requests automatically detect only new packages. No surprises.


### Fixing the pricing model


The current industry pricing model is "probably the worst model I've seen in the industry," according to one of our customers. It makes it prohibitively expensive to scan large numbers of repositories. We're rethinking this from first principles. Security shouldn't be a luxury.


But perhaps the most important difference is our philosophy. **We don't think security tools should create work—they should eliminate it.** Every alert should come with context and a clear path to resolution. Every update should be as safe as we can make it. Every integration should feel natural.


This isn't just about building a better tool. It's about changing how we think about security in software development. The current model of "shift left" often just means shifting work to developers. We think the real answer is eliminating that work through better automation and analysis.


We're close to launching DeepSource SCA. If you're tired of noisy alerts, broken updates, and tools that fight against your workflow, we built this for you. We'd love to show you what we've built.


[Sign up on the waitlist for early access.](https://deepsource.com/platform/sca)
