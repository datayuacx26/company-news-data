---
schema_version: "1.0.0"
document_id: "d47d6e2cf752dd63a7b9641e195b2057b85ac475d3f16920a74ac510976e98a8"
company_key: "yc-terra-api"
company: "Terra API"
source_id: "yc-terra-api-news-import-8e0bb378b82f"
canonical_url: "https://tryterra.co/research/why-does-athlete-physiology-resemble-postpartum"
published_at: "2026-07-28T00:00:00+00:00"
first_seen_at: "2026-07-28T17:06:51.606206+00:00"
fetched_at: "2026-07-28T21:33:52.463534+00:00"
content_hash: "sha256:f0a77a458b3eba435fd1362951caf8a7bda331370ef7ec59ebdd9236f2ef212b"
---

# Elite Athletes and New Mothers Share a Physiology | Terra

There’s this problem we’ve been tackling for a while at Terra Research: how do we transform information residing in a health space into information in a natural language space? How much can a health language model actually tell you?


In our internal experiments, we saw a much more impressive ability than we had thought was possible. A model that’s able to tell you that you were partying and drinking, that you were on your period, or even that you had a massage, simply off your wearable data. It sort of looked a little bit like magic, but it wasn’t.


When working with a language model that could be used across a wide range of people it’s really important to rigorously test for failure cases, and it is precisely while doing this that we found one that was incredibly interesting.


Our datasets are gender and sex-neutral. If our health is impacted by such a factor, we hope that the model learns it implicitly. Mostly, this seems to be the case. Except in our one very strange scenario, where our model ***kept telling athletic males that they were postpartum women.***


Loud and clear, breaking past all our confidence checks, the model was repeatedly telling a young man that his day *“Reads as a new-parent / caregiving state: fragmented, broken sleep consistent with caring for a baby (with a pregnancy/postpartum signature).”*


The first time we saw this we figured it was a one-off failure. A man that had fragmented sleep and looked like he was pushing his body hard. But then it happened again and we realized that we needed to dig deeper and look directly at the representations.


Our model works the same way all language models work today; in a representation space, people with similar health end up in similar areas. As we’re aware of how this space was formed, we are able to translate a person’s health back into the things they may be experiencing that day.


To understand whether this was a mistake our model would always make, we decided to check where athletic males and postpartum females landed in our representation space.


We checked for cosine similarity of health embeddings to our postpartum cluster centroid across a couple types of people. The numbers were, to say the least, surprising.


- New-mothers: 0.36
- General population: 0.12
- Typical women: 0.16
- Male athletes: ***0.20***


So our model would place an athletic man closer to the postpartum centroid than it would almost anybody except a real new mother. For a model that’s able to pick out massages this is a shocking mistake to make, but more importantly, it teaches us something about physiology: *A recovering new-mother has a health signature incredibly similar to that of an elite male athlete.*


It shows up in the data too, postpartum mothers have ***lower resting heart rates, higher HRVs, and a higher calorie burn*** .


Mistakes like this are bound to occur when we’re trying to differentiate concepts that may have health signals that differ very slightly and it’s always interesting to find something like this purely by accident.


*P.S. A public model within Terra ( **Health Terrain** ) is available on the dashboard for any Terra customer to run and has the same backbone as the model this article is about! If you’re one of our customers, check it out!*
