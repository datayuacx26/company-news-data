---
schema_version: "1.0.0"
document_id: "83aea50304f2b7fdf1d29f6d896add2cc1acdfe5733bdb6ea7273788bcf9dc2a"
company_key: "ibotta-inc-class-a-common-stock"
company: "Ibotta Inc."
source_id: "ibotta-inc-class-a-common-stock-rss-dcf741155171"
canonical_url: "https://medium.com/building-ibotta/matching-rewards-to-products-with-transformers-fc4465a6c92"
published_at: "2021-08-31T17:21:45+00:00"
first_seen_at: "2026-07-25T01:07:04.216753+00:00"
fetched_at: "2026-07-28T21:04:47.382363+00:00"
content_hash: "sha256:811c8ec1b5de21e96887461bfff6bcf9ab6292733f88a31c2930e75bf7433618"
---

# Matching Rewards to Products with Transformers

Member-only story


# Matching Rewards to Products with Transformers


## BERT powered rewards matching for an improved user experience


[Brent Lemieux](https://medium.com/@brentlemieux?source=post_page---byline--fc4465a6c92---------------------------------------)


7 min read


·


Aug 31, 2021


--


Press enter or click to view image in full size


Photo by[Iqram-O-dowla Shawon](https://unsplash.com/@iqram_shawon?utm_source=medium&utm_medium=referral) on[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)


[Transformers](https://en.wikipedia.org/wiki/Transformer_(machine_learning_model)#:~:text=A%20transformer%20is%20a%20deep,part%20of%20the%20input%20data.&text=Transformers%20are%20the%20model%20of,%2Dterm%20memory%20(LSTM).) have changed the game for what’s possible with text modeling. At Ibotta, the ML team leverages transformers to power one of our core backend processes: matching reward descriptions to product descriptions.


[Ibotta](https://home.ibotta.com/) is a platform and mobile app that gives our users cash rewards on items they buy every day. Our clients, consumer product brands and retailers, work with us to offer cashback to our users for buying their products.


## The Problem


On the backend, our platform works by mapping rewards to a list of product UPCs (Universal Product Codes). However, these lists of product UPCs are often incomplete.


If a user buys a product that should qualify for a reward, but that product isn’t mapped on the backend, we’ll fail to give them the cash they’ve earned. This is bad for everyone involved. Our user didn’t get the reward we promised, and they had a negative experience with our platform and our partner’s brand.


Solving this problem is important to keep the trust of our users and partners.


## The Solution
