---
schema_version: "1.0.0"
document_id: "c223db5c42c1e888651e8c17b9aa78b9d69e1ea40254a093be45b34fe425d8ea"
company_key: "yc-camfer"
company: "camfer"
source_id: "yc-camfer-news-import-eadafe2b34d8"
canonical_url: "https://camfer.dev/blog/winning-o1-hackathon/"
published_at: "2024-12-10T00:00:00+00:00"
first_seen_at: "2026-07-24T23:30:43.881527+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:a234244bfd5553589fea6cd4a2b2961683545f33d1c9e2680718b82bb3c8bdf7"
---

# We Won OpenAI's Hackathon

OpenAI held a hackathon in October where they gave YC startups early access to o1. o1’s ability to do complex math problems and think deeply before responding is particularly useful to us, so we saw this as an opportunity to level up camfer.


Before o1, camfer’s designs were normally limited by the model’s instincts for dimensioning. For example, to design a 3in by 3in L-bracket, the model can infer that that the legs of the bracket should be 1in wide. This is just a heuristic-based assignment, which is great for designing simple parts from a single sentence. However, this type of design tends to break down at higher levels of complexity. For example, if you prompted the model to “draw a sketch of a 16 tooth spur gear with a pitch of 64 and 20deg from scratch”, it would require the model to do a fair bit of math before it could make a proper sketch. Even with heavy prompt engineering, our previous version of camfer couldn’t reliably go very far on a task like this. But with reasoning models, camfer could dive deep, calculate the pitch diameter, root diameter, construct the involute profile, etc. — and get a lot farther in the design process.


In the hackathon, we decided to see how far we could push this new capability. We plugged in o1, added prediction based sims and multi-part design orchestration to its tool selection, and asked it to iteratively design air foils. In our final demo, we prompted camfer to “design 5 airfoil profiles optimized for 50 mph with a minimum lift-to-drag ratio of 15 at a 5 deg angle of attack”, and it did! It iteratively came up with the initial spline design from first principles, tested its design performance in sims, and updated its designs to meet the given specs.


# Highlights & Demo


Here’s a clip of Sam and Garry talking about camfer and our hackathon win:


Here’s a demo of what we built at the hackathon on the Lightcone podcast:


# OpenAI Dev Day


Our demo was also showcased at OpenAI DevDay 2024, showing how giving LLMs great tools allows them to drastically improve performance in new domains.


# Obligatory Note


We were able to win the hackathon, but there’s still a lot of hard problems to solve to build a tool that mechanical engineers love using. If you’re interested in creating text/image-to-CAD models, post-training, model prompting, and building great products, please reach out to me at[arya@camfer.dev](https://camfer.dev/blog/winning-o1-hackathon/arya@camfer.dev) ! Our team is talented and passionate, and we’re looking for like-minded people to join us build the future of engineering.


---
