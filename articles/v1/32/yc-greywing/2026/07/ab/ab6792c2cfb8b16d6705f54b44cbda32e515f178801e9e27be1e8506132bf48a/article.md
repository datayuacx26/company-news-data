---
schema_version: "1.0.0"
document_id: "ab6792c2cfb8b16d6705f54b44cbda32e515f178801e9e27be1e8506132bf48a"
company_key: "yc-greywing"
company: "Greywing"
source_id: "yc-greywing-news-import-fac42ec76768"
canonical_url: "https://www.grey-wing.com/blog/the-origin-story-of-sea-gpt"
published_at: null
first_seen_at: "2026-07-21T22:08:18.820655+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:b9f87f612cb018affc16683d0617a89af2d9da605f31720274089177428821b5"
---

# Inception: The Origin Story of SeaGPT

# Inception: The Origin Story of SeaGPT


How we brought Generative AI to Maritime.


Nick Clarke


[@NickGreywing](https://twitter.com/NickGreywing)


Published on 2nd Jan 2024


7 minutes read


# **Discovering Generative AI**


It seems like a long time ago now, but in late 2022, some of our Greywing team members began using AI Tools. They were writing code using AI, and talking about the developer tools that were beginning to incorporate AI. Things were *starting* to change.


At the outset, the AI solutions were okay, not great. It was on my radar but I never thought much of it. Then in February 2023, my Co-Founder reached out to say that he had been having a look at some of the OpenAI tools and that some of them were pretty compelling. This was still in the days of Chat GPT 3.5 breaking the internet. As a Y-Combinator company we had privileged access to OpenAI resources so we took advantage of that. Within weeks my Co-Founder came back to me and said to me, ”Look, we should do something with this, its powerful and this is my background. I really think that[Maritime needs an AI focused tool”.](https://www.grey-wing.com/blog/software-still-isnt-eating-maritime)


With so much work happening around our Crew Change module, the big question was, “What do we use it for?” We probably decided on a much harder problem to solve then we should have, but to his credit, Hrishi took it in his stride and cracked on.


### Recommended Watch


[Finding last minute crew replacements with SeaGPT.](https://www.linkedin.com/feed/update/urn:li:activity:7148548260666687488)


The first problem we decided to solve was automation of[Port Agency communication](https://www.grey-wing.com/blog/greywing-s-new-ai-for-maritime-sea-gpt-untangles-crew-change-hurdles-with-a-simple-conversation) . The basic premise was that Port Agency Communication takes up valuable time, the data comes back in so many random formats that it's hard to make an apples to apples comparison between different offers.


Could we:


- automate the exchange with the Port Agent,
- and then take the responses and turn them into structured data for analysis?


It turned out that we could. Less than 2 months later, in April, we released SeaGPT, a tool to automate the job of reaching out to and having a conversation with multiple port agents to get all the data a crew manager needed to plan a crew change.


### Recommended Reading


[Shipping AI chatbot looks to make light work of crew changes](https://www.tradewindsnews.com/technology/shipping-ai-chatbot-looks-to-make-light-work-of-crew-changes/2-1-1440010?zephr_sso_ott=Ke1Ytd)


We had a great response and within days over 100 users had signed up, but what we struggled with was that people did not just use SeaGPT to reach out to agents, they asked it everything...


> ` Search for port restrictions in china`


> ` Is it allowed for a Greek nationality superintendent to attend to a vessel in Canada?`


> ` Can Ukrainian crew to be repatriated to Russia?`


We had all sort of enquiries coming into Greywing, many of which fell out side of the core use cases we had initially trained it for. They gave us a great compass of what to focus on in future.


What blew our mind was the speed with which people signed up and how easy it was for them to use SeaGPT. It was unlike anything we had ever seen.


It changed the way that we thought about data. The status quo was that[maritime data sat behind our customers software](https://www.grey-wing.com/blog/dollars-time-and-strategy-why-maritime-needs-integrated-systems) , websites and other data sources that they used, liked spreadsheets. It was apparent that if Users could connect more directly with their data through voice or written word they will take that opportunity.


People are smart, and they look for the short cuts. By opening up their data we enabled them to create their own path.


It was a huge lesson; if we trained SeaGPT to handle the more demanding questions users had asked, we would have an exciting project on our hands.


# **Limitations**


Perhaps the biggest limitation that SeaGPT was released with, was that it focused too much on executing a single work flow.


We had a 95% delivery on what we had designed SeaGPT for, but in terms of what our Users were asking, it was a 50% outcome on whether the response would be good or accurate. In the early days, Users may have became disillusioned with SeaGPT, because it was not able to handle 100% of use cases.


If we were to repeat the process, I would make sure that we more clearly set expectations about what SeaGPT could not do, so we could channel people towards focusing on what it could.


This has also informed how we think about building the product. Focusing on SeaGPT being an assistant, for a more limited set of high value tasks, instead of a maritime version of Google or Chat GPT. By giving it high value tasks, we clearly create a niche where SeaGPT can be unique in what it can deliver. But we'll get into the[details of what that means](https://www.grey-wing.com/blog/present-day-gen-ai-in-crewing) in another post.


### Recommended Reading


[Building Generative AI in Crewing: Present Day](https://www.grey-wing.com/blog/albert-on-greywing)


Having spoken about its limitations; it also presents unrivalled opportunities. In the same vein of thought, whilst Generative AI cannot solve all problems, it can solve some problems that were previously intransigent. I think this is why we are seeing a boost in LLM and Generative AI technology. Its an additional tool that provides the missing piece of the puzzle for many solutions that were just waiting for that final element to enable them to solve a problem.


Within Greywing and its team meetings, Generative AI is almost like a new team member. The team practically delegate work to SeaGPT that might be harder or more cumbersome for them to do.


### Recommended Reading


[Greywing's One-Week Sprints Are Charting a New Course in Maritime Tech](https://www.grey-wing.com/blog/albert-on-greywing)


# **Iterations**


It was only natural that once we saw the initial demand for SeaGPT, we would follow up with new features. One of the values that Generative AI delivers that is unparalleled is the ability to[query any type of data using natural language](https://www.linkedin.com/feed/update/urn:li:activity:7140144879132672000) What this means, is you can talk to your data. Whether by voice or typing you can ask you data questions and retrieve data with a few short words.


This is a game-changer. The first place it made sense to plug it in was in flights. We connected SeaGPT to Amadeus so that customers[could query their travel agents for available flights](https://www.linkedin.com/feed/update/urn:li:activity:7105194430243995649) (including LCC, Marine and Ordinary content) just by asking for them. It is now one of the top three features customers use in Greywing, whether they are asking for a Seaman’s fare from Manilla to Amsterdam, or simply asking for the cost of a specific Flight.


We upgraded to Automated Port Agency Outreach to break down data to easily compare costs between different port agents.


Finally, we automated the process of keeping up-to-date with port agency pricing. For a number of customers, we reach out on a monthly basis to their preferred Port Agents for pricing, port state restrictions and any Visa requirements so that data is always a click away.


Why do we do this? When you are operating 50+ vessels with a limited headcount team, there is huge duplication of effort as team member reach out to the same port agents time and time again for similar requests. Co-locating all of that information in a single easy-to-use dashboard makes life so much easier for crewing team members. After-all, that is what AI is all about, artificially automating the tasks that they don’t need to repeat.


### Recommended Watch


[Set up Crew Changes with a Port Agent.](https://www.linkedin.com/feed/update/urn:li:ugcPost:7056527932147576832/)


# **Responses from Users**


The response from Users has been fantastic. Having the ability to automate communication that they previously had to do themselves, is a no brainer. Users help us identify inconsistencies in how we price crew changes and guide us towards what the right outcome should look like.


Overall, between our own efforts to introduce AI into the maritime space and the guidance of the Teams we work with, we have found an approach to using AI that make things easier for everyone in Maritime. The technology works well, we build a group of happy users, and happy stakeholders, and it gives us the budget to build the maritime technology we love.


Want to stay updated on all things AI we're building for Maritime? Sign up to our monthly newsletter[here](https://www.grey-wing.com/contact-us) .


## Make better decisions with Greywing


Talk to us about your fleet, or follow what we're building.


## Read more from **Proteus**


[Back to blog](https://www.grey-wing.com/blog)[10 minute read Proteus Superficially adopting AI or genuinely leveraging it? How to think about your AI Strategy in 2024. Nick Clarke @NickGreywing 12th Apr 2024](https://www.grey-wing.com/blog/superficially-adopting-ai-or-genuinely-leveraging-it)


[3 minute read Proteus Proteus Vs Ocean Oracle… which is better? Can I pit our two newest products against each other? Nick Clarke @NickGreywing 19th Feb 2024](https://www.grey-wing.com/blog/proteus-vs-ocean-oracle-which-is-better)


[5 minute read Proteus How we *had* to build OceanOracle How we were forced to become the first company to build a visual AI for documents Hrishi Olickel 1st Feb 2024](https://www.grey-wing.com/blog/how-we-had-to-build-ocean-oracle)


[5 minute read Proteus Inception: The origin story of Ocean Oracle Unlike in ancient history, we're giving you real data. Nick Clarke @NickGreywing 30th Jan 2024](https://www.grey-wing.com/blog/the-origin-story-of-ocean-oracle)
