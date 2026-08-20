---
schema_version: "1.0.0"
document_id: "d9a17654bbb14fd5df898534e3540e66be9c95e3d5150df28b5c81a2c670d888"
company_key: "yc-convictional"
company: "Convictional"
source_id: "yc-convictional-atom-2bd8494c6cc9"
canonical_url: "https://get.convictional.com/posts/vibe-marketing/"
published_at: "2025-08-06T00:00:00+00:00"
first_seen_at: "2026-08-09T21:02:07.552290+00:00"
fetched_at: "2026-08-09T21:02:10.293363+00:00"
content_hash: "sha256:8bb4717506d5b54f057814d8d050e65a832626b2d687754942d8f68180482bad"
---

# Vibe Marketing

#


## Making the switch from CMS to Claude Code


I began my career as a software engineer. I loved the creative aspects of the job. My favorite type of work is building. My professional role now is marketing, market development and strategy. When[Claude Code](https://claude.md/) (an agentic command line coding AI tool) came out, it awakened the latent engineer inside of me. I fully embraced vibe coding as a creative medium and as a way to learn. Soon after, we made the decision to completely redo our marketing site here at convictional.com with Claude Code at the center.


Before we were satisfied with our process and tools. We had leveraged external design and dev help and hosted our site on Webflow. Today, our website is now statically generated and hosted by us. We manage the source code within our GitHub mono repository, alongside our product code. We use VitePress as the static site generation framework, and we largely use Claude Code to make changes to content.


## A marketer in developer territory


As someone primarily focused on marketing and customer work, it may seem odd to be working in developer tools to do common content management tasks. But I absolutely love it. The learning curve wasn’t too steep (you literally can just ask Claude to teach), and the payoff has been tremendous in terms of both capability and efficiency. Managing the hosting through Google Cloud has been fairly straightforward, and this marketer has now learned a lot.


We now use Claude Code as a central tool for managing our marketing website.


I of course still rely on the support of our engineering team, but the experience has taught me about our software development process, git workflows, how our product is developed and deployed, etc. Beyond the side benefits of technical learning I think the experience has given me a lot of empathy and respect for my engineer teammates’ work. I feel much more on the same page as them now when part of product and engineering discussions.


## The power of integrated workflows


We get the built-in benefits of our development workflow using GitHub, so changes to the website are managed as pull requests where we can collaborate, review, and approve as a team. We can make very quick content, style, as well as functionality changes using Claude Code as an assistant in a way that we could never do before in a SaaS website CMS tool like Webflow. This very essay is a markdown file deployed as a pull request, and automatically deployed to our staging environment and then production when merged into our main code branch. I do all the writing, but Claude (with well written CLAUDE.md files) helps do the rest, like generating SEO meta tags.


Github pull requests aka PRs, static site generation, and github actions take the place of CMS publishing workflows.


Another benefit is that version control gives us a complete history of every change made to our website, making it easy to roll back changes or understand why certain decisions were made. This level of transparency and accountability is difficult to achieve with traditional CMS platforms.


Just yesterday, it took me less than an hour to implement a new form on our website and integrate it via a webhook to our CRM, kicking off a lead workflow internally. This kind of rapid implementation would have previously required coordination across multiple teams and potentially days of work, let alone distracting our engineering team away from the important product work they’re focused on.


## Looking to the future


Looking forward, I expect this new freedom and creative power will make our marketing website much more engaging, up-to-date, and relevant for our customers. Because the code for our marketing website co-resides with our application product code, in the future we expect to do things like build automated workflows that react to new product features introduced to the codebase to automatically suggest updates to the relevant parts of our marketing website.


The other benefit is that we can leverage the same styling, so over time our marketing website and our product both leverage the same style code. And it makes sense because, if you think about it, a marketing website is really just one part of our product experience. I actually prefer to think about the marketing website, and my work on it, as an extension of the product we offer our customers.


## Broader implications for the SaaS industry


We're early adopters here at Convictional of AI tools, and as a small team, we are often eager to change our processes and tools so that we can work and scale more effectively. I think our story might indicate implications for some broader market trends away from frankly truly great SaaS tools like Webflow. But having leveraged my current stack with Claude Code and GitHub, I think this is what the future looks like when it comes to web content management.


The cost savings are a nice side effect but wasn’t a factor in our decision. There is some trading of SaaS fees for AI inference costs but over the long run I see this as quite a big savings for the organization and the costs won’t scale as we add teammates into our process.


The democratization of developer tools through AI assistants like Claude Code is enabling non-technical team members to participate in previously technical workflows, fundamentally changing how cross-functional teams collaborate.
