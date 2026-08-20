---
schema_version: "1.0.0"
document_id: "a51c3e77e6052fb1df47b329e4c2914b2fce95e589c64e3b29f8290c4a3e8a43"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2015-12-11-internal_hackathon/"
published_at: "2015-12-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:32.834035+00:00"
content_hash: "sha256:2163079f35143dfaa506b0da516a961afa02c1946875e8d377003791ea29c0c2"
---

# trivago Internal Hackathon 2015, 2nd edition

### Oh my God 39 projects!


Twice a year, the trivago software developers gather to have a 2 day internal hackathon. This December saw another round of ambitious creativity, relaxed atmosphere, and good food :-)


One of our core values at trivago is *fanatic learning* . A great way to learn and expand your knowledge is to creatively try new technologies, or to apply well known technologies to new situations, without having to pay attention to a strict schedule or deadline. Exactly that is what roughly 120 engineers were doing on the first Thursday and Friday in December: It was internal hackathon time, meaning each developer could spend two entire days on their own projects as he or she saw fit.


While some people prefer trying something all alone - or their project is so crazy/difficult that they cannot find anybody to join them - others form teams of up to 10 people. Since the trivago engineering team has grown consistently throughout the year, we ended up with 39 interesting and exciting projects - more than ever before.


Projects included:


- A “Vanilla Hotel Search”. This team built a trivago-like hotel search from the ground up, with great accessibility properties and no JavaScript requirements.
- trivago apps for Smart TV and Smart Watches.
- A Slack bot for coordinating socializing activities like ping pong breaks, coffee breaks, smoking breaks, etc.
- A crowdfunding platform to collect money for refugees
- A trivago version with partial offline capabilities
- An edX-based platform for internal learning
- An automatic city travel guide employing Natural Language Processing (NLP) techniques
- And many more


Several teams reached stunning completion levels in those two days (and, to varying degree, the night in between). However, as is often the case with daring tasks and a short time frame, not every goal that was set was also achieved. There are always unexpected difficulties, set-up problems, missing infrastructure, and food breaks - out great colleagues from the Talents & Organization department once again spoiled us with first-class breakfast buffets, lunch, and late-evening pizza.


But even though every goal was not reached, it was very clear that all participants enjoyed the freedom of pursuing their very own ideas and to work in groups and in ways they might not have worked in before. After all, knowledge exchange and knowledge sharing are the primary, overarching goals - and those were certainly achieved.


## Categories


As usual, there were awards in the categories of “Most challenging project”, “Most innovative project”, and “Most production ready project”.


### Most innovative and challenging project


This time, one and the same project was voted both most innovative and most challenging project. The team called themselves *LogPunkz* , and they created representations of trivago user traffic as sound, light, and as a 3-dimensional terrestrial globe view. In this globe view, coloured dots represent searches, forwardings, and requests in general. For example, if a user is currently in Sydney, Australia, and searches for “Tokyo” on trivago, a violet dot will float from Sydney to Tokyo on the globe shown below.


Additionally, you can call up information on each country’s individual traffic by hovering your mouse over it. On the screenshot, we see this information for Brazil.


The implementation of this project made use of our Kafka-powered logging infrastructure, and involved custom software written in Go. The audio component uses[portaudio](http://www.portaudio.com/) and[portmidi](http://portmedia.sourceforge.net/portmidi/) to convert the metrics from the logging into midi values. The light component makes use of[ArtNet](http://www.lightjams.com/artnet.html) , which is DMX over ethernet. The data flow is depicted in the diagram below.


### Most production ready project


The project voted “most production ready” is an old acquaintance: The *trivago Finder* , which combines a large number of various data sources and offers a unified, integrated, faceted search over them. The Finder already won the same award last time, and was significantly improved during this hackathon.


Using Solr as the search backend, trivago Finder offers search results from our company Wiki, our ticketing system, our enterprise social network, employee lists, source code repositories, and other sources - very useful in everyday work life, and blazingly fast.


## Conclusion


I’m always looking forward to the internal hackathons, and this time has once more proved why. The atmosphere is very unique - relaxed, but concentrated, creative, and cheerful. Everybody has time and space to breathe, think, come up with ideas, and put them to action. Therefore: It’s a good thing the next internal hackathon is only a few months away!
