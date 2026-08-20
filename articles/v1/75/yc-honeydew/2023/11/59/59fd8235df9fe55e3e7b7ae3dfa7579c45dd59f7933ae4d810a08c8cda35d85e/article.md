---
schema_version: "1.0.0"
document_id: "59fd8235df9fe55e3e7b7ae3dfa7579c45dd59f7933ae4d810a08c8cda35d85e"
company_key: "yc-honeydew"
company: "Honeydew"
source_id: "yc-honeydew-rss-6ec5327dfb7a"
canonical_url: "https://honeydew.ai/blog/on-the-value-of-expressiveness/"
published_at: "2023-11-28T19:07:15+00:00"
first_seen_at: "2026-07-20T23:20:19.939873+00:00"
fetched_at: "2026-07-28T21:01:37.179147+00:00"
content_hash: "sha256:7242f4ffcb4e52175dd23e355c9fcdad06f43097671d846456e528d5e42f5944"
---

# On the value of Expressiveness in Semantic Layers

ccasionally (ok, **very commonly** ) I find myself in a conversation comparing different Semantic Layers.


And while many times the conversation veers towards the fundamentals like performance, cost, or tool support, I think there is one crucial component lacking.


## ex·pres·sive·ness


*noun*


1. the quality of effectively conveying a thought or feeling. “you will be floored by the expressiveness of her eyes”


(source: Oxford Languages. They give expressive examples.)


What is the Expressiveness of a Semantic Layer and why does it matter so much?


Let’s get some semantics (hah!) out of the way:


A semantic layer captures how you calculate a thing. Like the quarterly revenue, or the daily user count. In miniature, it’s just a person who knows how to do it. However, when you scale, an *enterprise semantic layer* is a tool.


That tool can be hand-written scripts in SQL, dbt models, no-code ETLs, a metric layer, a traditional BI (such as SAP Business Objects, IBM Cognos or Microsoft Analysis Server), or pure semantic layers such as AtScale. Anything where shared business logic sits. It can even a multi-billion balance sheet done entirely in[Excel](https://sambf.substack.com/p/ftx-pre-mortem-overview?utm_source=profile&utm_medium=reader2) ( **don’t** ).


The tool can help many people be effective: business users working towards a KPI, data analysts trying to predict it, data engineers piping data for it and executives choosing between trusting data and trusting their guts. It powers dashboards, notebooks, applications. Look under any action taken by a business you admire on how it uses data - there is a semantic layer peeking.


It’s better be good.


## Expressiveness, explained.


What is the “ *Quality of Effectively Conveying a Thought* ” of an enterprise semantic layer?


*Effective vs ineffective communication of allowed parking times. Source: LA Mayor’s office.*


*Effectively Conveying* is about the effort needed to say something. If you convey a thought effectively you don’t do a lot of talking until someone understands. Take a look at the parking times above. One is faster to understand.


***Quality of Effectively Conveying*** is about ***what can you say* *effectively*** . Is it possible to describe a concept a short way? Do you even have the words needed to describe it in a precise way?


The LA picture above conveys allowed parking by the time of the week *very effectively* .


But, alas, it can’t effectively convey anything about the type of the vehicle, the passenger, the weather. In Florida, that might be a problem. After all, how do you tell that one must pay a meter when[parking an alligator or an elephant](https://www.spatzlawfirm.com/blog/2019/09/10-weird-laws-in-florida-you-probably-dont-know-about/) , but not when parking a giraffe?


## Applied to Semantic Layers


People build and use semantics in semantic layers (that is, until[AGI takes over](https://en.wikipedia.org/wiki/AI_takeover) . We got[a decade or so](https://www.metaculus.com/ai/) ).


You may have someone that[knows everything about your data](https://honeydew.ai/blog/so-lets-talk-about-semantic-layers/) and SQL. She can answer any question armed with just a query editor between her teeth.


But semantic layer serve lots of people. We need to consider what the *average data user* can do with it, not the all-knowing ***Uberanalytiker.***


Apply the concept a semantic layer and ask:


1. *How much skill and tribal knowledge does it take to encode an idea?* If only a few people have enough skills, then a semantic layer is less expressive.
2. *Which words (semantic constructs) does it have to express an idea?* If the semantic layer makes it hard to build that metric you want, then it is less expressive.
3. *How easy it is to understand what you meant?* If the semantic layer is akin to deciphering Egyptian hieroglyphs then is less expressive.
4. *What kind of new words can you build with it and reuse?* The more you have to repeat yourself, the less expressive it is.


A very expressive semantic layer is one many people can say a lot of stuff with, and many people can understand. A not-so-expressive semantic layer is one where only a few can.


By the way, did I mention the expressiveness of a semantic layer can be *negative* ? It’s the one semantic layer no one can change nor understand - but everyone has to rely on.


## Forget cost, performance, and reliability


(ok, maybe not reliability)


> **If a semantic layer is not expressive enough for your business domain, the cost of maintaining it can outweigh any business benefits it can provide.**


Run the numbers. Take two semantic layers: fast, reliable, and reasonably priced. But differ in expressiveness.


**Low expressiveness** : only a few (engineers) can use it effectively, and it takes them time.


If your 100 business users ask for one change a day, and a change takes an engineers day ($1000) to design, build, test, and deploy: maintaining the semantic layer would cost **$20m/year** .


**High expressiveness** : most changes don’t need an engineer, and when they do, it’s easy.


If your 100 business users ask for one change a day, but only 10% of those need an engineer, and it takes just an hour, then maintaining the semantic layer would cost **$250k/year** .


Expressiveness matters.


## But don’t forget the business domain


Business domain and expressiveness are related: the same semantic layer capabilities can cover fully one business domain (making it expressive within it) and fail miserably on another (making it a hard-to-use pile of mess). Sometimes can make a reasonable guess based on capabilities. Often, the only real way is to try and find out.


## Examples!


**Negative Expressivess Semantic Layer:** No one can change it, or explain what it does.


That is, except for the rare pockets of sanity colloquially known as “Someone’s Job Security”.


Example: 1000 scattered scripts in 12 different languages, some of them dead, and one of them is processing crucial data from an AS/400 and uploading it to an FTP server via an obscure VBScript running in a Windows 95 machine that no one knows where it physically is, but it seems to be connected to the network somewhere in Building 5. Please no one touch that building.


It powers reports everyone relies on, but you[really hope it does not break up](https://time.com/4348494/pentagon-nuclear-floppy-disks/) . If you get unlucky and have to fix it, the cost runs up into $Ms spent and years of clean up.


**Low Expressivess Semantic Layer** : Few people can change it, or explain what it does.


Example:[lots of SQL](https://forums.sqlteam.com/t/very-long-sql-query-with-lots-of-complex-calculations-and-conditions/15872) .


Or, a best-case scenario: a 1000 well-organized dbt models that are managed by a strong analytics engineering with a library of Jinja Macros and an affinity for continuous integration.


The organization gets the consistency and integrity of data the semantic layer provides. But the maintenance can get costly. The more complex the lineage gets, the changes are dependent on high-end analytics engineering work.


**High Expressiveness Semantic Layer** : Many people can build on it and change it.


When a good enough semantic engine is coupled with a business domain that fits it.


Examples:[Airbnb Minerva](https://medium.com/airbnb-engineering/how-airbnb-achieved-metric-consistency-at-scale-f23cc53dea70) ; A well-done[Looker Explore](https://cloud.google.com/looker/docs/best-practices/how-to-create-a-positive-experience-for-looker-users) ; A well-designed[SAP Universe](https://michaelwelter.wordpress.com/2016/02/02/universe-design-best-practices/) .


Or, on Snowflake, a[Honeydew](https://honeydew.ai/) 🙂 (due disclosure: I am a co-founder of Honeydew).


## Expressive Power to the Win


Cost, performance, reliability, integrations, are all valid ways to compare semantic layers.


However, we need one more. And as a nerd, I like best the Computer Science[definition](https://en.wikipedia.org/wiki/Expressive_power_(computer_science)) . It calls this concept **expressive power:** the expressive power of a language is the breadth of[i](https://en.wikipedia.org/wiki/Idea) deas that can be represented and communicated effectively in that language. The more expressive a language is, the greater the variety and quantity of ideas it can be used to represent well.


I like this definition because of the word power.


The term “expressive power of a semantic layer” conveys - and must say its does so with a high quality and effectively - the idea that having an expressive semantic layer is very **powerful** .


It is.
