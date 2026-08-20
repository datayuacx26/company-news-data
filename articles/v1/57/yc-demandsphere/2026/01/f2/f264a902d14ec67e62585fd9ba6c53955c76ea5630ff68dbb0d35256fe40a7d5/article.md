---
schema_version: "1.0.0"
document_id: "f264a902d14ec67e62585fd9ba6c53955c76ea5630ff68dbb0d35256fe40a7d5"
company_key: "yc-demandsphere"
company: "DemandSphere"
source_id: "yc-demandsphere-rss-9fd33107a059"
canonical_url: "https://www.demandsphere.com/blog/2025-in-review-at-demandsphere/"
published_at: "2026-01-01T00:00:00+00:00"
first_seen_at: "2026-07-25T01:15:10.716199+00:00"
fetched_at: "2026-07-28T22:24:15.603719+00:00"
content_hash: "sha256:2e8a0da0bd2218f82feeb81be89b7ffd5735b68a346b51ce21656e1a7f1e1e4a"
---

# 2025 in Review at DemandSphere

## What should you know about 2025 in review at demandsphere?


## Introduction


This is the first post in a three-part end-of-year series covering what we learned in 2025 and what we’re focusing on in 2026.


- **Part 1 (this post):** Our work in the AI search community, and the major shifts that affected our customers and audience.
- **Part 2:** New product features and innovation from the DemandSphere team in 2025
- **Part 3:** What we’re seeing in 2026, and what we’re focused on next.


## TL;DR: 6 takeaways from 2025


- **Search data is behavioral data** : a way to measure human attention
- **Context windows are finite** , which means that retrieval and indexing will remain central
- **The index is the prize** : the source layer matters more than the UI layer
- **All search is AI search** : semantic retrieval has been here for years
- **AI Mode, AI Overviews, and query fanouts** are the new keys to understanding AI search visibility
- **DemandSphere’s focus stays the same:** provide the best data across all search experiences to make it actionable


## Big Trends and Observations


2025 was a turning point year in search and AI. I won’t be able to cover everything that happened, even in our industry, as this could easily fill a book.


A few themes do stand out, however.


## Search data is data about human behavior and attention


The most important thing to understand in search for 2025, in our view, is how the various AI search experiences work. Too much time and energy was wasted on silly debates between “SEO is dead” vs. “It’s all just SEO” when really we’ve been living in the world of AI search for 10+ years.


Thanks to Google.


The real question is, where is human attention focused at any given moment and what are the user experiences that mediate the human attention to the digital world.


At the end of the day, this is all that really matters.


What this means is that data about this mediated experience is behavioral data. This is an underappreciated fact. In our industry, we tend to view search data, such as data from SERPs and LLMs as machine data. And it is. But it is also behavioral data.


This shouldn’t even be controversial but it is, or was, for some weird reason that is likely due to the idiosyncrasies and mixed incentives to be found in the SEO space.


Nobody wanted to believe that Google was using user behavior to influence how SERPs work. Rand Fishkin talked about it and he was treated poorly for this. Many others did as well.


It always just seemed blindingly obvious that of course Google is using behavioral data to influence their products.


Why would they not?


They have Chrome.


They have Google Analytics.


They have Android.


They have all of these ways to monitor and measure user interactions.


Of course they will use it.


Thanks, in part, to antitrust scrutiny this has become broadly understood.


Now it’s becoming clearer what this means. It means that this data about human behavior and attention and how they impact the search engine results is immensely valuable.


## The context window is finite (and likely always will be)


Google’s search index is also something else: it is the most comprehensive, up to date, technologically advanced real-time index of human knowledge that has ever been built before.


We’ll put aside for the moment that it was built off the efforts of every company and individual that has contributed content to the internet.


It is very easy to underestimate how hard it is to build any kind of search engine, let alone the best and most comprehensive one in the world.


This brings us to OpenAI and the other LLM companies.


We probably all know the difference between foundational models and live retrieval. The core issue with foundational models is that the world moves too fast at too large of a scale for the models to ever keep up.


The context window is not infinite.


We define infinite as “the entire global internet, up to date, in real-time, at every millisecond.”


Global economic forces are aligned against the very possibility of an infinite context window. Unless something changes on a fundamental technology level, this will always be the case. Because of this finite context window and the limitations of foundational models, we have live retrieval. The problem with live retrieval is you need an index or indexes from which to retrieve.


This is where things get dicey for companies that are not Google.


Who has the best index in the world? Google. Who has the second best? Microsoft Bing.


Which search engine would you use if you had to choose between Google and Bing?


It doesn’t matter how much money you’ve raised and how many of the best search engineers, even ex-Googlers, you hire.


It will still take years to build something even remotely usable on the scale that Google has.


Just look at Bing. It was launched in 2009. And it’s a great product but are you using it daily?


Probably not.


## The index is the prize


This means that OpenAI has to use an index. They started with Bing and, until quite recently, everybody insisted that they were only using Bing and not Google.


This seemed ridiculous to me so I decided to do some research.


[We did a great study on AI Overviews](https://www.demandsphere.com/2024-aio-report/) with Botify at the end of 2024.


It’s still worth reading because there is still so much foundational knowledge in there and it is a good snapshot of how things worked just a year ago.


That project inspired me to start doing some additional research because during our work on the AI Overviews project, even then, it became clear that it is really all about the citation URLs.


And this means **it is really all about the index** .


We have a data pipeline product called SERP Intelligence (it will be renamed to Search Intelligence in 2026).


SERP Intelligence is a BigQuery-based data warehouse that is fully managed at an operational level, so once you subscribe, all you need to do is start running queries to ask questions about any of the SERPs you are tracking. You don’t need APIs, developers, caching layers for your BI tools, etc. It’s a real-time interface to the most valuable behavioral dataset on the planet.


Anyway, I decided to start looking into the question of whether ChatGPT was really using Bing for all of its citation URLs, because I was skeptical. This was in February.


If you have Google, why would you use Bing?


I[presented my findings at brightonSEO UK in April of 2025](https://speakerdeck.com/raygrieselhuber/exploring-the-relationship-between-traditional-serps-and-gen-ai-search) . As far as I know, I was the first person to talk about this.


The LLM-tracking community didn’t really want to acknowledge this and, at best, they would try to sort of acknowledge it by saying that “ChatGPT isn’t using Bing, they are using SerpApi.”


This is a disingenuous obfuscation. The main reason, in my opinion, they didn’t want to acknowledge this is that it destroys the narrative of “SEO is dead.”


If everything comes back to Google’s index, then clearly SEO is not dead.


Long story short, ChatGPT is and has been using Google’s index for a long time. It’s not even hard to discover. It has become a little more cloudy with query fanouts but this is mostly a cost issue and can easily be surmounted in research use cases.


I believe that this was the reason behind the[January SERPocalypse](https://www.demandsphere.com/blog/observations-on-the-january-serppocalypse/) and I would also bet that it was the reason behind the[September pagination update](https://www.demandsphere.com/blog/google-tests-forced-pagination-on-serps/) .


So, one major trend topic to continue to pay attention to is what I have been saying all year: **the index is the prize** .


## All search is AI search


OpenAI is in for some pain relative to Google.


I’ll talk more about Google’s empire of AI in one of my next posts.


The key point though is that all search is AI search. There is no such thing, anymore, as search that is not impacted or built on top of AI search infrastructure.


[As Mike King continues to hammer home](https://ipullrank.com/how-ai-mode-works) , we have not been dealing with lexical search technology for more than ten years.


We live in the world of:


- semantic search
- embeddings
- vectorization and vector analytics
- entities
- knowledge graphs


And more.


The question is not “is this AI search or not” the question is, what is the user experience that is drawing the most attention.


Currently, today, that is still SERPs, by a long shot, at least in terms of traffic numbers.


But you also can’t ignore the growth of generative AI user experiences such as ChatGPT, AI Mode, AI Overviews, Gemini, and others.


And you definitely can’t ignore the rise of agentic usage via APIs, MCP, and enterprise integrations.


Since it is all about human attention, it can sometimes be difficult to account for how human attention is measured in agentic systems. I’ll be writing more about this soon too with a simple example looking at X.


But, human attention is indeed there, it’s just that it is externalized to the systems into which the AI behavior is integrated.


So now we’re dealing with the externalities of human attention across trillions of dollars worth of cloud infrastructure and SaaS applications.


# Industry Changes


As I mentioned before, there were far too many changes to document all at one time here, so I’m going to focus on the big ones that most impacted our customer base.


## January SERPocalypse


[We wrote about the January SERPocalypse](https://www.demandsphere.com/blog/observations-on-the-january-serppocalypse/) (as we have called it).


This was the first indication that Google was taking steps to make it more difficult for companies like OpenAI to index their SERPs.


The net effect was that JavaScript ended up being required far more than before in order to get accurate SERP data.


## AI Mode Launch


[AI Mode was announced in May 2025 in Google Labs only](https://blog.google/products/search/google-search-ai-mode-update/) . This means that you had to be logged in and have Google Labs enabled in order to see it. We decided to wait for the inevitable public launch, at least for the US, in order to build indexing capabilities to track it.


We were at SMX Advanced Boston on June 12th when it launched. In less than 24 hours, we had tracking capability for it and, in fact,[we were the first company to support it.](https://www.demandsphere.com/blog/were-the-first-platform-to-track-google-ai-mode/)


## Query Fanouts


With the launch of AI Mode, another new topic began to be discussed in earnest:[query fanouts](https://blog.google/products/search/google-search-ai-mode-update/) .


As probably everyone knows by now, query fanouts are the synthetic, additional, background queries that LLM applications will use in order to gather additional information from the SERPs and other sources in order to more competently answer user queries.


As one can imagine, being able to capture and understand this fanout data is of critical importance, which is why we decided to make this available as a feature in the DemandSphere platform, in our Suggestions Explorer.


Again,[we were the first platform company in the industry](https://www.demandsphere.com/blog/new-features-ai-search-visibility-monitoring-covering-chatgpt-perplexity-ai-overviews-and-more/) to support this level of intelligence.


## AI Overviews (AIOs) are here to stay


To the chagrin of many, AI Overviews are here to stay. This may sound a little surprising to people now, at the beginning of 2026, but there was still a lot of discussion about this at the beginning of 2025. Yes, there are still a lot of inaccuracies and other problems but they aren’t going anywhere.


We have some of the best AIO tracking in the industry, with a complete Share of Voice (SoV) model, as well as integration with the rest of the SERP in question.


Both AI Mode and AI Overviews are very good reasons why it is critical to have integrated SERP and LLM tracking in the same data pipeline.


## ChatGPT ecommerce


OpenAI has been making some of the earliest strides in ecommerce integration into their LLM experience. Most significantly,[they announced an integration with Shopify](https://www.shopify.com/news/shopify-open-ai-commerce?term=chatgpt%20ecommerce&adid=776409441547&campaignid=15436644376&utm_medium=cpc&utm_source=google&gad_source=1&gad_campaignid=15436644376&gbraid=0AAAAADp4t0o2S5EKzimVuUi4XNwyBzVnE) which, from a product perspective of both companies in question, makes all the sense in the world.


The narrative, as it goes, is that in a few years websites won’t exist anymore and everything will just be an MCP (or some variant) server for the LLMs to interact with.


On the one hand, I understand the narrative.


On the other hand, I’m skeptical that it will be so comprehensive of a change. At least in the short to mid term.


This narrative is simply too focused on a) ChatGPT and b) B2C commerce, in my view.


For the first point, owning B2C ecommerce necessitates that a company like OpenAI can come to be default user experience and gain effective ownership over the credit card. From our current vantage point, I don’t see that happening.


I do see Google being able to do it though.


Which brings us to the second point.


People forget that B2C commerce is essentially a downstream of B2B ecommerce. And B2B ecommerce has lived outside of the traditional web experience for decades.


I remember implementing EDI-enabled ERP-based commerce systems way back in 2001. And I was the new guy on the block, people had been doing this for years already.


A lot of the agentic workflows people imagine on the B2C side have already existed as heuristics and rule-based decision systems within these ERP behemoths for 20+ years.


And that’s just ecommerce. We haven’t even touched all of the other industries.


Long story short, I think the web is here to stay for quite some time.


That doesn’t mean I am bearish on agents and agentic commerce.


Quite the opposite.


I just think that new use cases and usage patterns will emerge and it will be an addition vs. a replacement.


Also, in the same way that the **index is the prize** in AI search,** the feed is the prize** in automated commerce and shopping.


Guess who owns the feeds today?


Hint: it’s not OpenAI.


## Google pagination update


This is the one that threw everybody for a loop.


[We wrote about it here, again, before anybody else](https://www.demandsphere.com/blog/google-tests-forced-pagination-on-serps/) .


Google made a simple change that disrupted the entire SERP tracking industry.


They removed an HTTP parameter that allowed users to get 100 organic results on one request and this change instantly made it more complex and expensive to get datasets that resembled what people were used to seeing.


Fortunately, through a lot of hard work, research, and cooperation with proxy partners,[we were able to come up with a solution](https://www.demandsphere.com/blog/google-num-100-pagination-update-from-demandsphere/) where we now default to providing the top 5 pages by default and, if clients need it, the top 10, at no extra charge.


Things can always change, of course, in our industry but we are committed to working as hard as possible to always provide as much data as we can to our clients for the best possible price without sacrificing quality. Any savings at the infrastructure that we achieve, we pass on to our clients.


# Events


We were happy to meet so many great people at the numerous events that we held, sponsored, spoke at, and attended in 2025. The list below is not exhaustive but covers quite a bit and it certainly had us on a very busy travel schedule.


## WTSFest London


This was our first[WTSFest event](https://www.womenintechseo.com/conference/) and we loved it. The community there was one of the most engaging and it gave us a chance to bring over Miki from our Japan team to join us.


Thanks so much to Areej and Erin, we will be there again in February!


## brightonSEO UK (April 2025)


[brightonSEO](https://brightonseo.com/) , both in the UK and San Diego, is always a great opportunity for us to connect with our existing clients and partners and meet new people.


We had a big group from our team join us in April and the event was a lot of fun, with great energy.


## SEO Week


[SEO Week](https://seoweek.org/) is Mike King and iPullRank’s event and what a great time it was. Mike pulled no punches in putting together an incredible lineup of speakers and even brought Busta Rhymes for the afterparty.


We also had a side event for SEO Week, a party that we hosted and we also hosted a great panel with Jori Ford, Zach Chahalis, and Ian Lurie.


We had a ton of fun as sponsors and we will also be there again in April. See you in NYC!


[SMX Advanced Boston](https://searchengineland.com/smx/advanced)


I hadn’t been to an SMX Advanced event since well before COVID, so I wasn’t sure what to expect but this was one of our favorite events of the year. The quality of the attendees and speakers was as high as you would expect from an event with the history of SMX Advanced.


## WTSFest Philadelphia


WTSFest has events all over the world and we were proud to join again, this time in Philadelphia. It was a different city and different vibe (both are great) and a great experience all around.


## brightonSEO UK (Fall 2025)


I spoke at brightonSEO in the UK and this was probably one of my most enjoyable experiences speaking, the audience seemed very engaged. I provided further information on the relationship between LLMs, SERPs, and the retrieval layer.


## brightonSEO San Diego (Fall 2025)


I spoke again in San Diego and covered the same topics that I did in the UK events.


## FOUND Tokyo


[FOUND Tokyo](https://foundconf.com/) was DemandSphere’s own global conference and it was the first time we had done anything on this scale.


You can see a video highlight reel of the event at the link to FOUND Conf[here](https://foundconf.com/) .


Some pics from the event:


We’ll be doing it again!


## Ayudante Toronto Seminar


Right after coming home from Japan, I drove up to Toronto to join our partner, Ayudante, for a very enjoyable time[speaking at their event](https://ayudanteinc.com/seminar-digital-marketing-strategies-for-the-ai-era-13-november-2025/) .


## Tech SEO Connect


[Tech SEO Connect](https://techseoconnect.com/) is such a great event, it’s hard to explain it if you haven’t been there. And if you haven’t, you should definitely make sure you go this year. We were proud to sponsor it again.


## Other events that we sponsored


We sponsored a lot of events in 2025, we believe it is important to help foster the community as much as we can. I know I’m forgetting a few but I got most of them here.


### Tech SEO North (UK)


Chris Lever and the team[run a great event](https://techseonorth.com/) in Sheffield, we are proud sponsors and hope to be able to attend in the future.


### Previsible Regional Events


We were proud to partner with[Previsible](https://previsible.io/) on a number of regional events in the US and each of them had a great turnout. Because the US is so big, I’m a big believer in these regional events because every major city is a whole ecosystem in its own right.


### SEO Beers in Seattle


We also enjoyed partnering with Shawn Huber and[Lastmileretail](https://www.lastmileretail.com/) on the monthly SEO Beers Meetup in Seattle. I was able to fly out to Seattle to hang out with some of the crew.


### SEO Beers Chicago


### Boardwalk Bash at brightonSEO San Diego


[Noah Learner](https://theseocommunity.com/about-us/noah-learner) (who also joined us at FOUND Tokyo as a speaker) hosts another favorite event of mine, the Boardwalk Bash, as part of the SEO Community’s events surrounding brightonSEO San Diego.


## Webinars and online interviews


In addition to in-person events, I also enjoyed speaking on a number of online events and webinars.


### Ayudante Webinar


Jasper Poon, of Ayudante,[put together a great webinar](https://www.linkedin.com/posts/jasper-poon_another-great-honour-to-host-a-forward-looking-activity-7355895120732327937-pTbi/) for the Asian market and he invited me to come on and speak about the future of AI search strategies.


### Semji Event


Semji hosted a fantastic online event called[SEO Square and I was invited to speak about AI search](https://seosquare.semji.com/replay-ray-grieselhuber-inscription/) . There was a great Q&A session at the end where I really enjoyed interacting with the audience.


### Sitebulb webinar with Aleyda Solis


Sitebulb hosted a webinar with the one and only Aleyda Solis and I was honored to also join as a speaker. You can watch a replay here.


[SEO for LLMs: Adapting to the AI-first Web with Aleyda Solis & Ray Grieselhuber #seo #webinar](https://www.youtube.com/watch?v=T9d4GOCpBIE)


### Sitebulb webinar with Zach Chahalis


We were also pleased to join Sitebulb, iPullRank, and Zach Chahalis (who recently joined the IPR team) on a 3-part webinar series called[The Search Matrix](https://sitebulb.com/resources/guides/speed-to-meaning-technical-measurement-blueprint-for-ai-search/) .


Zach provided one of the most in-depth walkthroughs to date on all of the technical aspects of optimizing for LLMs and AI search.


[How to do SEO to Show up in AI Overviews, LLMs & AI Search Systems #seo](https://www.youtube.com/watch?v=SY3e0o5pOtI&list=PLDY2psDlZTLVAJIrkZbSzlBakTHmRkexS)


### Interview with Shelley Walsh of Search Engine Journal


I really enjoyed speaking with Shelley Walsh, Editor at Search Engine Journal. She prepares extensively for all of her interviews and asked some of the most thoughtful and interesting questions I have encountered in an interview.


[Ray Grieselhuber: Focus On Human Behavior First, Not LLMs - IMHO](https://www.youtube.com/watch?v=sfCZoW1OvUM)


### Interview with Matt Bertram


I also enjoyed speaking with Matt Bertram, host of the Best SEO Podcast.


[Ray Grieselhuber Tells You How To Win With LLM’s](https://www.youtube.com/watch?v=qDEazqFyTo0)


## Partners & Customers


We were blessed with an amazing group of speakers, partners, customers, and sponsors in 2025.


In addition to the people and companies that I have mentioned above, we were proud to work with a whole host of companies and individuals to promote better visibility in AI search. This includes all of the wonderful speakers and sponsors of our FOUND Tokyo event.


## Thank You


Thanks again to all of our clients, partners, and friends for an amazing 2025. We wish you all the best for an even better 2026!
