---
schema_version: "1.0.0"
document_id: "c5c1837edf691004ebfc8b0958bdb66b58d7c7fc0e0998f725d8c1680234675c"
company_key: "yc-supernova"
company: "Supernova"
source_id: "yc-supernova-rss-864f3bee1480"
canonical_url: "https://www.supernova.io/blog/beyond-launch-nurturing-sustaining-design-systems-q-a-follow-up"
published_at: "2023-09-18T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:40.084347+00:00"
fetched_at: "2026-07-28T21:01:43.013341+00:00"
content_hash: "sha256:be6b54c8c3d52aabebeabb3fb07562d2667e1fd6908f6c682fa978a2a256b0b9"
---

# Beyond Launch: Nurturing & Sustaining Design Systems — Q&A Follow-Up

Building and launching your design system is just the first part of the journey. A lot goes into nurturing and sustaining it to ensure your design system is usable and up-to-date with your company’s needs.


We hosted an expert panel, “Beyond Launch: Nurturing & Sustaining Design Systems” with industry leaders[Bethany Sonefeld](http://bethanysonefeld.com/) (Design Manager for Duo Authentication at Cisco Secure),[Henry Daggett](https://twitter.com/henry_daggett) (Design Systems Lead & Product Designer at Societe Generale), and[Jeremy Dizon](https://twitter.com/jeremydizon) (Lead Product Designer, Design Systems at Disney Streaming) to dive into what maintaining your design system looks like and how to do it effectively.


If you missed the live panel, you can still[watch the recording](https://www.supernova.io/nurturing-and-sustaining-design-systems) or[read the takeaways](https://www.supernova.io/blog/design-system-documentation-is-a-moving-target-panel-recap?utm_source=blog&utm_medium=link&utm_campaign=ds-docs&utm_content=q&a_blog) from the discussion. We got a ton of audience questions that we weren’t able to answer during the talk, so our panelists have graciously shared their insights below.


Let’s dive in!


#### **Q: Let’s say someone is starting out with building their design system today. What advice would you give them to make maintaining that system easier in the future and as the system grows and evolves?**


**Bethany:** Try to build the system in a way that it’s flexible and dynamic in nature. At IBM on the Carbon team, we ended up having to completely scrap a few components because we didn’t build them with scale in mind. Building in that flexibility early will also allow for more teams to adopt, and empower designers to still express their creativity. There are a million ways to style a component, so how can you build in variants that allow for consistency but also uniqueness?


**Henry:** To start with I’d recommend not getting too attached to anything, and approach early work with an aim to provide value and to make things easy for adopters — even if it makes things a little harder for you as a maintainer. Next, I wouldn’t waste too much time “artifacting” — or making components absolutely perfect and perhaps over-engineered. Then, **document everything** . My biggest regret is having missing documentation for so many things!


‍ **Jeremy:** Something that the Disney Streaming systems team is dealing with right now is setting up a consistent way to build each component, whether to utilize Figma’s Variables feature, Variants, or Component Properties. Having a clear understanding of how the system components are built will enable your system to scale accordingly and be as flexible as it can be for your users and partners.


#### ‍ **Q: Is there a limit or threshold of “how many hands are in” are touching a design system? Re: Jeremy “the DS is more flexible when everyone has their hands on it?”**


**Jeremy:** I’m a firm believer that “more the merrier” is better but yeah, too many cooks in the kitchen is not always a good thing. At the end of the day, the systems team will keep everyone within the established system structure and make designs to ensure those system patterns are followed. But in terms of evolving a system, I think everyone has the responsibility to do that, not just the systems team.


**Henry:** With clear governance, more people can be added to the process, so long as someone has the final say and it’s respected. At the end of the day, Design Systems are building things for use in products, so it’s the end user who should really be at the top of all design decisions. For simplicity, though I’d recommend mainly just getting people to work on topics that are in their area of expertise, for example, if you have a data visualization expert on your team, they should probably be the ones guiding your data vis…


#### **Q: When setting up those channels for feedback or requests, what were the best ways?**


**Bethany:** Having a point person (almost like Engineers assign someone to PagerDuty), was really helpful for the Carbon team. It can feel overwhelming to see requests pile up, whether it’s in GitHub, JIRA, or Slack. Have a plan for answering or moving these forward in some way — it will help contributors feel heard and valued.


**Henry:** Single points of input are useful, we have just one repo where people can open tickets for all sorts of requests/issues. For communication it’s probably a case of using literally whatever means are possible in your company, Slack or Teams channels, emails, mailing lists, good release notes, going to town halls — you can get really creative with your communications. Also, I do believe that effort in communications pays off, if your material is engaging, then more people are likely to read it and remember it.


**Jeremy:** At Lyft, we set up Slack aliases to easily ping Systems Designers, Systems Engineers, or Systems Platforms reps that include both Systems Designers and Systems Engineers to help focus the support they need. This worked really well for questions or bug requests. When the Systems Team verified it was indeed a bug, we would ask the partner to submit a JIRA ticket to help track that bug request. So, we used both Slack and JIRA in parallel to create an efficient and trackable feedback loop.


#### **Q: What are your thoughts on how AI can contribute to design system documentation/maintenance/processes, and have you incorporated it in any ways?**


**Henry:** Can it just do it all for us, please? There’s obviously a big use case that is documentation, and there are a lot of possibilities there like quick documentation discovery and help with writing documentation. We haven’t integrated it at all (although I suspect one or two of our documentation pages have had some chatGPT involvement).


For the future, I’m interested in the possibilities of how to make content that is best readable by AI, and then letting users define how to read their doc. They could have everything in bullet points, long prose, or a poem… you get the idea.


**Jeremy:** Touché on the interesting question! I haven’t really dug into the AI realm but I think we could really leverage it to help offload the mundane (and oftentimes, busy-work) tasks to free the systems team up to support partners in different ways. Off the top of my head, I think of an extension of a Slack bot to provide answers that pull from the documentation resources. I still think a systems designer needs to write the documentation because I’m a purist but maybe one day AI can take this task on for us.


#### **Q: Which metrics would you advise to adopt to justify or explain to top management why they should keep investment around maintenance?**


**Henry:** Even though I do work in a bank we actually aren’t too big on metrics — you have to remember that anyone with financial/business training knows that statistics can easily be selected to say whatever you want. That said if you have a clear adoption target of X projects using the DS out of a total that’s a nice number to have, and then also things like how long it takes to integrate changes are important to know, and maybe also volume/time to resolution of issues and tickets etc.


‍ **Jeremy:** I’ve been thinking about this a lot and the “Component Detachment rates” is the first metric I would really dig into to understand what is really going on with a component and why designers are finding it unusable. On the flip side, it’s hard to measure the new Figma features that get released and make the case to Leadership why maintenance is important. However, I think keeping the system up-to-date with these new features improves both the efficiency and usability of the components.


#### **Q: How do you work within a team with no PM during maintenance mode? How were you able to communicate your ideas, the roadmap, and the overall strategy?**


**Henry:** Yep! We just do it ourselves and communicate via all the channels that we have! As a System lead, you’ll find yourself focusing on communications more than you thought so it’s a good skill to train.


**Jeremy:** I see this as a win-win because at Lyft this was our reality and we got to decide our own roadmap and projects we wanted to tackle. Needless to say, we tried to sync up our plans with our engineering partners and sent out periodic surveys to get an idea of system projects that were important and valuable to our design partners. But we would flex our communication writing skills and discover the best cadence to send out those comms, etc.


***Pro-Tip:*** *Write your comms with your engineering partners so the content will be on the same page.*


#### **Q: How do you educate your users (developers and designers)? Do you knowledge transfer? How do you communicate with your consumer teams? Is it through an announcement in Slack or an e-mail newsletter to know what is new in DS, or do you implement office hours like weekly or bi-weekly? Any advice?**


**Bethany:** I think the education piece can come in with having thorough, clear, and beautifully laid out documentation. In terms of announcements and comms, I always err on the side of over-communicating… Slack/email/what’s new pages on your system site. You have to be your team’s PR agent. For office hours, try out a few different options, but keep it consistent.


**Henry:** We have a subscription list with our release notes that we put a lot of effort into, and we try and communicate big changes via all channels that we can, even with presentations to some teams. We also rely on teams and people close to us to spread the message. We actually don’t offer specific office hours, but our DS maintainers often go to design reviews and other things, plus we’re always available to chat. We avoided office hours just because in a large company like ours things can get a little out of hand!


**Jeremy:** There are a lot of different ways to educate system partners so it really depends on the appetite of your org. I personally enjoy workshops and skillshares but if no one is going to them, then you’ll need to try something else. Having a dashboard on your documentation site or area where updates can be posted is my preference. This content can also be sent in Slack to the appropriate channels as a snippet or announcement with a “Learn More” link to the documentation site 😉


On the office hour front, we have two time blocks a week that we recommend design and engineering partners sign up for so we can ensure the right systems folks are in the room. If no one signs up, then the systems team can catch up on work or take the opportunity to review things we’ve been exploring.


#### **Q: Part of sustaining a design system is getting people on board who may think it stifles creativity or are not currently interested in using one. How do you go about convincing those skeptics?**


**Bethany:** I hear this a lot, and it is totally valid. I would encourage the design system team to build the system in a way that IS flexible in nature. An example is building data tables — you can build a variety of variants that include styling preferences like zebra stripes, borders, background color, etc. I also love hearing from designers on ways to make the system more flexible so that they don’t feel constrained creatively.


**Henry:** Our DS just passed its 9th birthday so it’s been a little while since we had to deal with things like that as the system is pretty well integrated into our platform (if you want to be on the platform, you need to use the DS and platform building blocks).


To answer the creativity argument though, I think some of the best creativity comes when working within constraints so it’s just about angling creativity away from things like border-radius and more towards solving actual user needs.


Also having a DS just saves literally so much time — take date pickers for example, I think ours took something like 3-4 months to fully design and build so there’s no way you want to do that every time!


**Jeremy:** I experienced this firsthand at Airbnb when we launched DLS. I worked with those folks in 1-on-1 sessions to really understand their apprehension and pushback. A lot of times they just didn’t know where to start, so we’d co-design together and I’d show them how to pull the system components into their mockups, etc. That’s how I turned detractors into promoters but it wasn’t the most efficient. We later incorporated a section within new designer onboarding to help new hires feel more comfortable using the system.


#### **Q: Have your design systems helped improve accessibility at your organizations? How do you track that? Any tips to share on how you review and test accessibility of components and patterns?**


**Bethany:** Absolutely, yes! Teams get a11y “for free” when they adopt components that have already been thoroughly tested and vetted. If you have any tooling at your org or an accessibility specialist to audit products, that’s a great way to track the impact.


**Henry:** I think it has. Our System carries the disclaimer that “nothing in the system is inaccessible, but the way you use it can be”. We do push for accessibility internally, too, which is helped because as we’re part of a French company, there are very stringent government guidelines that we have to adhere to.


**Jeremy:** As systems designers, accessibility is something we think about A LOT, and building it into the components, behaviors, and patterns makes it easy to argue about the value of using the system. It also is a good educational topic to discuss with feature designers to help them think outside of just their feature and specific user base. I’ve been lucky enough that Lyft engineering instigated accessibility reviews and have tests in place so as systems designers, we incorporated an accessibility toolkit to help keep this discussion at the forefront when we collaborate.


*Want to learn more about how to scale accessibility through design systems?*


*with Geri Reid, Lauren Beatty, and Stephanie Walter. Then, learn*[how Geri and her team at NewsKit created accessible components](https://www.supernova.io/blog/how-to-create-accessible-design-system-components) *for their design system.*


#### **Q: How do you serve designers who only design for a single brand vs designing for multiple brands? It seems like we have an exponential amount of guidance to provide.**


**Henry:** We have one main brand, but a few other brands do use our system. Our DS is very unbranded by design - we don’t really use branded color and everything is just designed for clarity, so others are able to use it with a logo switch and one or two small color updates.


**Jeremy:** I’m still trying to master this! Coming from Airbnb and Lyft which only focused on one brand, Disney Streaming encompasses multiple brands. The guidance doesn’t change but what we focus on more is how to ensure the brand is still shining through, whether it’s through colors and other UI styling. And I usually ask my partners what they need support on to help focus what guidance I give.


#### **Q: Talking about learning systems and system thinking, how would you go about helping your co-workers to get better at it?**


**Henry:** I think the easiest way is definitely by doing. We often work with collaborators and so they can start to see how things work and how to break down components into parts and also begin thinking more deeply about states and interactions.


**Jeremy:** Usually I ask pointed questions on how they can adapt a component they’ve built for another feature or another platform. I usually guide them down those rabbit holes and allow them to answer me with their best guesses. It’s a continuous conversation to have, whether it’s 1-on-1 or during office hours chats.


#### **Q: Bethany, how do y’all at Cisco and previously IBM reconcile “consumers” vs “enterprise” design systems? Is there one core system or are they separate based on user needs?**


**Bethany:** At IBM, we had a larger Design Language with foundational elements like color, typography, grids, icons, etc. The org was so big that Product teams then interpreted those core styles and created a “branch” of the IBM Design Language.


At Cisco, within the Security Business Group, we are working towards merging a few design systems into one, primarily due to the push for consistency across many different business units. It’s taking time, but will be worth the effort in the end!


#### **Q: How does your organization support the council? Those people have their own products and agendas, how do you make sure that they give you time — is this mandatory attendance for them?**


**Henry:** One of our council requirements is that members must have at least 50% of their week dedicated to DS work. So we ensure they have time before they join. The DS work will then also be in their objectives etc. so they can be compensated for that work etc


#### **Q: Apart from the design system you work with, what are some of your favorite design systems?**


**Jeremy:** I usually get inspiration from Material Design (Google), Polaris (Shopify), and, of course, the Human Interface Guidelines (Apple). Plus I’m biased and adore the Lyft Product Language (though it’s not public for anyone to reference).


**Henry:** For me, I’ll go for Carbon, Adobe, NewsKit (RIP), Atlassian, Apple. I also get a lot of inspiration from libraries like Shadcn, radix, tailwindUi, and others.


**Bethany:** Material Design is truly the godfather of Design Systems – they really set the standard back in 2015/2016 for how to build a system at scale. I’m obviously biased here, but the Carbon Design System is a great example of how a small team (8 folks) can build something that’s scaled across a large org like IBM.


---


Want to speak on our next panel?[Fill out this form to become a speaker](https://bit.ly/3qRWGNX) at a Supernova event. We’re always looking for new, diverse voices to uplift and highlight in the design systems community.
