---
schema_version: "1.0.0"
document_id: "440234af85bc5addfa48bf5ead13acccf33bcf9a5b33b93ccbefa9c2927698a0"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/series-a-funding"
published_at: "2024-06-27T11:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T21:00:09.778529+00:00"
content_hash: "sha256:079deb5bbc79c1313e228ceda2d3ec156e18fdcbc4089ee5998aeaff1714c69e"
---

# AuthZed Raises Series A Funding from General Catalyst

## Series A Announcement


If you’re a regular reader of our blog, including my posts, you know that we usually talk about very technical concepts, in a way that we hope is approachable. Today, I want to talk about something completely different! The good news is that with this announcement, you’re guaranteed many technically interesting posts in the future!


Today, I’m proud to announce that AuthZed has secured $12 million of Series A fundraising! This is our first additional capital since our original[Seed funding](https://authzed.com/blog/funded) back in early 2021. Before I get into the details of the funding, I thought it might be fun to take a moment to reflect on how we got here.


## Our (Brief) History


Jimmy, Joey, and I started AuthZed to scratch our own itch. In our past lives, we built a product called[Quay](https://quay.io/) , the first private Docker registry. When it was time to add authorization to Quay, we did what everyone does: we quickly wrote some code to add a very basic role-based model to the product which allowed for point to point sharing of repositories between users.


That didn’t last us very long: we were quickly inundated with requests for organizations, teams, nested teams and namespaces, default permissions, and so on and so on. Every time we had to change the permissions model we were **terrified** . Permissions code is security critical by definition, and each time we took a stance on how the code should be written, we were proven wrong by the next request. There were actually features that we never shipped due to the difficulty of the required authorization changes. Additionally, our naive implementation was far from scalable: far too high a proportion of our underlying database’s CPU was spent on computing authorization.


Of course, we explored other ways to do authorization. We tried to leverage our authentication provider; but they weren’t in the authorization game. We tried to embrace the concepts of ABAC and explore policy engines; but our data relied on deep context and relationships between users, other users, and the data, which necessitated doing most of the work for the policy engine ahead of time. We even tried to create something like AWS’s IAM; it didn’t map nicely to our domain with point-to-point user-driven sharing.


In 2019, Google published[“Zanzibar: Google’s Consistent, Global Authorization System”](https://zanzibar.tech/) commonly known as the Zanzibar paper. The paper details how exactly Google accomplished authorization as a centralized service across the vast majority of their products. After reading the paper, I was **hooked** . Finally, a way to scalably and flexibly manage authorization. In 2020, we formed AuthZed to commercialize the concepts in the paper, and we’ve been doing just that ever since.


In 2021 we joined YCombinator’s W21 batch, and shortly after that, we raised $3.9m in seed funding, led by Work-Bench, with participation from YCombinator and Amplify Partners. We used that money to build and launch several popular, successful projects and products.


First, in 2021 we launched a multi-tenant, permissions systems as a service at authzed.com. Then, in October of that same year, we open-sourced the underlying Zanzibar implementation behind authzed.com:[SpiceDB](https://github.com/authzed/spicedb) . SpiceDB is the most faithful implementation of the Zanzibar paper outside the walls of Google. As of the time of writing this, it has over 4,500 stars on GitHub and over 2,300 members in our[Discord community](https://authzed.com/discord) . Leveraging our learnings from building and running authzed.com, in the summer of 2022, we launched a single-tenant IaaS-style managed version of SpiceDB that we call[Dedicated](https://authzed.com/products/authzed-dedicated) . Most recently we have added another product called[Materialize](https://authzed.com/blog/materialize-early-access) which riffs on the concepts of materialized views in traditional relational databases, but reapplied to the permissions domain.


Our products are really resonating with the market. In the short time since we first launched, we’ve grown AuthZed at an incredible rate, achieving millions in revenue, and more than tripling each year. With such clear signs of product-market fit, we knew it was time to put our foot on the accelerator.


## Series A


Now for the juicy details! We’ve raised an additional $12 million of funding, led by General Catalyst, with continued participation from Work-Bench, YCombinator, and Amplify Partners. We couldn’t be happier bringing General Catalyst on board as our newest investor and partner. In case you haven’t heard of General Catalyst, they’re a multi-stage VC firm that has invested in companies you may have heard of, like: Stripe, Canva, Gusto, and Snap. Their connections and experience are already proving invaluable as we transition to this next phase of the company.


As we’ve grown and evolved our product and team, we’ve been very thoughtful about the bets that we’ve made. We’ve made pretty big bets on being authorization only, and using the Zanzibar design model as our fundamental thesis. This approach has taken us quite far, allowing us to land amazing customers like Canva and Turo, as well as receiving open source contributions from engineers at organizations like GitHub and Reddit.


Our Series A funding will enable us to build and commercialize our next round of big bets. First we plan to bring our[Materialize](https://authzed.com/blog/materialize-early-access) product to general availability so that everyone will be able to take advantage of denormalization and incremental view maintenance with relationship-based access control (ReBAC), accelerating checks and providing an access control change stream.


The next hypothesis that we have, and bet that we’re planning to make, is that more companies than just large enterprises would love to be able to utilize the AuthZed Dedicated approach to authorization. To that end, we’re also working on a version of that offering that’s more approachable for smaller and medium sized businesses.


## Why now?


I love when people ask why we’ve raised so little, and so slowly. In a VC backed company, this is usually a sign of an unhealthy company, one that might not have the growth or reach to raise more money. This additional funding brings our total raised to $15.9 million to date, which would be considered a small seed round for one of the hot AI companies that dominate the news today.


The reality is that as second-time founders, our approach to venture capital and company building is slightly different to most of the other companies out there. We want to make sure that we’re building a sustainable business, and that means that we’ve made two strategic decisions that have impacted the timeline and amount of our funding. First, we want to grow mostly out of our own revenue, only bringing in venture capital to accelerate something that’s already working, and make new big bets. Secondly, we’ve been very judicious with our hiring. The overwhelming pattern during the COVID ZIRP years was to over-hire, and then when the market forces shifted, to panic.


Where most companies might try to raise money when things aren’t working, at AuthZed we’re raising money because things are clearly working.


## Building The Best Product With The Best Team


It has been an absolute privilege to get to build another critical piece of infrastructure with such an amazing team. The people who make up AuthZed are the most talented people I have ever worked with. I want to thank each and every one of them for helping get the company to where we are today.


This also applies to our investors. The fine folks at Work-Bench, YCombinator, and Amplify partners have been hugely helpful, from navigating tricky business decisions, to connecting us with great talent. The fact that they have all chosen to participate in our Series A, shows how much conviction they have in our mission and execution to date.


The last group I want to thank are our customers. In some ways, they’re making the biggest bet of all on a young company that is approaching the permissions problem in a slightly different way. Anytime someone tries to commercialize a concept that came from Google, there’s always a question of whether that approach works at all outside of the halls of that venerable institution. Our customers have shown that the need is real, and have helped us forge the path forward together.


## Boldly Go


Our company shares deep roots with[CoreOS](https://www.redhat.com/en/about/press-releases/red-hat-acquire-coreos-expanding-its-kubernetes-and-containers-leadership) and our past mission to secure the internet through automatic updates. In some ways, we’re just continuing that mission at AuthZed, helping to secure the internet by enabling companies to build rich, secure sharing experiences. Behind every digital interaction lies a permission: negotiating access decisions between entities. Federation is a fundamental part of what makes the internet an interesting place, and helping our customers' end users securely share their most sensitive information with confidence and precision drives us forward.


I am excited to take the next step of this journey with you all together!


- Jake


On this page


- Series A Announcement
- Our (Brief) History
- Series A
- Why now?
- Building The Best Product With The Best Team
- Boldly Go


## Related


[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)[Community Agentic AI Foundation Names AuthZed's Adora Nwodo and Sohan Maheshwar as Ambassadors Adora Nwodo and Sohan Maheshwar from AuthZed have been selected as official ambassadors for the Agentic AI Foundation (AAIF), joining the program's inaugural cohort. Melissa Smolensky · Jul 9, 2026 · 4 min](https://authzed.com/blog/authzed-agentic-ai-foundation-ambassadors)


[Engineering Authzed joins the CNCF A return to the ecosystem we helped trailblaze Aug 17, 2022 · 2 min](https://authzed.com/blog/cncf-membership)[Engineering Authzed joins the CNCF A return to the ecosystem we helped trailblaze Jimmy Zelinskie · Aug 17, 2022 · 2 min](https://authzed.com/blog/cncf-membership)


[Community Authzed Raises $3.9M to Solve Application Permissions Space Challenges Authzed raises $3.9M to solve application permissions issues. Lead investor Work—Bench, with support from Amplify Partners, YCombinator, and angel investors. Learn more on our team page. Apr 21, 2021 · 2 min](https://authzed.com/blog/funded)[Community Authzed Raises $3.9M to Solve Application Permissions Space Challenges Authzed raises $3.9M to solve application permissions issues. Lead investor Work—Bench, with support from Amplify Partners, YCombinator, and angel investors. Learn more on our team page. Jake Moshenko · Apr 21, 2021 · 2 min](https://authzed.com/blog/funded)
