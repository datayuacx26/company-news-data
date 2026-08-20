---
schema_version: "1.0.0"
document_id: "6618b62dcaf78b5e1cfae28cc1b71aac49b1dd4085a7629911dd915ab1450a13"
company_key: "kaltura-inc-common-stock"
company: "Kaltura Inc."
source_id: "kaltura-inc-common-stock-rss-8a80d100aa25"
canonical_url: "https://medium.com/kaltura-tech/simplifying-complexity-how-an-architecture-team-streamlined-our-product-journey-c8ef37803ba1"
published_at: "2024-07-04T08:02:25+00:00"
first_seen_at: "2026-07-20T23:18:51.911067+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:c6f7f09e6511d46186fb8c435c53ae2b860ca410a9cb96466e85cad78ada77eb"
---

# Simplifying Complexity: How an Architecture Team Streamlined Our Product Journey

# **Simplifying Complexity: How an Architecture Team Streamlined Our Product Journey**


[Mizrachi Shai](https://medium.com/@mizrachi.shai?source=post_page---byline--c8ef37803ba1---------------------------------------)


6 min read


·


Jul 3, 2024


--


Press enter or click to view image in full size


Like many agile software companies, Kaltura Media & Telecom’s product development was based on direct communications between product managers and developers. While effective in smaller setups, this breaks up at larger scales, leading to inefficiencies and solutions that are not adaptable to changing requirements nor meet the cost and scale requirements.


In this blog, I will explore the journey of migrating from a direct approach to a more holistic approach, overcoming challenges along the way. If you are an executive thinking about such an organizational shift or you’re involved as a participant in such a product-system architecture-development chain, then read along to get some important insights.


## **Initial Situation — a Direct Approach**


Prior to the System Architecture team’s existence, we had no technical design function on the product side of the business (requirements went straight from product to development). So, there was no team ensuring that what we built would be adaptable to changing requirements, adhere to our long-term technical strategy, and be built according to the best technology practices of the industry. As the company evolved and more independent sections were set up that needed to be orchestrated, a team with an end-to-end broad view had to be introduced.


We were a software organization still evolving from a startup and looking to implement the mature delivery processes expected from a company providing products to medium and large-scale companies.


Another crucial impact was that our process did not allow for feature and system documentation. Product managers have high-level product knowledge derived from their product requirements, while programmers do not write system documentation. They concentrate on documenting their code. As a result, a feature could have been developed without end-to-end documentation describing how it should be operated, and how exactly it was implemented.


On top of the purely technical aspects mentioned above, there were also human aspects that did not work effectively. Product managers sometimes lack the deep technical knowledge to fully understand development trade-offs and implications, leading to exhausting discussions between people who lack a common technical vocabulary. Programmers want to get mature and accurate requirements and be excused from such discussions, which seem to them like a waste of time. So, these discussions can often lead to an intense atmosphere and, eventually, impact the final product quality, causing it to be degraded.


## **Introducing the System Architecture Team**


To address these challenges, a new system architecture team was introduced, a bridge between product vision and technical design. Someone who could speak both languages fluently.


This linked chain demonstrates where the new system architecture function came in and emphasizes the strong collaboration required between all parties involved.


Press enter or click to view image in full size


While it is an inevitable step towards a much better process, it comes with its own challenges. The good news is we successfully tackled those. Let’s see how we did it.


## **Overcoming Challenges in the New Process**


### **Proactive Development**


Our main concern was whether the new process would slow us down and, if so, whether we would be committed to it when time-to-market pressure was tight.


The workflow included a new architecture step that required extra time to understand the product requirements and design the best way to implement them inside a large, complex product.


Did it slow us down? Not in retrospect. We found that it might have delayed how long it took for the first line of code to be written, but on the other hand, accelerated the date at which the code passed QA and would be ready to deploy. If we invested the time upfront in getting a mature design, we would eventually save time on bug fixes and refactoring that would be required if we didn’t do this work, not to mention the frustration of such refactoring iterations. Product development professionals call it a “shift left” in the product development process, causing it to be a more robust and scalable product.


Additionally, to retain a swift process for smaller features, we implemented a quick analysis procedure. In it, a system architect evaluates a feature at a high level to decide if system architecture is required to make a full low-level design for that feature. If not, it could be handed quickly and directly to the developers. If, at any point, anyone felt like the feature was getting complicated, they could raise a flag to pull in system architecture.


### **Team Synergy**


Sometimes, people are reluctant to make changes when they are in their comfort zone. Introducing the new team also raised personnel concerns. Some programmers felt threatened, fearing the architects would take over some of their decision-making and, by that, limit their freedom. Product managers were worried about losing direct control. To overcome these concerns, the architects had to use sensitivity and soft skills. Especially in the early stages, architects had to emphasize the collaboration path rather than imposing or enforcing solutions. The balance between listening to others while still pushing for the desired solution type had to be found each time.


Additionally, with all the technical knowledge that had been built up within the development team, we knew that the new venture was only ever going to succeed if the system architecture team could leverage the experience and knowledge of the dev team. So from the outset, collaboration between architects and programmers was essential.


If you fail to coordinate, consult, and plan properly, you break the linked chain (above diagram). You can end up with something like this …


Press enter or click to view image in full size


We made sure to consider continuous feedback to improve the end-to-end workflow. On top of regular ongoing feedback that was encouraged, we also held formal discussions, and each stakeholder was able to express their thoughts on what was good and what they thought could be better. Integrating this feedback into the formal process helped to gain trust in the new team and the new process.


### **Process Commitment**


A crucial part was management commitment. If the accountable senior manager is persistent in his obligation to the new workflow and backs it up, all stakeholders feel a sense of commitment and try to embrace the process.


Patience and persistence are key to the integration of new procedures and their success. This is true in general and especially for cases in which a new team is created and inserted as a new step in an existing workflow. It’s one thing to create a set of new processes and ask people to follow them, but it takes time for the culture change to follow when people really buy into those processes.


## **Final Conclusion and Points to Think About**


Even though moving to a new process and introducing a new system architecture function in the product development chain can feel like a big challenge, the payoff of increased productivity and getting more done makes it totally worth the effort.


So, how can you measure if the transition is a success? Obviously, you can see that the features developed and the overall product structure look better, backed by a common understanding of all stakeholders that we see a fruitful process and a better product.


Maybe a better sign of success is the product and development personnel’s new behavior. If a complex feature is proposed, the product managers now insist on pushing it via the system architecture pipe because they understand that it would be better handled that way. And the programmers now insist on getting a detailed low-level and mature system requirement they can work with. They know that the design was carefully built and will likely not change on the go, so they will not have to refactor a code they already wrote just because someone didn’t think of a proper solution or because the product team suddenly understands they want it otherwise.


What is next? Well, processes keep evolving, and the proper balance of what should traverse the full new chain and what can take the shorter direct path is constantly reevaluated. Seeing the new workflow’s success, other development teams that were not part of the initial process are now expressing interest in adopting it as well, so the team is being pulled into extra areas and responsibilities.


What is the proper balance for you? That is for you to decide based on your organization’s size and culture. Are you willing to take the journey and make such a transition? As a main conclusion from our journey, the real question is, are you willing not to take it?
