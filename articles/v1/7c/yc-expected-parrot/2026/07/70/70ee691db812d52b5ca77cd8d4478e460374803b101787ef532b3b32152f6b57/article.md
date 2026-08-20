---
schema_version: "1.0.0"
document_id: "70ee691db812d52b5ca77cd8d4478e460374803b101787ef532b3b32152f6b57"
company_key: "yc-expected-parrot"
company: "Expected Parrot"
source_id: "yc-expected-parrot-rss-aebdb8c877b2"
canonical_url: "https://blog.expectedparrot.com/p/marginal-ideas"
published_at: "2026-07-22T20:44:36+00:00"
first_seen_at: "2026-07-27T02:15:59.471977+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:208dd4ba32632c497490d5966498906fd1bac4731da7189b0fa3444bec5c3103"
---

# Marginal Ideas

# Marginal Ideas


### Using Expected Parrot to find clusters of opportunities


[John Horton](https://substack.com/@johnjhorton)


and[Amir Sariri](https://substack.com/@amirsariri)


Jul 22, 2026


Yesterday


[Amir](https://amirsariri.com/) presented our paper, “


[Marginal Ideas](https://amirsariri.com/assets/documents/sv/marginalideas.pdf) ,” at the NBER Summer Institute


[Entrepreneurship](https://www.nber.org/conferences/si-2026-entrepreneurship) session. Here’s


[a video](https://www.youtube.com/live/3_exm-tsQ_4?si=9pn9Gz9ES9yBrXkL&t=20944) of Amir giving the talk and


[Scott’s discussion](https://www.youtube.com/live/3_exm-tsQ_4?si=Hf3hWqEZMB2ygbdH&t=22068) .


The paper conceptualizes entrepreneurship as a kind of


[congestion game](https://en.wikipedia.org/wiki/Congestion_game) where anyone can pursue any idea they want. Entrepreneurs have a guess as to how good an idea is, but don’t know for sure unless someone pursues it. This causes entrepreneurs to spread themselves out over ideas, with good ideas attracting more entrepreneurs than seemingly bad ideas. In equilibrium, it’s the “marginal idea” that matters: the worst idea that still gets funding.


You can


[read the paper](https://amirsariri.com/assets/documents/sv/marginalideas.pdf) if applied microeconomic theory is your thing, but as this is the Expected Parrot blog and not the “cool tricks with supply and demand curves” blog, let us explain the empirical portion. The paper’s big problem is:


*How do you identify when startups are pursuing the same idea?*


## Birds of a feather (bid on the same keywords) together


If two startups are bidding on the same keywords to boost their websites in Google searches, they are likely competing for the same customers. Google’s ad auction data (if you can get it, more on that below) is especially nice because it shows what economists call a “revealed preference.” It means exactly what it sounds like. Imagine you want to infer whether John likes strawberry or goat cheese ice cream. He claims he would never eat goat cheese ice cream, but you can also observe what he actually pays for when he visits


[Toscanini’s](https://maps.app.goo.gl/PYHGvdtS3LMUCJwu5) ! Same idea. Startups pay real money, per click, to intercept a specific customer intent, so two startups bidding on “dog walker near me” are rivals in the only sense that matters economically, whatever their websites may say.


The idea in one line is:


*Get the keywords, then cluster startups based on the similarity between these keywords* .


## Problem 1: Most startups don’t actually buy keywords


So why not just look up what startups


*actually* bid on? We tried. Two problems. Most startups never run paid search at all, so the ones that do are badly selected—perhaps they have more money, or think they have a better idea. That’s a big problem because we don’t want to learn about a select group of startups who are different for reasons we don’t understand. We want to know about the


*population* of startups. But, even if all startups did have real marketing keyword bids, we need to buy this data from vendors that sell historical bidding information. Well, even after negotiating the “research use” quote with one of the largest vendors of these data, the price tag made clear it was probably meant for hedge funds, not academic research.


## Problem 2: No 10-Ks for Startups


The standard academic move doesn’t work here either. The


[best existing measures](https://hobergphillips.tuck.dartmouth.edu/) in Industrial Organization—the canonical field concerned with competitive forces of the economy—are built from the legally mandated business descriptions of public firms, obtained from their Form 10-Ks. These are fairly long, standardized, and legally required to be accurate. (Now you know how to find out what Palantir does.) For startups, though, there is no legally mandated anything for product descriptions. All we have is a blurb from Crunchbase, possibly written by the founders; sometimes marketing oriented, other times surprisingly puzzling.


## Problem 3: Semantic/topic similarity isn’t good enough for our purposes


Here’s one description from our data: “Application: Uber for dog walking. Hardware: Fitbit for Dogs.” You, a human, read this and think: okay, dog care with a health tracker! Every NLP method we tried though reads these words and matches this either with ridesharing platforms or wearable devices. The same method groups DoorDash and Instacart together because of the obvious delivery and food keywords in their descriptions, but one focuses on restaurant delivery and the other on grocery delivery. They are not direct competitors.


What Nano Banana Returns for “Uber for Parrots.” We have no idea what this hypothetical startup does.


## Enter the Stochastic Parrots (LLMs)


To get the keywords for all startups, we generate them using AI. We give an LLM agent the role of a Google AdWords specialist and hand it nothing but the vague blurb, then ask it to draft the ten non-branded search keywords this firm would bid on to reach customers who are already searching for a competing product. Figure D1 below from our paper shows the exact prompt.


Note that not all keywords are created equal! We can safely assume that DoorDash is far more willing to pay for “restaurant delivery” than for “grocery delivery,” and for Instacart it’s exactly the reverse. We also ask the same AdWords specialist to decide what fraction of their marketing budget they’d allocate to each keyword, mimicking what a human bidder does. The key point here is that the specialist isn’t asked what the company is, it is asked what the company would


*pay for* .


We run this as an Expected Parrot survey over every startup in our data. In short, we chose E\[P\] over direct API access because it makes experimenting, documenting, and large-scale piping of LLM surveys much easier. Here are a few examples of how it helped us.


We ran the same prompt across models from different providers and across different agent personas, in parallel, so figuring out what configuration produces best results was a couple of hours on a single script rather than an engineering project.


Another advantage is that the answers come back in the shape you asked for. Ask for a list of exactly ten keywords (note the


` max_list_items` and


` min_list_items` in Figure D1 above) and you get a list of exactly ten keywords. You can choose from several survey types, including Likert scale and dictionaries. Anyone who has regex-parsed “Sure! Here are your keywords:” out of thousands of raw API responses knows this is not a trivial problem.


## But what about…


Now, we anticipate that you are screaming “Garbage in-garbage out!” How do we know the keywords are good? The short answer is by “standing upon the shoulders of giants.” Recent work by economists on the


[frontier of AI and social sciences](https://www.benjaminmanning.com/) shows that LLMs excel at filling latent details from sparse inputs and predicting human behavior obtained from gold-standard randomized experiments.


Of course, we also do our own due diligence. To assess the risk of “garbage in,” we manually read predicted keywords from multiple randomly selected samples. To assess the risk of “garbage out,” we hand-label a subset of data (think of this as our test set) and assess the final clustering outcomes against it. As we’ll see, the results are quite good!


## From keyword portfolios to markets


We are one final step away from finding out who’s competing with whom. We just need to take these keyword portfolios and find pockets in which there’s “a lot of” overlap. The remaining challenges are fairly straightforward technical tasks. If you are familiar with these terms, we use a mutual k-NN graphs with Leiden clustering and find the model parameters using a hand-labeled dataset.


If you are not familiar with these terms, here it is in English. To turn pairwise overlap into markets, each firm nominates its closest competitors, and then we keep a link only when the nomination is mutual. By now, we have a massive hairball with almost all startups connected to each other, either directly or indirectly, and some strongly with lots of keyword overlap, and some weakly. To chop this graph up into small communities of competing startups, we use an algorithm that ensures each cluster’s members are internally connected and clusters pay a price for their size (otherwise, you have no way of knowing where to stop).


It works. Against our hand-labeled data, about 90% of the niche-mates assigned by the model are genuine market-mates (precision), and the niches capture about 80% of their market-mates (recall).


And the dog thing startup? Turns out there is more than one “Uber for dog walking” in our data, which, yes, is rather the point of the paper. There are also quite a few pet-wearable startups, and the startup with the puzzling description, was clustered with them (with Uber and Fitbit nowhere near to be found).


## What did we find??


Read & cite the paper.


Thanks for reading! Subscribe for free to learn more about how you can use Expected Parrot in your research.


A guest post by


[Amir Sariri](https://substack.com/@amirsariri?utm_campaign=guest_post_bio&utm_medium=web)


Economist studying technology entrepreneurship | UofT & Creative Destruction Lab '21 | MITSloan '25 | Current PurdueBusiness


[Subscribe to Amir](https://sariria.substack.com/subscribe?)
