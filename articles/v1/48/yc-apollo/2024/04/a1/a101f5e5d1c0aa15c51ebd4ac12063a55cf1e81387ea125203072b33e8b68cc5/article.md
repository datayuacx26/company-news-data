---
schema_version: "1.0.0"
document_id: "a101f5e5d1c0aa15c51ebd4ac12063a55cf1e81387ea125203072b33e8b68cc5"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-wayfair-achieved-graphql-success-with-federated-graphql"
published_at: "2024-04-10T09:15:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:40113910b8afb2e7406bbc979012141e97efa2271c36bbada9711ef84516903f"
---

# How Wayfair achieved GraphQL success with federated GraphQL

Wayfair is one of the world‘s leading home goods brands, offering 14 million items from more than 11,000 global suppliers. With the help of[Apollo Federation](https://www.apollographql.com/docs/federation/) , Wayfair implemented a unified schema that became a cornerstone of the company’s larger technical transformation.


At[GraphQL Summit 2023](https://www.youtube.com/watch?v=sNqcow1psEA&list=PLpi1lPB6opQzUOqG3QroLLN06FF-Q_uhX&index=25) , Leah Hurwich Adler, Associate Director of Storefront Platforms at Wayfair, shared the secrets of Wayfair’s GraphQL success. In this post, we’ll explore how the Wayfair GraphQL platform team developed a product mindset to build community, establish governance, and learn how they approached technical challenges when supporting suppliers, partners, customers, and internal developers. Let’s dive in!


## Context and challenges


Before Wayfair incorporated a federated graph, its teams used GraphQL in an enormous PHP monolith — with unsustainable results.


“The monolith was so unwieldy that we hit a ceiling in our velocity, and hiring more engineers didn’t help us push code to production faster. We needed to fundamentally change the way we built software to continue to scale our business,”


– Leah Hurwich Adler, Associate Director of Storefront Platforms at Wayfair


To address the issue, the company opted to break down the monolith into domain-oriented microservices, and it quickly became clear that a federated GraphQL architecture would be an ideal solution to unify their APIs.


In the company’s previous implementation of GraphQL, many applications failed to leverage the basic compositional[architecture best practices](https://www.apollographql.com/docs/technotes/TN0027-demand-oriented-schema-design/) typically used in GraphQL. Instead, they simply mirrored the backend structure in their GraphQL schemas. These patterns negatively impacted Wayfair’s endpoint performance, leaving many of its teams with GraphQL trust issues. The GraphQL platform team at Wayfair knew that to successfully launch a federated graph, they would need to build trust again by prioritizing education to facilitate a cultural shift. In other words, the “people problem” was as important as the technological problem when laying the foundation for successful platform and tooling adoption.


This is where[Apollo Federation](https://www.apollographql.com/docs/federation/) came in. Using a[supergraph](https://www.apollographql.com/docs/resources/glossary/#supergraph) , Wayfair could implement a modular, decoupled approach for service teams while providing simplicity of the monolith for client teams. Apollo Federation allowed Wayfair’s service teams to maintain APIs on separate servers called[subgraphs](http://r/) , which combine into one schema to form a supergraph.


“Lucky for us, the decision totally paid off,” Leah said. “Today, Wayfair has a graph that sees 220 million requests a day from 80 different subgraphs. We have a community of approximately 500 users, and we’re growing about 5% month over month.”


– Leah Hurwich Adler, Associate Director of Storefront Platforms at Wayfair


Figure: Metrics for Wayfair’s federated graph ecosystem supported by Apollo Router


So, what were the secrets to Wayfair’s overall GraphQL success?


## Four key principles for GraphQL adoption


### **Be patient**


Remember: no technological transition happens overnight. “The need for patience remained constant throughout our experience,” Leah explained. Whether you encounter false starts, overturned proposals, or pushback from actors who don’t understand the platform vision, maintaining patience will help you push through the noise.


“It was helpful to keep iterating and focus on playing the long game. We didn’t need everyone to like all of our proposals; we just needed enough buy-in to keep going. We celebrated big and small wins, enjoyed each other’s company, and never took ourselves too seriously,” said Leah.


### **Know your users**


Investing in your users – understanding the challenges your teams face, and discovering the benefits of your platform – lets you strategically prioritize your efforts across how you invest in platform integration, education, developer tooling, and community, enabling you to deliver value to your users sooner. Spending time getting to know your users, similarly helps ensure you tailor your messaging to the right audiences. With the right research, you can tell a more compelling story about how your graph delivers value — and ultimately generate more leadership buy-in.


### **Meet in the middle**


Another principle that helps drive success is engaging senior leaders alongside your users. Senior leaders can serve as highly influential advocates to help garner greater support within the organization. In Wayfair’s journey, Leah found that “through teaching senior stakeholders the value of GraphQL, we \[could\] more effectively support the many early adopters who helped us make our vision a reality.” By spending time understanding senior leaders, you can show them why GraphQL is an investment in the future of their organization.


Leah’s team found that by investing in educating the leadership, teams aspiring to be early adopters experienced less friction, making it easier for them to prioritize adopting the graph, and enabled her team to grow their user base more rapidly.


### **Set clear expectations**


Finally, it’s imperative to set clear expectations around your GraphQL adoption process from start to finish. The more you articulate your working models, what success looks like, and which areas of opportunity still exist within the platform, the more trust you can build. Setting clear expectations gets all your teams on the same page, and strengthens your ability to deliver results.


## How to apply the four principles in your journey


Wayfair developed a clear stakeholder communication framework to effectively engage different groups for GraphQL federation.


“We found it best to tailor our communications based on our audience’s perception of both the value of our graph and their current graph proficiency,” Leah said. “Most of Wayfair started as skeptical novices. They didn’t have much exposure to federated graph best practices and were speculative about whether it was worth the hassle. It was most helpful to stress the value federation could provide when talking to such audiences.”


Over time, Leah and her team moved the needle with these groups, who became enthusiastic about the prospects of GraphQL federation. The team focused on teaching best practices to convert these individuals into experts — and they ultimately became the most powerful tool for influencing other groups. Utilizing user testimonies can help organically convey the value of federated graph, and while generating buy-in from skeptical experts is never easy, displaying the success of early platform adopters can show them how the platform is capable of scaling. However, the most impactful approach was allowing the early adopters to organically evangelize the benefits of the federated graph, as hearing firsthand accounts of improved outcomes from trusted peers carried immense weight in convincing skeptical experts.


### **Phased adoption of federated GraphQL**


The tactics Wayfair used to understand its users, invest in stakeholder relationships, and set clear expectations differed in each phase of GraphQL federation.


#### **Design phase – aligning architecture and engaging stakeholders**


“In our design phase, we worked across technology organizations to align our graph and platform architecture. We didn’t have a platform team yet, so we worked directly with individual contributors from other teams to build and align the future of our graph,” Leah shared. She and her team used two working groups: a tiger team to build the federated gateway, and a group of senior architects across Wayfair to align the end-state architecture. This collaboration allowed Wayfair to learn about its users while architecting the foundational platform. It also helped the company develop a group of well-regarded advocates early in the GraphQL journey.


To effectively engage senior leadership, these working groups ensured that they updated CTO staff and directors on their progress, demonstrating a commitment to investing in senior leadership to the same extent as they invest in their users. They created transparency and solicited feedback from key stakeholders on the end-state architecture and go-to-market strategy — encouraging them to feel more comfortable with the platform. This step built trust in the value of federation and early advocates’ ability to execute, eventually leading to a budget for a full platform team.


Leah and her team set the expectation early in the design phase that the transition to graph federation would come with cultural changes. They made leaders and key contributors aware upfront that teams would be responsible for owning their services while contributing to a shared graph. Crucially, they emphasized the need to “move slow to move fast” by investing time and effort in learning best practices upfront, rather than iterating through mistakes. This proactive approach aimed to get buy-in from teams to accept a potentially slower initial pace for delivering features, in order to build a solid foundation through proper education.


#### **Rollout phase – hands-on, working side-by-side with pilot teams**


During the rollout phase, the platform team built the foundational platform, developer tooling, and community touchpoints needed to reach general availability of their graph. They prepared to move from a closed ecosystem they could control to a more unpredictable open ecosystem.


“During this phase, we worked side-by-side with pilot teams to help them deliver \[their desired\] business outcomes using federation. Taking this hands-on approach ensured that our platform and educational materials met our users’ needs — and helped impress potential users of the value of our platform,” Leah said. Wayfair needed support from both pilot subgraph teams and their clients’ leadership to buy into the value of GraphQL. So the platform team leveraged the senior leader relationships they had established during the design phase, expanding their network to include leaders of directly impacted teams, such as API clients, and tangentially impacted teams, such as upstream backend services. In doing so, they connected with potential users ensuring they could directly address any concerns and convert skeptics into enthusiasts.


In the rollout phase, the team also set expectations around goals for their pilots. They made it a two-way street: the platform team would help pilots deliver important business wins, but they expected them to contribute to the platform in return. This included championing federated GraphQL within their own organizations. Many of the initial pilot participants played critical roles in up-leveling their organizations and contributing to Wayfair’s graph governance bodies.


## Looking toward the future


“Now that we’ve reached general availability, anyone can contribute to the graph. We have more opportunities to unlock value at scale,” Leah shared. “We can prioritize our roadmap based on \[more\] users’ needs and use it to validate new features in our larger platform strategy.” The four key principles — be patient, know your users, meet in the middle, and set clear expectations — were the secrets to Wayfair’s graph federation success with[Apollo GraphOS](https://www.apollographql.com/graphos) . Check out Leah Hurwich Adler’s[session at the GraphQL Summit 2023](https://www.youtube.com/watch?v=sNqcow1psEA&list=PLpi1lPB6opQzUOqG3QroLLN06FF-Q_uhX&index=25) to learn more. If you’re ready to begin the transition to Graph federation with Apollo,[contact](https://www.apollographql.com/contact-sales?referrer=nav-contact-sales) one of our representatives today!


*Note: Leah Hurwich Adler also contributed to this blog post.*


Written by


Ishwari Lokare


[Read more by Ishwari Lokare](https://www.apollographql.com/blog/author/ishwari)
