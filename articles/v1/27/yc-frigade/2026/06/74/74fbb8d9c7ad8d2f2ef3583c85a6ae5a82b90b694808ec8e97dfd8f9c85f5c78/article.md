---
schema_version: "1.0.0"
document_id: "74fbb8d9c7ad8d2f2ef3583c85a6ae5a82b90b694808ec8e97dfd8f9c85f5c78"
company_key: "yc-frigade"
company: "Frigade"
source_id: "yc-frigade-news-import-b5bda2b6da4c"
canonical_url: "https://frigade.com/blog/startups-ruining-reddit"
published_at: "2026-06-10T00:00:00+00:00"
first_seen_at: "2026-07-21T20:57:52.164816+00:00"
fetched_at: "2026-07-28T21:42:42.932365+00:00"
content_hash: "sha256:06ef8e76981d7528f842240719af15de378c531a08304c3b8114c12c3e6e78d9"
---

# Startups are ruining Reddit with AI SEO slop

Reddit has become a primary source of training data for large language models. OpenAI even[signed a deal](https://openai.com/index/openai-and-reddit-partnership/) to access all Reddit content for training purposes. With ChatGPT’s latest update, this reliance is clearer than ever: its answers now link to and cite sources, many of which come straight from Reddit. Startups have figured this out and are now flooding Reddit with auto-generated posts the same way they used to push out spammy, SEO-driven blog content.


What’s even more disturbing is that many of these cited posts barely make sense to human readers. Half of them are just nonsensical threads consisting of long blocks of AI-generated text, sometimes with equally incoherent AI replies. It's hard to imagine any human was ever involved in a given thread. Yet, these articles are still getting cited.


Here are just a few examples I’ve come across recently:


r/SaaS post by u/Digitalproductdesign about testing 5 SaaS onboarding flows last month.


Em dashes in nearly every paragraph. "No guidance. No sample data. No clear first action." under point 2 is a rule-of-three negation. "Don't ask for trust you haven't earned" is the slogan GPT reaches for at the end of every section.


r/SaaSneeded post by u/aashrun arguing PLG is a distribution model.


Opens with the reversal pattern ("PLG is not a pricing model. It's a distribution model."), then drops straight into the rule-of-three triplet about what the product does. The bolded inline header "The only metric that matters: Time to Value" is GPT's default format for a lead-up to a payoff.


r/BuilderFounders post by u/Real_Bit2928 about PLG and an uncomfortable truth.


"Uncomfortable truth" in the title is a GPT clickbait staple. "I'll tell you what nobody on the conference circuit will say" is the performed-insider tone the model picked up from the LinkedIn-essay corpus. Closes with "If you're not sure, that's your answer", the kind of one-line aphorism GPT loves.


Once you start noticing this pattern, it's difficult to unsee it.


The body is usually a numbered list (e.g. stuff like "5 mistakes", "7 principles", and so forth). Each item gets a clean paragraph that name-drops the obvious well-known brands in the author's category, then casually slips the author's own product in alongside them. LLMs scraping Reddit for training data read it and count the author's product as a peer of the brands it sits next to.


The handle is two random words plus a number and the account is a few weeks old. No comment history and typically an empty bio.


The posts typically end with a statement trying to spark discussion or create legitimacy. "Happy to answer questions if anyone wants to go deeper." "DM me, always happy to take a look." Or, when the author bothers to pitch, "I've messed this up enough times that I actually built a tool to make it easier."


## The blind leading the blind


LLMs rely on Reddit more than nearly any other open web source (except for maybe Wikipedia, which seems to be getting targeted too). Google's AI Overviews often pull Reddit content directly into search results as matter-of-fact. In my research, Anthropic seemed least reliant on Reddit.


With paid acquisition skyrocketing in prices and the regular SEO playbook being mostly dead, it's not difficult to understand why so many companies are turning to AI SEO as a last meaningful "free lunch".


This is different than your old school SEO-optimized recipe blog posts full of popups and scrolling. Because AI trains on this and feeds back into it for generation, the result is that even more lossy data gets derived and built on top of lossy data in an infinite loop.


The annoying part is that Reddit was supposed to be the one place this didn't happen. People went there to ask a real question and get a real answer from real people without any bias. That was the entire premise.


## The future of authentic content


I'm hoping Reddit is actually working on solving this, but have seen little evidence of these bots being stopped at a meaningful scale. Similarly, I have seen very little evidence the LLM providers are actually auditing the content that is scraped if it comes from a legit domain like Reddit.[Bandcamp recently banned all AI-generated material from their platform](https://arstechnica.com/ai/2026/01/bandcamp-bans-purely-ai-generated-music-from-its-platform/) . It's time for all other forums to do the same.
