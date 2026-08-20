---
schema_version: "1.0.0"
document_id: "25c3225b8a618de3c5758af64122e5c4fcdbe6d5251b50d75666fd39797ade99"
company_key: "yc-supernova"
company: "Supernova"
source_id: "yc-supernova-rss-864f3bee1480"
canonical_url: "https://www.supernova.io/blog/components-building-the-building-blocks-panel----audience-q-a"
published_at: "2024-02-12T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:40.084347+00:00"
fetched_at: "2026-07-28T22:26:15.382818+00:00"
content_hash: "sha256:b02fa7ecdef442a0271f3f48db85f02278e93ce280b8b22c0f1c7dc69def7e1e"
---

# Components: Building the Building Blocks Panel — Audience Q&A

What are key principles to keep in mind when designing and development components? How do you handle versioning and rolling out new components? How do you create components that are flexible and scalable yet still consistent?


We answered these questions and more in our expert panel["Components: Building the Building Blocks"](https://www.supernova.io/components-building-the-building-blocks) with industry leaders[Alissa Engstrom](https://www.linkedin.com/in/alissaengstrom/) (Lead Product Designer at Apollo.io),[Dmitry Belyaev](https://x.com/blvdmitry) (Principal Front-end Engineer at Booking.com), and[Giles Perry](https://www.linkedin.com/in/gilesperryuserexperience/) (Director, Product Design at Skyscanner).


If you missed the live panel, you can[watch the recording](https://www.supernova.io/components-building-the-building-blocks) from the discussion. We got a ton of audience questions that we weren’t able to answer during the talk, so our panelists have graciously shared their insights below.


Let’s dive in!


#### **Q: What are some good and bad ways to approach communicating across teams to advocate for and roll out your design system?**


**Alissa:** I’ve had the best success in approaching collaboration from the lens of being a support agent. I let those I'm collaborating with know that I am there to work with them to support their needs. After all, they are my users.


When a new designer joins the team, I always make sure to meet and talk to them 1:1, not just about design systems but personally to get to know them. It helps build trust, and I have found I get less pushback later on when I ultimately need to tell them no or to use another solution.


I also practice a lot of active listening when I join product meetings or jams with co-workers. I listen to their wants and needs and tie those back to what other teams and the business need.


Additionally, I hold workshops and education sessions. The best way to get people to use the system is to show them HOW to use it correctly and its capabilities that will help them.


One of the worst approaches would be to be too prescriptive. To over-govern your co-workers. The best way to make a design system unsuccessful is to not be flexible and work with the needs of the feature squad and company needs


**Dmitry:** Some of our biggest wins resulted from collaborating with the teams, discussing their needs, and creating and executing actionable plans. It's tempting to focus solely on developing the system, which can lead to losing touch with the product's needs, but this approach is not beneficial in the long term.


The most effective shortcut is to maintain communication focused on the problems you are solving for the product teams and to demonstrate how you accomplish this. A significant challenge in this process is the lack of context that other teams have compared to those building the design system. What may seem stable and easy to adopt for us might appear difficult and time-consuming to others.


A few years ago, we met with a team using a custom component library. To address assumptions about difficulty, we created a demo showcasing our offerings, concluding with a quick recreation of one of their pages made in just 30 minutes before the meeting. While it wasn't thoroughly tested or was implemented perfectly, this approach significantly helped with our storytelling.


#### **Q: How to convince stakeholders to use time for building design systems in the startup environment?**


**Alissa:** When trying to convince stakeholders of anything design-related, I tie the reasoning back to the company. How will this design system help the company? Will it help the team work faster in the long run?


One of the ways I have had success in convincing the stakeholders to create a design system is by suggesting we build out each component as we are building the feature. Working on design and new features in parallel and only building the components that would be used within the feature. This way, we weren’t devoting our resources only to the design system (which can be a hard pill to swallow for stakeholders) but to furthering the business goals.


Also, start small. Build only the components that you know you will absolutely need to be successful. Since a design system is never complete, you can always add more later.


**Dmitry:** Startups differ significantly from large organizations, and so do their design systems. The greatest risk for startups regarding platform investments lies in the "fog of war." Design systems can greatly aid product development, but they can also slow it down.


When starting from scratch, I recommend not asking the company to devote resources to developing the first version of the system for a year. Start small. Identify current design and development issues, request time to address these, and report the outcomes.


Ultimately, the focus should be on cost and process optimization to enhance the product quality. Even with a well-funded design system team, the objective shouldn't be to achieve 100% product coverage with system components or to implement components that can be built with equal quality in 30 minutes. The goal should be to tackle the complex aspects that teams spend days or weeks on, becoming experts in the design systems and accessibility domain for these challenges.


#### **Q: I’m currently facing difficulty when it comes to naming and categorizing components. For ex: Select options with dropdown, another dropdown with actions like three dot button with dropdown, etc. Could you share your approach?**


**Alissa:** For common components, I always try to go with common naming. There are a few ways to approach this.


The first is to make sure you are naming your components in step with what the component would be named on the development side. For example, simple components like buttons, radios, and checkboxes. Try not to get fancy with these. You ultimately want to make sure you are speaking the same language within design and development.


Second, take a look at other design systems. What are they calling these components? Can you find a common naming pattern between them? No need to reinvent the wheel.


Third, ask others opinions! There’s a great design system slack community ([https://www.design.systems/](https://www.design.systems/) ) with a channel called #topic-naming-is-hard. I think you will find you are not alone in the challenge of naming components. 😉


Lastly, name the component with the team and company in mind. What name makes the most sense in the context of how it will be used within the product? Ultimately, it just needs to be understood by the team.


**Dmitry:** Don’t overthink it. Many of us have heard a story about the component[called Fred](https://www.linkedin.com/feed/update/urn:li:activity:7009585660072181761/) .


If you're still driven to make things right and “scalable”, a great approach is to review other design systems' documentation. You'll find some unique names, but also notice many common ones. I personally follow a limited number of design systems to stay informed about how they approach similar problems and a good starting point for creating your own list could be the[Storybook Encyclopedia](https://storybook.js.org/showcase/) .


Ultimately, it's about gaining experience in identifying and structuring repeating patterns, and developing flexible APIs and names for the components.


#### **Q: How do you think the usage of a design system in a large organization can be tracked both in design and development?**


**Alissa:** For design, there are a few ways to track the usage of the design system. One way would be to look at the design systems analytics in Figma. Take a look at which components are being used and how often. Which components are not being used? You may decide to deprecate a component or wrap the functionality into a more commonly used component. Which components are being detached the most often? If a component is being detached, perhaps it isn’t suiting the team's needs, they do not know how to use it correctly, or maybe the functionality is broken.


Another is surveys and qualitative data! It’s great to send out a design system survey just to get a pulse on the system, how folks are using it, if they are finding it useful, and most importantly, why.


Lastly, keep track of requests and changes. This will give you a good idea of what the teams need and where the gaps might be in the system.


**Dmitry:** The most common starting point is tracking the flat number of components used in the product. For design, built-in Figma Analytics is useful, while for code, tools like \`react-scanner\` can help in assessing your React component library.


However, this number alone doesn't provide comprehensive information beyond general trends. It's essential to compare it with another metric. This could be the total number of components used in the product (including both custom and third-party library components), or a different metric, such as the number of support tickets received weekly.


Establishing this correlation helps better understand the system and its value. For instance, if the adoption number of flat components increases, is it because the product is growing, or are teams using it more? Are the components effectively helping teams? Comparing this with the total number of components and the volume of support requests can reveal if the change is due to an evolving adoption ratio. Even if the number of support tickets remains the same – you might be actually performing better than before due to more teams using the system now.


#### **Q: Do you have any tips if you need to build a multi-brand design system and one company color will be super light and the other company will have super dark colors? How to handle the components? Inverting the black to white in button for example.. change the lightness for system colors etc. Do you have some tips?**


**Alissa:** The best tip I can give when it comes to handling multiple brands or themes is to utilize tokens/variables (in Figma). It is great to have a base palette, but when it comes to light vs dark, it is great to utilize more contextual tokens. Tokens specific to the area of the component itself.


For example, this can be a tokens-related form field, such as a specific token for the background or the border. Another example is that maybe you have a nav bar that is light in one theme but dark in another.


Create a contextual token for the navbar background. Take the time to create and plan which contextual tokens you will really need that can be reused and where and ultimately minimize the amount of tokens overall.


**Dmitry:** The most transformative phase of our design system was the transition from "core" color tokens (100-900 color palette) to semantic color tokens. This approach means product teams use colors designated for specific roles, such as \`foreground-primary\` or \`background-neutral\`. This shift enables more informed decisions when creating themes, as it clarifies how colors are used and interact within the product.


Furthermore, this strategy allows for the generation of contrast pairs. For instance, with a \`background-primary\` color token, you can automatically create an \`on-background-primary\` color that resolves to black or white, depending on the contrast ratio.


#### **Q: How do you include QA or user testing in your component development processes? Can you share examples of when that feedback process has worked well?**


**Alissa:** As far as QA goes, you always want to factor in QA for every component. This is one area I would not skip as far as the design system goes. Everyone at the company will be using this design system, and you don’t want to introduce bugs into the product at the system level. To quote Josh Clark. “It’s a bad idea to inject crap into the bloodstream.”


As far as user testing goes, it’s not always feasible to test every component, especially at the atom level. At the atom level, it’s best to follow common UX guidelines for the component if you cannot test.


For larger organism components or patterns, I have personally found it helpful to do small rapid testing sessions for usability. These are smaller tests that could include testing only a portion of a component. This might include testing pagination on a table or accordion functionality on a card. This rapid testing allows you to iterate quickly and validate the pattern before introducing it to the system.


Another good way to get feedback on a component is to establish a BETA phase of the component, where teams can use and test the component before it gets added to the main system.


**Dmitry:** When it comes to Quality Assurance, the objective is to maximize automated testing. Relying heavily on manual testing is risky, particularly when the system is widely used across the product, and numerous reported edge cases have been addressed.


Regarding user testing, our standard approach to component requests is to collaborate on a custom implementation within the product first. We assess its effectiveness for the specific feature before integrating it into the system. This ensures that the additions to the system aren't just temporary solutions for the product.


This approach has led to instances where product teams tested new components that ended up not working well. They kept iterating and eventually decided to use a different combination of already existing components. At the same time, there have been cases where teams conducted their experiments within the product and then submitted requests with all their experimental data included.


#### **Q: What tips do you have for a 1-2 person design system team?**


**Alissa:** This is personally my favorite stage of a design system as it has a lot of autonomy. This is really a great time to set up processes together and take time to plan and build out the core structure of the system. This will include your foundations and base components and governance structures, so ultimately, when the team does grow, you are ready for it.


Some of the best advice I can give at this stage is to start small. Don’t overwhelm yourselves. Build the bare minimum. You can always build more later. As long as you have a strong foundation, you can scale and show progress.


When your core components are ready, pilot the system to only one or two squads to not overwhelm yourself with questions that may arise.


**Dmitry:** Your primary advantage is the ability to make swift decisions without waiting for approvals from large groups. However, your main challenge is limited time. Therefore:


- Be strict about your scope and prioritize based on product impact. Avoid attempting to build everything. It's completely acceptable to say no. Focusing on a few well-implemented components will protect you from constantly revisiting them in the future, which would leave no time for further development.
- Don’t spend too much time discussing how to make your system perfect, especially if you're a small team at the early stages of adoption. Your small size allows for quicker decision-making and iteration, even if it results in breaking changes.
- Begin adoption with a single product team, addressing their primary use cases and maintaining close collaboration. Only then consider expanding to other teams. This approach prevents the need to rebuild foundational elements once they're widely used, and it helps manage support requests effectively.


#### **Q: To have full control of the styling and interaction design, would you avoid at all cost off-the-shelf front-end component libraries? Specially if you need more customisation.**


**Alissa:** I think this depends on the component. There may be some components where you don’t need as much custom functionality such as your atoms. If you are trying to move fast or have a relatively small team, it’s not a bad thing to use a library to get started.


Know that you don’t have the use the library for every component. When you are ready to tackle a component with more functionality, you can add it to the system as custom.


**Dmitry:** If you're open to using a third-party library, it's worthwhile to consider this option. Currently, there are two main approaches to integrating third-party libraries effectively:


- Headless Libraries: These libraries come without any predefined styles but manage component interactions and accessibility. This allows you to maintain style consistency with your own design system while leveraging the functionality and accessibility features of the library.
- Starter Libraries: Choose a library that acts as a foundation for your design system, offering full control over its subsequent development and design. This approach saves time and resources during the initial setup and allows your team to start working almost immediately. It can also facilitate easier approval for funding, as the team can demonstrate quick progress and effective implementation.


A huge thank you to Alissa and Dmitry for answering the questions from our panel audience!


To learn more about how other teams are handling components, check out Supernova’s State of Components report:


\[


## State of Components 2023 Report


Get the latest insights on component trends and best practices.


Download Now\]([https://www.supernova.io/state-of-components#download](https://www.supernova.io/state-of-components#download) )
