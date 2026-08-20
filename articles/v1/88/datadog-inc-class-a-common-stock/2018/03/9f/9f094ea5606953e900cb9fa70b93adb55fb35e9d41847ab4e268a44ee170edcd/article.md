---
schema_version: "1.0.0"
document_id: "9f094ea5606953e900cb9fa70b93adb55fb35e9d41847ab4e268a44ee170edcd"
company_key: "datadog-inc-class-a-common-stock"
company: "Datadog Inc."
source_id: "datadog-inc-class-a-common-stock-rss-a5f59b9b4ce5"
canonical_url: "https://www.datadoghq.com/blog/engineering/what-product-designers-can-learn-from-explanatory-journalism/"
published_at: "2018-03-23T00:00:00+00:00"
first_seen_at: "2026-07-20T03:32:32.081856+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:fd36ac4d191939bcbd84e193cc20902e45c8e11285321faef528dec953c084e0"
---

# What product designers can learn from explanatory journalism

Steve Boak


*How to be a better designer by being a better explainer.*


---


I didn’t know explanatory journalism was a thing until I started listening to[The Ezra Klein Show](https://www.vox.com/ezra-klein-show-podcast) . He’s a founder of[Vox](https://www.voxmedia.com/pages/vox) , and I see a lot of parallels between the work of explanatory journalism and product design.


Explanatory journalism isn’t new—there’s been a Pulitzer Prize awarded for it since 1998—but explanatory news sites like Vox are built on it. Their value is not just in reporting the news but explaining it. Seeing only the most recent article on a big story will never give complete context—this is where explanatory journalism shines.


I think there’s a lot we can learn from explanatory journalism about how to be better product designers—by being better explainers.


## Favor the important over the new


> …the way we do news often privileges the new over the important[- Ezra Klein, NiemanLab, Jan 2017](http://www.niemanlab.org/2017/01/ezra-klein-hopes-vox-can-change-the-fact-that-people-who-are-more-into-the-news-read-the-news-more/)


We’ve all experienced some of the ways the internet and social media have transformed the news. Feeds that are constantly updating, pushing the latest information to us in an effort to command our attention, have fundamentally altered the way we consume and understand the news. Explanatory articles take the opposite approach, collecting context as it evolves to explain both what happened today and what happened that led to today.


In our work, every new support ticket, meeting summary, or customer call will impact our judgment. Without the ability to fit these pieces into the larger contextual puzzle, we too will fall back on a bias toward the new over the important. It may seem obvious that feedback from our biggest and most valuable customers should carry more weight, but when it’s scattered around it’s easy to lose track of. Bringing all of the feedback together, and identifying the sources clearly, will help weight it appropriately. Groups of people will also agree about things, so keeping a count of them helps identify our most common requests and prioritize them¹.


## Prevent context collapse


> We are better than ever at telling people what’s happening, but not nearly good enough at giving them the crucial contextual information necessary to understand what’s happened.[- Ezra Klein, The Verge, Jan 2014](https://www.theverge.com/2014/1/26/5348212/ezra-klein-vox-is-our-next)


The term *context collapse* was coined to describe the way “social media technologies collapse multiple audiences into single contexts”. As our messages spread to more and bigger audiences, they will be perceived differently by those who know us differently (be it personally or professionally) or don’t know us at all—and with social media we don’t tailor our messages to these specific audiences, so they all “collapse” into a single context. In our work, it’s easier than ever to share updates via Slack channels, hangouts, and collaborative document editing. I can post mockups of an idea and get feedback from my team in seconds, but for big projects the team might be lacking the relevant context to really understand the solution. As our companies get bigger, the case for building something new may include input from:


-


customer support tickets and conversations


-


meeting notes with customers from product managers


-


sales team requirements to close a deal


-


interview notes from design researchers


-


executive team goals


This context is assembled over time, from lots of people, and we learn more from each new piece of it. The first part of the job is simply assembling all of these pieces and putting them in one place. Preventing context collapse, though, is all about the follow-up.


Once the design work is ready to review, we need to be able to explain not only the solution being proposed but how it fits, or doesn’t fit, into the contexts each of these groups provided us (since some cases may be triaged out). It’s more than presenting the work—it’s being sure we can frame it properly to each audience.


## Space is not scarce


> What’s different today is that with digital publishing the norm, space is not scarce. With podcasting the time available to explain things expands. And with many more publishers feeding much more “stuff” into the system, the need for background and context is more obvious to more people[- Qifan Zhang, Feb 2016](http://projects.nyujournalism.org/newsliteracy2016/topics/explainer-journalism/)


I find that my own design process happens in two phases: expansion and contraction. The expansion is the collection of all the research, precedent, and data I have to work with—building a long papertrail² helps me understand the problem, articulate the requirements, and constrain the solution space. The more thorough and better organized I am in the expansion, the more effective and concise the contraction can be. Compressing all this research into short written summaries and ad-libbed conversations is so much easier and more effective when I know the research well.


I think half the reason this works is simply that these short summaries can link to the appropriate piece(s) of this massive papertrail for additional context. It may end up reading a bit like a David Foster Wallace novel, but it keeps the main themes together without a lot of sprawl. The other half is simply that the rigor and time it takes to organize everything means I have a strong understanding of the information, and I’m better able to summarize it.


Thankfully, this work all has immense benefit to me whether or not anyone else reads it, but it turns out they do (at least the pieces of it). Invest the time to create the papertrail. Keep it updated, refer to it often, and use it as canon for your design decisions.


### \[1\] Design vs Product Management


If some of this sounds like product management to you, consider a few things:


-


This work focuses on the usability of the product: workflows, interaction design and other nuts and bolts of functionality. This leaves PMs to focus on things like who’s going to pay for it, how much, and how long it will take to build when prioritized against the other things the engineering team needs to do.


-


The lines get a little blurry, but I’m making a distinction between product design, which I assume to include ownership of UX, and say visual design which likely would not. If you’re a product designer but your organization considers most of the above the domain of product managers, consider getting a new job—[Datadog is hiring product designers](https://grnh.se/fwo8mp1) .


### \[2\] Building the papertrail


Here’s how I typically organize my design research.


#### Interview notes


One document for each interview, with at least two sections:


1.


**context** (yeah, it’s like contextual inception) like the company size, person’s job, and team structure. This is super valuable when going back to the interview notes. I often don’t remember everything about the person/company I talked to, and in addition to weighting feedback based on how big a customer they are, the team’s shape will say a lot about why they want something. This becomes the basis for personas if we don’t already have them.


2.


**substance** —what they want and/or their feedback. I’ll often comb through the notes right after the interview to bold the good pull-quotes and categorize related points. It’s pretty important to do this right after the interview, since I’ll usually forget the entire conversation within a week or two, and if I am doing 5 interviews that day my memory will be a jumbled mess by the time the day is over.


#### UX priorities document


A document summarizing the design goals for the work. This will include:


-


a prioritized list of UX goals


-


sections capturing the most common and important themes from the interviews, as well as links to each interview, to support the priorities in the list


The completion of this document usually represents the first check-in point for a big project, where the exec team, PM, and engineering leads will buy in. In practice, this occupies about a third of our overall time in design (with the other thirds being interaction design and visual refinement).


-
-
-
