---
schema_version: "1.0.0"
document_id: "9e9768a48236e4d4091a85a4533ab80fa1865a259dbd1b004446cbbbc81d1565"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/10-best-practices-for-schema-stewardship-part-2-of-2"
published_at: "2021-08-30T19:28:35+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:d045abc94bbd3da1341476f95d4dfafe1575b9d397960321d1b08119afaa0732"
---

# 10 Best Practices for Schema Stewardship (Part 2 of 2)

Welcome to part 2 of our blog series exploring the best practices for guiding the development of GraphQL schemas at your company. In[part 1](https://www.apollographql.com/blog/community/graphql-champions/10-best-practices-for-schema-stewardship/) , we reviewed the first five best practices, and here we’ll cover the second five. If you’d prefer a lean-back version of all ten, check out a[recording of my talk on this topic](https://www.apollographql.com/events/tech-talk/thank-you/establishing-your-schema-governance-best-practices?referrer=blog) .


GraphQL schemas capture a *shared* and a *unified representation* of our data, services, and digital capabilities. In these posts, we’re exploring how to best reach agreement on that representation, how to best balance the needs of all the parties, and how to achieve the outcomes we seek while avoiding creating a process bottleneck that slows us all down and becomes ignored.


Above is the full list of best principles I’ve identified based on my experience working with GraphQL champions. We’ll cover the key concepts involved in any collaborative exercise, understanding, empathy, encouragement, reflection, iteration and models of organization–the topic of best practice number six.


## **Best Practice #6 – Keep iterating your organizing model**


#6 – As with evolution, the organizing model that works “best” will change over time. Choose a model that meets your current needs while iterating with agility as those needs change.


Collaboration is a multi-team sport so choosing the right organizing model is key. Yet, no model is perfect, and we often inherit them, so the best practice is to keep improving yours. Let’s review four models we’ve seen companies adopt and look at opportunities for iteratively improving their effectiveness.


### Organizing Model 1 – Siloed


Siloed Model


In the siloed model, each team adopts GraphQL for their own reasons. Expertise is developed within each team and each team defines their own schema review and governance process. Without taking time for cross-team coordination, each team gets a quick start. Yet without any sharing or coordination, lessons learned on one team don’t benefit the next. Since this model leads to multiple disconnected graphs and no cross-team best practices, companies that stay with siloed graph organization models find those early wins don’t lead to sustained value and certainly not to strategic company-wide value.


### **Organizing Model 2 – Graph Guild**


Graph Guild Model


Teams that begin with the siloed model can improve sharing and accelerate learning by forming a guild or a community of GraphQL experts from across the company. Sharing best practices, advice and learning about each team’s work provides an opportunity to look for common purpose. The challenge is that volunteer-only guilds depend on a few key individuals who must divide their focus between stewarding the graph and their other responsibilities. Graph guilds and community efforts are essential, but they are hard to keep vibrant and engaged, which is why many companies decide they need to dedicate resources to the work.


### **Organizing Model 3 – Central Graph Team**


Central Graph Team Model


In some cases, a platform team is responsible for building out the first graph as the need to centralize expertise, set standards, and define a schema review process is apparent early on. In other cases, a central graph team is formed after exploring the siloed or graph guild models. In either case, the creation of a central graph team marks a significant increase in maturity level for a company because it indicates they recognize the graph as a strategic asset that justifies dedicated resources. Yet, there’s a challenge with this model, and it’s inherent to its central nature. It’s a bottleneck.


The desire to concentrate GraphQL expertise in a central team creates a company-wide dependency on a single team. Schema collaboration and reviews require a centralized intake process. The graph team’s slack channel is a buzz of questions and requests for help. In the beginning, this is a good thing, but over time, as the graph becomes more broadly adopted and more central, the load on this team reaches an unsustainable level. If you’ve ever been on a central graph team as I have, it’s both exhilarating and overwhelming. So while it’s fantastic to have dedicated folks who tend the graph all day long, this is a hard one when the organization gets large — it just doesn’t scale.


### **Organizing Model 4 – Hybrid**


Hybrid Model


The centralized bottleneck of a single graph team reminds me of the bottleneck that results from a large monolithic graph. So it’s unsurprising that the solution to both is the same–a federated model. In the hybrid organizing model, the central graph team works in partnership with experts embedded in each team. That expertise can arise in two ways. One is a rotation model in which experts learn on the core team and then embed in local teams. This can also be reversed, local teams rotating one of their teammates into the central team to gain experience.


In either case, the essential ingredient is that the local experts work very closely with the central team and work together on the shared goal of stewarding the larger effort. Since that work is distributed, this model scales and the rotation model helps prevent the burnout that can occur when developers spend too long on a central “service” team. This model also helps ensure key members of the shepherding community are close to the problem and don’t fall prey to an ivory tower perspective. And finally the Graph Stewardship team has a special role for a graph product owner, more on that idea below in best practice #9.


### **Audience Poll**


When I polled my talk audience about the organizing model in use at their company the results came out as I guessed they might. The most common model is the siloed model 1, and each subsequent model is slightly less prevalent. This nicely supports the idea that there is a progression these four models as companies adjust and tune their organizing model to fit the needs of their growing graph.


Poll results for question “What organizing model is your company using today”?


Whichever model you’re currently using, there will be aspects that are working well, and others that could work better. The best practice here is to keep exploring and iterating toward models that help you reach your strategic outcomes and avoid the[tragedy of the commons](https://www.youtube.com/watch?v=CxC161GvMPc) , where each team is optimizing locally but there is no unified strategy or vision. Reflect on what’s working and let the friction of what isn’t guide your next step.


## **Best Practice #7 – Scale your review processes with async reviews and schema tools**


#7 – Avoid creating a bottleneck and endless re-reviews by adopting a distributed and asynchronous review process centered around use-case driven schema proposals. Leverage tools to automate schema checks and schema mocking to unblock teams stuck debating theoretical benefits.


Too often, the approach for making a group decision is to schedule a meeting. Yet with individuals and teams spread across time-zones, companies that use async schema reviews of schema proposal documents create the opportunity for more feedback while also reducing the review cycle time. Since a schema’s value is measured by its utility to clients, it’s essential these documents capture the client use cases with UI screenshots if appropriate. In this way, the reviewers have the needed context and you will increase the quantity and diversity of reviewer feedback. On the tools front, leveraging automated schema checks and linting tools help flag issues early and at the individual developer level, before the formal schema review.


To summarize, here’s a list of things that will help scale your schema review process:


- Conduct async, distributed reviews of schema proposals as these scale better than meetings and encourage more diverse input.


- Ensure schema proposals include full use cases and alternate schemas considered.
- Leverage mocking for schema iterations and Apollo Workbench for federation explorations.
- Integrate automated schema and linting checks into your CI/CD process to catch issues earlier and upstream of in-person schema reviews.


## **Best Practice #8 – Pay special attention to a team’s first schema review**


# 8 – First contact is a crucial time to engage, explain your graph strategy and enlist them to your cause. Initial reviews should be mandatory, subsequent reviews can be generally opt-in.


A team’s first GraphQL schema review is a special time. First contact, I call it. Before the formal review make sure you spend the time to communicate with the new team on your schema style guide, but more importantly, on the larger goals and objectives for your graph. Providing the detailed thinking behind your schema guidelines is key because it answers the **why** , without which schema style guides can appear arbitrary. Sharing the bigger picture goals and taking the time necessary to answer their questions or doubts helps enlist their support to the larger cause.


This brings us to another key decision, whether schema reviews are optional or mandatory. I’ve talked to companies that have explored both models and the evidence I’ve reviewed suggests that for new teams contributing their first major schema, mandatory reviews are the best choice. Once the overall shape of the types and operations is reviewed and the choices align with the larger graph goals and guidelines, subsequent schema evolution can be optional. Yet even then, consider the benefits of holding open reviews for all schema changes that are significant or “interesting”–such as a new approach to error handling or server-driven UI schemas. Rather than thinking of this as a “gate” that must be passed, think of it as an opportunity to spread learning and create consensus across teams.


To recap, first contact is the time when you have the most opportunity to set mindsets, explain your graph goals and forge consensus. Investing extra time up-front will pay off many times down the road.


## **Best Practice #9 – Adopt a product mindset**


#9 Think of your graph, and their schemas, as a product to manage–with its own customers, roadmap and success criteria.


Common best practices of graph stewardship and product management


When building this list of practices it struck me that four of the practices: Understand your customers; Lean into the friction; Iterate; and Adopt a client-centric mindset sound exactly like best practices for another discipline, product management. And that makes sense, Both are cases where a small but influential group of individuals seek to guide toward a better product-market fit. They use understanding and empathy to effectively partner and build consensus, they use iteration and experimentation to explore the solution space and the model by which decisions and software are made. And in all ways, they must keep primary focus on the end customer.


If you think about your unified graph’s schema the way a product manager thinks about a unified family of products, with customers, stakeholders, success criteria and a roadmap, it reveals why it’s worth putting all this time and effort into effective collaboration. It also justifies the need for dedicated staff focused not on the infrastructure of the graph, but stewarding the direction and ensuring the goals of the graph align with the objectives of the organization and the needs of the customers.


And don’t forget to take another page from every good PM’s playbook: self-promotion. Just as PMs find any and every opportunity to promote their product and its importance, trumpetting every minor improvement–so too should you look for every bit of supporting data, and every anecdotal success story to reinforce the pivotal role the graph plays within product development. If that doesn’t come naturally, partner with a product manager by promoting your graph vision to them first and inviting them to help you apply a mindset they know well.


## **Best Practice #10 – Keep moving forward**


*#* 10 – Championing change can be daunting, time consuming, and sometimes exhausting. Yet all great progress requires curiosity and courage in equal parts. When blocked, route around, find allies, and always look for new opportunities to move forward!


I’ve saved this one for the last because it’s the one that matters most. In the graph, we see the promise of a unified representation of our company’s work that we can build and evolve together–a means to avoid the repetitive cycle of duplication that takes so much energy away from true innovation. Delivering on that new promise asks for new perspectives, new approaches and a new alignment between the twin powers of the front and backends teams. That doesn’t always come easy and it can be a lot to ask.


Yet two things should give us encouragement to keep pushing forward. *First* , we know well what the alternative holds. Building rich digital experiences, quickly, without a unifying graph is no longer sustainable. Team after team and company after company have come to the same conclusion–the graph is a critical new layer of the stack. *Second* , the past decades has made clear that even paradigm shifts are best tackled iteratively. Just as the journey to cloud native development didn’t happen in a single step, neither will the creation of a unifying graph. That didn’t stop or even slow us in our adoption of the cloud and it won’t with GraphQL, as long as we keep moving forward, adjusting our approach and sharing our learnings with one another.


Each roadblock we encounter, be it technical or organizational, is an opportunity to learn and improve our next approach. It’s trite but true that we learn far more from setbacks than from quick wins. It’s also true we learn more from each other than on our own. So this is a call of encouragement but also a call for community. Reach out to connect with our Graph Champion community atchampions@apollographql.com . We’d love to hear about your graph journey, share our encouragement, connect you with your peers and help the community keep moving forward!


Written by


Dan Boerner


[Read more by Dan Boerner](https://www.apollographql.com/blog/author/danboerner)
