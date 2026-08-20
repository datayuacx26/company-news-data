---
schema_version: "1.0.0"
document_id: "2f481d98796dfe08a899ec6bad4c73352a8853867017fd79461b69bbf128a730"
company_key: "yc-depot"
company: "Depot"
source_id: "yc-depot-rss-ed70a28fffeb"
canonical_url: "https://depot.dev/blog/github-is-the-wrong-shape-for-this-new-world"
published_at: "2026-07-29T00:00:00+00:00"
first_seen_at: "2026-07-29T13:09:58.343982+00:00"
fetched_at: "2026-07-29T13:10:00.743032+00:00"
content_hash: "sha256:4d5f75b66bf80f3b424afec54059a3ac0e84bf8c9f79cf9a0341f65e9056a51e"
---

# GitHub is the wrong shape for this new world

A lot of attention is given to the overall performance and reliability of GitHub. Rightfully so. But I think the more interesting thing to question is the paradigm that we use with GitHub. It's the wrong shape for how we build software today, and we need better tools, workflows, and infrastructure primitives that meet the new demand.


## Software engineering has changed


I realize this statement is a lot like saying "water is wet."


We live in a fundamentally different world for developing software today. Are there familiar things? Definitely. Are there things from the past that we can use in this new world? Absolutely. Code is still code after all. As a mentor used to preach to me, it's bytes in and bytes out.


The bottlenecks we underinvested in are still around, from source control to CI/CD to code review and even deployments. Only now they are killing the newfound velocity we have.


The pain of these bottlenecks is compounding because everyone can now contribute code. Code has expanded outside of an engineering team and into other teams like sales, support, and marketing. Meaning, a 10-minute build is felt by literally everyone in a company, not just a small subset.


This has profound implications, as it calls into question the assumptions and things we have accepted as normal. It raises questions that I think are still unanswered:


- Who owns the code? Or, put another way, who is responsible for the code that gets deployed?
- How do we trust the code?
- How do we review all of this new code being generated?
- How do we connect code from one place to work with code in another place?
- How do we weigh the costs of token spend with the output of quality features?


Ask these five questions of five different people, and you'll most likely get five different answers.


This is what I mean when I say that software engineering has changed. It's not just that LLMs and agents can generate an entire feature off a five-sentence prompt. That's a tool we are using.


It's that the entire paradigm around software engineering is now drastically different and moving at a velocity that we simply can't keep up with. Assumptions we have made are crumbling with each passing release.


I believe we must rethink our paradigm and the tools we are using. We are bending our existing tools and human-centric paradigms into this new world. This is the wrong approach.


## Collaboration vs infrastructure


Really, this blog post should be titled something like "GitHub, GitLab, Bitbucket, and others are the wrong shape for this new world." But that's a mouthful, so when I say GitHub, read it as all of them.


I think of GitHub as a collaboration tool. Google Docs is a place where I can go to collaborate on this blog post with our technical writing team. GitHub is a place I can go to collaborate with other engineers on the code that makes up Depot.


I grew up with GitHub. In fact, I wanted to work at GitHub when I first became a software engineer. My entire muscle memory is built around the paradigm that GitHub pushed me to use.


- Write code in a branch.
- Open a pull request when I'm ready for my changes to be reviewed.
- Wait for CI and checks to pass to validate that everything works.
- Review comments from my colleagues and go back and forth on ideas in the PR.
- Once everything is green and my colleagues have signed off, merge my changes.


There are derivatives of this workflow. There are wildly differing opinions on the effectiveness of this paradigm. But ask any engineer, and they will know it like the back of their hand.


When LLMs first started to come online, it was natural to bolt them onto our existing paradigm. They could write code in a branch, open a pull request, get a code review from a human (or another agent), use the PR to collaborate via comments, run our tests via our existing CI pipelines, and eventually merge the code.


It was logical. The systems existed, and the agents at that time still largely moved at human speed.


Then everything changed. Agents with the latest generation of models got incredibly good. Freakishly good. We went from models where we often had to nudge the Jenga blocks back into place to get good code out, to models that produce consistently correct code given enough context.


What's the net result of better models? More code, more branches, more parallel work, and more strain on our existing clunky human-powered paradigm.


Our collaboration pattern, and the underlying systems that back it, are now the primary bottleneck. Every engineering team is feeling bottlenecks in GitHub itself, CI, code review, pull requests, security scanning, and even deployments.


There is now an impedance mismatch between our tools and how we want to build. Our existing collaboration paradigm, largely built around GitHub, is now in conflict with how we actually build software today.


We must rethink our paradigm for software delivery. Not through the lens of human-centered collaboration, but through high-throughput infrastructure primitives.


## High throughput software delivery


Building a new software delivery paradigm requires taking a step back from the human-centric process that we have used over the last decade. Rethinking software delivery, not as a sequence of human actions, but as a continuous automated process driven by systems.


Once you start imagining that world, the problem of software delivery starts to look less like a collaboration tool and more like infrastructure. Where infrastructure, in this context, means a small number of primitives on which everything else can be built. We can look to how we have adopted cloud-native technologies for inspiration:


- Compute as a primitive gave us VMs
- Storage gave us object stores
- Networking gave us load balancers


We should be thinking about the fundamental infrastructure primitives we need for software delivery:


- **Source control** : durable, immutable history of how code evolved
- **Execution** : isolated, performant compute for building, testing, and validating changes
- **Artifacts** : reproducible outputs that can be moved across systems
- **Caching** : reuse of deterministic work
- **Identity** : proving who or what produced code
- **Policy** : machine-enforceable rules for quality, security, and compliance


These aren't features of a developer tool. They're infrastructure primitives. Just as we stopped thinking about provisioning individual servers and started thinking about compute, storage, and networking as composable building blocks, software delivery needs the same shift.


The winners of the next decade won't build a better pull request. They'll build the infrastructure that makes software generation, validation, and deployment operate at machine scale.


## Related posts


- [What we need from CI for agentic engineering](https://depot.dev/blog/ci-for-agentic-engineering)
- [The bottleneck has shifted from writing code to integrating it](https://depot.dev/blog/the-bottleneck-has-shifted)
- [Staying in control of your codebase in the AI era](https://depot.dev/blog/staying-in-control-of-your-codebase-in-the-ai-era)


Kyle Galbraith


CEO & Co-founder of Depot


Platform Engineer who despises slow builds turned founder. Expat living in 🇫🇷
