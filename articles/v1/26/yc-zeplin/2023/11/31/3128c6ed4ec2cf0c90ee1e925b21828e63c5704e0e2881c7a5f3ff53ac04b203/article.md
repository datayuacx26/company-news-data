---
schema_version: "1.0.0"
document_id: "3128c6ed4ec2cf0c90ee1e925b21828e63c5704e0e2881c7a5f3ff53ac04b203"
company_key: "yc-zeplin"
company: "Zeplin"
source_id: "yc-zeplin-news-import-9b7fbebec6c2"
canonical_url: "https://blog.zeplin.io/design-delivery/how-to-accelerate-and-improve-product-delivery-outcomes/"
published_at: "2023-11-15T15:12:30.357+00:00"
first_seen_at: "2026-07-24T09:13:15.076143+00:00"
fetched_at: "2026-07-28T21:33:41.699737+00:00"
content_hash: "sha256:d3e5b43fe3b37eefbf845fea138a822451d8c8c826cda7e122f79cc0e0fad8e5"
---

# Accelerating and improving product delivery outcomes in a complex world

A few years ago I went to visit a customer in the airline industry and had the opportunity to sit with the people who designed the very apps I used to book my business travel. As a customer, and vendor, I was fascinated to know how they built the experience that I have come to love (and sometimes hate).


Fast forward to this year’s UXDX EMEA Conference in Dublin, Ireland, I had the privilege to connect with professionals in product, UX, design and dev from a variety of companies and industries.


I still ask people the same question today. “What is the most difficult or challenging part of the work you do?” As I met with people over 2 days at UXDX, the answers I received were all very similar, and were gathered around one key theme; complexity.


Teams today are under pressure to build faster, better products, in a world of evolving technologies, working practices, structures, and team dynamics.


*But they find they are consistently delivering **less** due to increased complexity.*


Complexity exists everywhere you look in product delivery. It’s in our tool stacks, the way we collaborate in our delivery organizations, our company structures, our design systems, and even in our products themselves.


I dove into this idea a whole lot more and I’m sharing my key highlights below for any team wanting to uncover the complexities that are holding them back. And more importantly, you can also learn the solution — a set of foundational product delivery principles that thousands of global organizations like Amazon, Ticketmaster, Bacardi, Givenchy, and Ericsson use to:


- accelerate product delivery outcomes
- simplify the transition from design to development
- improve design system usage and product performance
- and reduce fragmentation in the code base


##
**Product delivery today**


The idea of getting less and less than before — while still paying the same or more — is something often referred to as **shrinkflation** . Growing up, I remember buying a 6 pack of Cadbury Creme Eggs. We’d share them out: 1 for me, 1 for my sister, 2 for mum, 2 for dad.


Today, Cadbury sells them in boxes of 5. So now, it’s 1 for each of my 3 kids, 1 for my wife, and 1 for me. Even though it’s one egg, that’s 16% less! The entire experience has changed.


Modern product delivery mirrors this shrinkflation. Delivery outputs are getting “smaller”, releases contain less than we planned and expected, and the end user experience is taking longer to deliver, even when it’s coming from the most mature and talented product teams. But we all know that’s not the intent.


If you look closely into shrinkflation in the product delivery world, you’ll find teams who have every intention to build beautiful product experiences, but are struggling to handle massive amounts of complexity — in their technology, in user expectations, and at the core of how their organizations must operate in the current economic environment.


### **Too many tools**


We’d all be right to wonder — hey, the tools we use are 10x better, why can’t we ship 10x faster? Well, it’s because our tools are just one part of the equation in product delivery.


Taking one tool away or consolidating tools for the sake of simplicity, for example, doesn't always help, because the problem isn’t having multiple tools. In fact, we should absolutely use and enjoy all the brilliant tools that we do have, like Slack, Jira, Storybook, Figma, FullStory, Dovetail, Maze, etc.


But what we really need is to understand the value each tool gives, and **whether it’s adding or removing complexity** .


### **User expectations and requirements**


User expectations have grown exponentially. They want more and better features and are looking for complex capabilities. The bar is getting higher, which makes it harder for teams to deliver features to match those expectations using our old workflows and processes. For example, we’re now dealing with:


- **Constant comparison** . The “too many tools” problem we were just talking about also means users are endlessly comparing and asking “why can’t your tool be more like X?” or “your competitor does Y, when are you going to introduce that?”. The complexity of competing tools is stifling advances in innovation.


- **Focus on new technologies such as AI** . Everything now needs to be ‘smart’ or ‘AI-driven’.


- **Speed by default** . Users are accustomed to instant gratification in technology, and have come to expect things to load instantly, complete in fractions of a second, and complete right away.


- **Integrations and interoperability** . It’s no longer enough for your tool to work on its own. You now have to consider how it will integrate or work with other sites, tools, and data points, which means you’re now having to take into consideration technical inputs over which you have no control.


- **User journeys.** It’s no longer enough to design and build for just the ideal user journey and perhaps the most straightforward use case. We now have to consider nuances and alternative paths, because users are using tools and apps in more organic ways than ever before. User expectations have gotten more complex, so our work must adapt to that complexity.


### **Working practices have changed**


We’ve all seen the environmental changes that have placed the way we work under greater pressure. It’s not just COVID, either, but we certainly saw a major shift from that point on. In parallel, the nature of product delivery has also changed.


**Teams and product orgs are becoming larger and more layered** , leading to more process, remote working, and collaboration.


**Teams have become more global,** and that’s led to org structures becoming organized around geographies, products, brands, lines of business.


**We are operating in a constant, continuous, always on environment** . Collaboration is done asynchronously across a massive and sprawling scale.


**Delivery methodologies and techniques are changing** and, in some cases, lines are blurring. Designers are developing and managing products. Developers having more input into design.


**There is never just one moment of “handoff” anymore** — teams are in it together from design intent to final product, whether they’re taking ownership of it or not.


All of these changes and complexities have come at us more rapidly than the speed at which most teams have been able to adapt. But there is a way through it.


At Zeplin we’ve analyzed hundreds of companies and, from that, we’ve identified a number of hidden forces that are slowing organizations down due to their complexity, and as a result we have learned how to apply product delivery principles to them. They are; understanding design intent and requirements, managing changes and approvals, and measuring adoption in production.


## **3 hidden forces slowing you down**


I’m a big fan of the Doherty Threshold — a framework that applies to these exact kinds of organizational struggles around process, time, and complexity:


- Too many meetings, messages, and custom processes to understand **design intent & requirements**
- Inconsistent tracking and managing of ongoing **changes and approvals**
- Difficulty **measuring component adoption in production**


In the 80s, Walter Doherty was looking at ways of speeding up how computers were responding and connecting to one another. And what Walter and his colleagues were trying to do was reduce the response time of computers from 2,000 milliseconds — or two seconds to you and I — down to 400 milliseconds. Anything they were able to achieve that exceeded that or bettered the 400 milliseconds was said to exceed the Doherty Threshold.


In the world of UX, the Doherty Threshold suggests that **productivity soars when people interact at a pace that ensures that neither has to wait on the other.**


So the solution to these complexities is in finding ways to reduce the amount that your team has to wait on one another. And actually, these solutions are also[design delivery principles](https://zeplin.io/principles-of-design-delivery/) — simple, foundational rules that help your team overcome the most unproductive areas in their collaboration.


### **1. Communicating design intent & requirements**


It doesn't matter how fast you are able to design something if your team can’t understand it.


We all know how this can play out — designers, developers, PMs, and more folks on your team have to spend valuable time in design review meetings, Slack messages, emails conversations, and more to clarify the things that you might have thought were quite simple.


Even at the UXDX conference, I talked to countless people who have confirmed that this is a pain for them. A lot of it comes from the fact that designers themselves are organizing their work according to their own preference, and those who are receiving the designs don’t have the organization that helps *them* . As a result, everyone wastes time going back and forth to get to the clarity they need, or worse yet, they start working on something even without fully understanding it... and ultimately implement the wrong thing.


#### **The solution: Organize in a standardized way**


We’ve analyzed hundreds of companies across the world who struggle with communicating design intent and requirements clearly, and it’s possible to get this down to 40 minutes.


That’s possible by applying the principles of[standardizing the way you organize your designs](https://zeplin.io/principles-of-design-delivery/standardize-design-organization/) and clearly[separating in-progress design work from finalized](https://zeplin.io/principles-of-design-delivery/separation/) and ready-to-build. By doing this, your team gains the structure and simplicity they need for design requirement documentation, without the frustration and time wasted from lack of clarity.


### **2. Changes and Approvals**


When a piece of design work is done, is it really done? We know that the real question isn’t whether the design work is done but rather…who needs to review and approve it? And what subsequent changes and iterations will we get from stakeholders, product managers, legal, etc.?


Every design inevitably goes through rounds of review that result in tweaks and changes. Some of those may not be huge, but still there’s this ongoing cycle of design updates where the first iteration of a design is rarely the last.


The analysis we've done is that 70 minutes per designer and developer per week is spent going around this iterative cycle, and we believe that teams can reduce this time down to 15 minutes with one very specific thing — tracking changes to shared designs deliberately.


#### **The solution: Track changes to shared designs deliberately**


Making continuous iterations to designs, even after developers have started building, is part of the process. This is the nature of product development today and something we cannot avoid. The way to prevent further complexity, stress, and time (and costly rework down the line) is to adopt the principle of[tracking and communicating changes to shared designs deliberately](https://zeplin.io/principles-of-design-delivery/track-changes/) .


This means being very intentional and specific about publishing changes and making that information known and aware to the people those changes impact — mainly developers, but also PMs, marketers, stakeholders, and more.


Zeplin helps teams be more intentional about change management with our Git-like approach to[design version control](https://blog.zeplin.io/UX-design-version-control) , with unique features like[screen-level version history](https://support.zeplin.io/en/articles/1438841-storing-screen-versions) ,[Version Diff](https://blog.zeplin.io/announcing-version-diff) , and most recently,[component version control](https://blog.zeplin.io/component-version-control) . We also have[Approvals](https://blog.zeplin.io/approvals-in-zeplin) , which adds a valuable layer of organization and visibility around stakeholder sign-offs on UX designs.


### **3. Measuring adoption in production**


When the products you deliver don’t match the design, the immediate thought is to track down what was *supposed* to be delivered? Where’s that source of truth coming from, and how are we measuring how close or far we are to it?


Virtually every established product team today is working from a design system, and according to the main mind behind design systems and thinking — Brad Frost — the source of truth is in production.


> *While a Figma library is super helpful for designer efficiency, the true source of truth for a design system is a library of coded components that build real digital products. After all, the only thing that really matters at the end of the day is the actual user experience human beings interact with.*


The problem is that measuring adoption in production is a monumental effort.


It takes engineers months, not days or hours, to measure what is in production and then work back and see the impact of that in their business. All of this complexity and lack of visibility results in dreaded technical debt that continues to grow.


It’s like the 1-10-100 principle, which says if you catch a bug or error in development, it will cost you an arbitrary $1 to fix.


If you miss that in development and it's not caught until QA, it's going to cost $10 — 10x the amount because you have to fix it, reimplement it, and retest it.


If it makes it into production, the cost grows a hundredfold from that initial problem if you had found it, identified it, and fixed it right up front in development.


This 1-10-100 problem is real, which is why it's so important that you are measuring what is in production and working backwards to ensure that you can eradicate it earlier on.


#### **The solution: Get instant insights**


We’ve been thinking about and working on this problem of design system measurement and adoption for some time and it actually led to Zeplin’s second product,[Omlet](https://omlet.dev/) . Omlet measures component adoption in the codebase, giving your team visibility into that product “source of truth” that Brad Frost is talking about.


We use Omlet to[measure design system adoption](https://omlet.dev/blog/how-leaders-measure-design-system-adoption/) , and so do some great product teams at Optimizely, Instacart, GoFundMe, and more. Anyone can[try it for free for 30 days](https://feta.omlet.dev/login) .


## **Reduce complexity and deliver products faster using foundational principles**


We can’t avoid the complexity in the world around us, or in our tools or user expectations. But what we can do is be more intentional about what types of complexity we ourselves are perpetuating in our day-to-day work and processes when delivering products.


World class teams who deliver products that exceed expectations, use simple and clear processes that are based around[proven product delivery principles](https://zeplin.io/principles-of-design-delivery/) . They realize that productivity soars when people interact at a pace that ensures that neither has to wait on the other.


Zeplin is built around these foundational principles and we help millions of designers and developers ship beautiful products everyday. Their successes give us the confidence that **this is the way** to accelerate and improve product delivery outcomes.[Contact us today](https://zeplin.io/enterprise/contact-sales/) to learn about these principles and how Zeplin can help your team ship more products — faster.
