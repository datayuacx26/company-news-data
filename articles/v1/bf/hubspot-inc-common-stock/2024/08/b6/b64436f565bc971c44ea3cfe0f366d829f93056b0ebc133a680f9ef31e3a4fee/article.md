---
schema_version: "1.0.0"
document_id: "b64436f565bc971c44ea3cfe0f366d829f93056b0ebc133a680f9ef31e3a4fee"
company_key: "hubspot-inc-common-stock"
company: "HubSpot Inc."
source_id: "hubspot-inc-common-stock-news-import-815987d58dd2"
canonical_url: "https://product.hubspot.com/blog/grow-better-product"
published_at: "2024-08-20T00:00:00+00:00"
first_seen_at: "2026-07-21T23:16:14.524102+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:b6421c6cc55eb5f845a4a4a98dc2ca67efc022519055b8c3e6d9cea0d82c9c57"
---

# Growing Better in Product: the Importance of Collaborative Culture

*Written by Dana Land, Deliverability Consultant @ HubSpot*


In October of 2023, the email industry held its breath as[Google and Yahoo announced new sender requirements](https://blog.google/products/gmail/gmail-security-authentication-spam-protection/) that would go into effect in February of 2024. Email sending services and marketers alike started buzzing with questions, concerns, and general unease.


- What exactly was Google and Yahoo requiring?
- Who would be expected to make those changes?
- What happens if you don’t meet the requirements?


HubSpot’s Deliverability team sits within the Product and Engineering org. We were familiar with the requirements as they had been best practices in the deliverability space for years. This allowed us to spring into action to ask our own question: “ *how can we make compliance with these new requirements easy for us and our customers?* ”


We had 3 months to change fundamental processes, as well as design, develop, test, and deploy features that if done incorrectly could prevent our customers from delivering their mail- no pressure, *right* ?


Product Managers spun up project doc after project doc and checked for dependencies, engineers worked with Deliverability to discuss all of the nuanced changes needed currently and to anticipate what might be needed in the future, and designers started thinking of the most user-friendly ways to display this complex information. We were successful and delivered our changes ahead of schedule at the end of December and through January.[One of these being the new email sending domain wizard which shows DMARC, DKIM, and SPF status.](https://knowledge.hubspot.com/marketing-email/manage-email-authentication-in-hubspot)


Working on such a massive project with such a short deadline and still being able to deliver a great tool is not easy; but we did have a very important factor on our side: a strong culture of cross-team collaboration. Email Deliverability, Email Sending Infrastructure, and all other Email-based teams work exceptionally close to each other and often participate in team bonding activities to strengthen their ability to collaborate. This was not our first project together, nor will it be our last. Even when not actively working on a product release together, we often find ourselves consulting with each other, engaging in hang outs and coffee chats, and getting together in person when the chance presents itself.


So when it comes time to work on a stressful project, we find it easy to support each other and deliver a great product for our customers.


*Caption: Dana Land, Amber Olofson, and Amara Ellis (not pictured) of Email Deliverability, Jackie Brenna of Email Sending Infrastructure, and Hannah Roberts of Connected Email (not pictured) get together in Dublin for lunch to discuss future product ideas and to strengthen their cross-team bonds.*


Any Product & Engineering team looking to be able to deliver under tight deadlines when working cross functionally can benefit from building positive relationships with members of other teams. I interviewed fellow members of HubSpot’s Email Sending Infrastructure team, Email Deliverability team, and Messaging Product Design team about how building a culture of collaboration has impacted them.


Jackie Brenna - Product Manager of Email Infrastructure and Email Services


Q: The topic area of email authentication and all of the RFC standards for email sending were still pretty new to you when this project kicked off. You had to go through a crash course on what exactly Google and Yahoo were requiring within the tight deadline of 3 months, very similar to what our customers were experiencing. How were you able to leverage and collaborate with other teams to help put the pieces together? Did our strong culture of cross-team collaboration help you feel comfortable asking questions?


> “I really cannot put enough weight into how vital our cross team collaboration was to make this project happen. When the requirements came out I had minimal understanding of DNS and deliverability. This project was an opportunity for me to dive in the deep end, and you, as a member of the deliverability team, were instrumental in expediting that learning process. Our one-on-ones really became a safe space for me to learn, grow, and ask questions. Without our collaboration I would not have been ready to make the critical product decisions that had to be made in a timely manner.
>
>
> What really made our collaboration so successful was a mutual understanding and respect for one anothers expertise and roles. We were all focused on solving for the customer and knew that the best way to do that was in leveraging each other's skill sets. Instead of feeling competitive with one another, or trying to prove how smart we were, we built each other up and helped one another succeed within the confines of our jobs. I really believe this was only possible because of the strong foundation that already existed between our teams.”


Q: As the primary PM on many of those releases, you needed to get buy-in and commitment from teams that you do not usually partner with on such short notice, as well as keep all of the different teams aligned towards a common goal. You did amazing at this. If you had to give advice to PMs experiencing tight deadlines and dependencies on outside teams, what would you say?


> “I found that the most influential thing I could do to gain buy-in from other teams was to have clear documentation about the impact these projects would have - or more so the impact of not doing them. Beyond that, I think it’s important to remain flexible in your plans, be open to feedback, and try to lead with empathy. I learned a ton from this experience and personally hope to do better at all of these things the next time I have an opportunity to run a large scale project like this."


**Ali Dehghani - Senior Software Engineer II on Email Sending Infrastructure**


Q: You were one of the engineers who not only worked on building the tools for this release, but then supported our internal stakeholders as they learned the tool by answering their questions and helping them confirm expected or unexpected behavior of the tool. It’s not often software engineers jump into internal support channels to field daily questions, and that assistance made a huge difference to Email Deliverability and our internal stakeholders and their customers. What was the motivator for you to jump into an internal support channel to help answer questions? Did you find it provided additional value to you, as an engineer, as you continued to refine and update the release?


> “Absolutely, that's a great point. Jumping into internal support channels isn’t something every engineer gets to do, but for me, there were a couple of key motivators.
>
>
> Firstly, being involved in the support channel gave me a unique perspective on how our tools were being used in real-world scenarios. Sometimes, when you're deep into development, it's easy to get tunnel vision and think purely in terms of code and functionality. But actually hearing from the internal stakeholders, understanding their challenges, and seeing how they navigate through the system provided invaluable insights. It was like getting a front-row seat to the user experience.
>
>
> Another huge motivator was the immediate feedback loop. Normally, we build something, release it, and then wait for feedback to come through various channels, which can take time. By being directly in the support channel, I could see firsthand where people were struggling or where the tool was excelling. This real-time feedback was instrumental in allowing me to iterate quickly and make needed adjustments. Plus, knowing that my involvement was directly helping someone solve their problem was incredibly rewarding. It made the long hours of development feel worthwhile.
>
>
> As for the added value, absolutely, it was beneficial. Engaging directly with the users of the tool gave me a deeper understanding of their needs and pain points, which helped me refine and improve the product in ways I might not have considered otherwise. It also deepened my empathy for the end-users, a quality I think every engineer should cultivate. By seeing and hearing their issues firsthand, it helps you build better, more intuitive solutions in future projects.
>
>
> It also fostered better collaboration and communication between the engineering team and other teams. When stakeholders know that you’re approachable and willing to help, it builds trust and strengthens the working relationship. Plus, it often led to more nuanced and valuable feedback that you might not get through formal channels alone.”


Q: This release had a lot of moving parts and a lot of different teams sharing feedback and asking for development changes- a lot of cooks in a pretty small kitchen so to speak. Even the best collaboration can be noisy sometimes. When you are collaborating with multiple different teams who may have competing priorities, how do you set expectations with those teams on what you can and cannot do? Do you have any advice for new engineers on how to remain focused when there are a lot of competing priorities pushing you in different directions?


> “Yeah, I totally get what you’re saying—when you’ve got a lot of different teams involved, it can definitely feel like you’re in a small kitchen with a lot of cooks. It's a tricky scenario, but quite common in dynamic work environments.
>
>
> So, the first thing I’d say is communication is key. When you're dealing with multiple teams, it’s essential to have clear communication channels. It helps to set a framework at the very beginning of the project. For instance, you can have regular meetings where each team can voice their priorities and updates. This way, everyone is on the same page, and it becomes easier to manage expectations. It’s really about creating a shared understanding of what can and can’t be done.
>
>
> For setting expectations, I’d recommend being upfront and transparent. Let the teams know what your capacity is and what the realistic timelines are. If there are any limitations or potential bottlenecks, don’t shy away from discussing them.
>
>
> As for new engineers, staying focused amidst competing priorities can indeed be challenging. My advice would be to prioritize tasks based on impact and deadlines. Learning how to distinguish between urgent and important tasks is crucial. Also, it’s important to be comfortable saying no when necessary. If you're stretched too thin, it's better to be honest about it rather than overcommitting and underdelivering.”


**Lewis Royal - Design Manager for the Messaging Product Group**


Q: Much like the Product Managers and the Engineers, you had a very short deadline to update the design for a very important feature: our email ending domain connection wizard. In what ways did collaboration and the relationships you’ve built across teams help you achieve your goals with the design while still meeting the tight deadlines (if at all)?


> “This was actually my first time working this closely with a lot of the collaborators on this project. But there is a mutual respect and trust that everyone is an expert in their space, and that we all have a common goal in trying to solve for the customer. So when you get critique or pushback, you know it’s got the customer’s best interest at heart.
>
>
> I’m relying on my partners who know the space best to help me build my own knowledge. Everyone was so open to answering my (constant) questions, which helped me translate complexity into simplicity for our customers who are not experts. Email Deliverability helped distill the requirements into something I could understand, Product Designers for Domains gave guidance on common patterns for setting up DNS records, and Engineers helped clarify how the underlying mechanics work and ensure we covered all edge cases.
>
>
> As a cross-functional team, it felt like we were building our knowledge together, daily, in pursuit of making things as simple as possible for the customer.”


Q: For the wizard, you were getting feedback from multiple teams- Email Sending Infrastructure, the Domains team, and Email Deliverability. That is a lot of people from different areas that may have been pointing you in different directions. Do you have advice for other designers who may be juggling many stakeholders across different teams? How do you ensure that collaboration remains helpful and not a hindrance to your process?


> “Get something on paper - when working through a complex topic, I find sharing visuals can break through the complexity, spark conversation and highlight areas of misalignment. So I shared low-fidelity wireframes with stakeholders early in the process, which helped us iterate quickly and make trade-off decisions as we went along, as well as squeezing out all the edge cases and ‘what if’ scenarios that might otherwise get missed.
>
>
> Lean into async - being in different time zones and teams, asynchronous communication was our friend. Having stakeholders comment directly in Figma files, starting (sometimes very long) slack threads, and taking shared ownership for updating project scope docs not only meant we could keep momentum on decisions, but also ensured progress was well documented for others to see. It saved having to rely on syncs to stay on the same page, and so instead syncs were used to hash out the harder problems.”


**Amara Ellis - Senior Deliverability Consultant on Deliverability Operations**


Q: There is no shortage of major changes in the email industry, most recently with the new Yahoo Sender Hub. This additional release from Yahoo has also seen our team needing to make changes to our foundational processes. How has Deliverability Operations partnered with our other email teams to ensure our customers see success with the new requirements that come along with Sender Hub?


> “The new Yahoo Sender Hub is a major change that requires senders to make modifications to their DNS to continue receiving spam reports. However, this would have resulted in thousands of customers having to go through the process manually, and DNS changes typically don’t go smoothly.
>
>
> By partnering with other teams outside of Deliverability such as Ali Dehghani - Senior Software Engineer II on Email Sending Infrastructure and Jackie Brenna - Product Manager of Email Sending Infrastructure and Email Services, we were able to come up with an alternate solution during the beta phase of the program and test our solution well-ahead of the retirement of the old feedback loop program.
>
>
> This change ensures that none of our customers have to make DNS changes on their end and continue with business as usual when the old Yahoo Feedback Loop program is retired on August 1st. The success of this implementation was critical on working cross-functionally with other teams, in both making the technical changes from an Engineering perspective and ensuring that we followed the strict timeline and had an organized project plan with Jackie and Ali’s help!”


Q: How has having built a strong foundation for collaboration with other Product & Engineering teams strengthened your ability to react and meet the needs of Inbox Providers like Google and Yahoo? Do you think the strong collaborative culture we’ve built between Deliverability and Email Sending Infrastructure supports our network’s ability to help our customers deliver their emails?


> “In my daily work, nothing is ever the same, and this requires me to be constantly adaptable, like MacGyver. However, I’m only able to do so because of the collaborative environment that exists between Deliverability and Email Sending Infrastructure. While I’m able to do the research and analysis on different types of Deliverability issues, I’m by no means a SQL expert or an Engineer able to build dashboards and tools from scratch to detect anomalies within our network.
>
>
> On any given day, I can ask for help developing a new query to investigate a new issue, provide input on an existing sending configuration for improvements, or strategize our next steps in taking on any major changes revealed by ABC mailbox provider. The Email Sending Infrastructure team can also detect irregularities or performance issues that I may have missed because they have a different perspective of the network than I do, resulting in a more thorough and well-rounded monitoring system. Working with Jackie on the product side further guarantees a concise strategy in addressing any major MBP changes by setting clearly defined goals and timelines when beginning a new project, as well as clear internal and external communication to support customers and senders.
>
>
> When we tell customers that our networks are monitored 24/7, it means that multiple teams, not just Deliverability, are fully invested in the success of our customers' email experiences.”


Are you ready to make an impact? Check out our[careers page](https://www.hubspot.com/careers) for your next opportunity! And to learn more about our culture, follow us on Instagram[@HubSpotLife.](https://www.instagram.com/hubspotlife/)
