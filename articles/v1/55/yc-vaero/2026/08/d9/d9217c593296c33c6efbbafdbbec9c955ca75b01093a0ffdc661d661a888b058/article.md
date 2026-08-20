---
schema_version: "1.0.0"
document_id: "d9217c593296c33c6efbbafdbbec9c955ca75b01093a0ffdc661d661a888b058"
company_key: "yc-vaero"
company: "Vaero"
source_id: "yc-vaero-news-import-297f3f75de70"
canonical_url: "https://www.vaero.co/blog-post/personal-lessons-in-seo-with-high-volume-content"
published_at: "2026-08-07T00:00:00+00:00"
first_seen_at: "2026-08-05T00:12:30.667220+00:00"
fetched_at: "2026-08-05T03:48:27.623827+00:00"
content_hash: "sha256:1d3397a6375d7a51ad1f1c5299a3c6abe99a2ea5a94232158934a893839f6dbb"
---

# A Founder's Personal Lessons in SEO with High-Volume Content

As the founder of Vaero, I’ve never told the story of starting my first company. With Vaero’s work in the content space, it’s the perfect time to explain how my SEO journey started with a high-volume, scaled-content site called Justice Toolbox.


I started JT for idealistic reasons about access to justice, but I ended up learning a lot about SEO in the process!


The idea behind Justice Toolbox was to help people find a great lawyer by telling you how many cases every lawyer had won and what kinds of cases those were.


We ended up receiving 30k monthly unique visitors to the site, pretty handy given we were only targeting two states at the time, Maryland and Washington, D.C.


The site had tens of thousands of SEO-optimized pages, and we never once got flagged by Google for scaled content abuse because our pages had unique data that was actually helpful.


## Key Takeaways


- High-volume, scaled content based on unique court records achieved 30k monthly unique visitors across two U.S. states
- Long-tail strategy led to ranking for thousands of searches for lawyers’ names and case types
- Key techniques were applied for technical and content SEO for a high-volume approach with tens of thousands of pages
- If scaled to all 50 states, then the site was projected to reach over 1 million unique visitors monthly


## Leaving my job as a lawyer to start a lawyer-statistics site


The problem of finding a good lawyer was one I had seen over and over again.


At the time, I was a lawyer at a large and prestigious law firm in Washington, D.C. Inside pretty much every law firm there is an internal mailing list called variously “referrals” or “ISO \[in search of\]” where people inside the firm post asking for a referral to a lawyer for some specific problem. Most of the posts are for legal problems of a family member or friend for cases that are too small for the firm to take on.


It’s very difficult for a consumer to find a lawyer. Most people don’t know any lawyers, don’t trust any lawyers they don’t know, and don’t know how to evaluate lawyers they meet.


I thought that this problem could be solved with statistics – that there should be a website that listed all the lawyers, along with their track record of cases won and lost, which would let consumers choose the best lawyer for their case.


The court data for this was mostly available in online court databases, so it would be a matter of using a computer to go into those databases to gather the data, calculate the statistics, and publish them online. It seemed to me that making this data available to consumers was only fair, given that the court system and its data is paid for by taxpayers.


I gave notice and started the company. I really enjoyed my law firm, but I had a burning desire to solve this problem.


## Giving consumers access to legal data


It took several months, but using my computer science background I wrote a program that gathered court data from Maryland and Washington, D.C. and analyzed it to generate the legal track record for every lawyer in those states / districts.


When we launched, we were the only lawyer directory with real court case data and track records. The big lawyer directory sites at the time like Avvo only had contact information and user reviews (and most lawyers had few or no reviews).


A few other startups were also trying to work on the idea of court statistics at the time. However, most of them were focused on data for businesses, and did not make their data free for consumers.


Justice Toolbox was the only site where consumers could get free information on the track record of different lawyers from real court data.


## High-volume, scaled content as our primary GTM


The primary strategy I had for attracting users to the site was high-volume, scaled content. The strength of the site was our unique lawyer statistics and court data. Our strategy was to create a unique page of data for every lawyer, which would give us coverage of long-tail searches for the lawyer’s name or firm.


For a given lawyer’s name, usually the top results would be their law firm profile page and other legal directories like Avvo, but otherwise there wasn’t much competition.


‍


Every lawyer page on Justice Toolbox showed a wealth of statistics and court data about the lawyer


We also created category pages for each type of legal specialty x geography, so for example we could list all the criminal lawyers by case win rate for any specific town or county. This would help us get in front of users who were searching for a specific type of lawyer nearby.


As long as Google considered our pages valuable, then we’d be ranked for many thousands of long-tail searches.


## Real lessons from high-volume, scaled content


Since our entire GTM strategy was SEO, we had to take care to get everything right.


**Making category pages canonical** . For each category of case type x geography, we had a list of lawyers who handled that type of case in that geography, ranked by cases won. It took a substantial amount of technical SEO work to make sure each of these category pages had a single canonical URL, so that these could rank in Google. One annoying thing initially was making sure all the query parameters in the url appeared in a canonical order, since otherwise there’d be multiple versions of the URL floating around. It also meant we had to make the queries on our site use GET instead of POST, which annoyed me slightly from a technical perspective but was worth it.


‍


Category pages listed every lawyer for a specific case type in a city or town.


**Non-repetitive text.** Since we had a page for every lawyer, this meant we had tens of thousands of pages, and each of these pages was based on a template. At the time before really smart LLMs, it was very difficult to generate different text for every lawyer. So I was very scared of Google’s algorithm finding a pattern of repetitive text on our pages and automatically flagging us for scaled content abuse. That fear never ended up coming to pass. I used minimal templates for each page so there was as little repetition as possible and as much of the text would be different between pages as I could manage.


**Alt text** . Our main content pages for lawyers had a lot of graphs and charts to make the statistics easier for consumers to follow. However, this meant that much of the data was not native text that would be readable by the Google ranking algorithm. One of the key tactics we used from the beginning was making sure every graph had clear, strong alt text.


**Social tags** . Because our site had unique data for consumers, I also expected that there would be sharing on social media. To handle this, I automatically generated the open graph protocol tags on each page. Usually, images are a big part of social sharing and our site didn’t have any lawyer photos, just graphs. So one of the interesting things we did was automatically generating the social thumbnail that would appear based on our dynamically generated graphs.


## Result


The site received 30k monthly unique visitors, while targeting just Maryland and Washington, D.C. At one point, I calculated that based on the populations of those two small states compared to the U.S.A., that if we scaled to all 50 states we’d have traffic of over 1 million unique visitors per month – in the same ballpark as Avvo.


I would have done it, but the months of work it took to do just 2 states made it very daunting to try to do all 50. Every court database was different and their records were all in different formats. This made it very time consuming to write a computer program to collect data for each one.


The site was available for years. Eventually, I started working on other things and took it down. We received a lot of nice messages from people who had found the site helpful and interesting.


‍


‍


In the end, I changed the direction of the company completely and renamed it to Cognition IP, which applied AI to legal practice. We got accepted to Y Combinator, I raised close to $3 million in venture funding, and we served hundreds of startup clients. That’s a story for another day.


‍
