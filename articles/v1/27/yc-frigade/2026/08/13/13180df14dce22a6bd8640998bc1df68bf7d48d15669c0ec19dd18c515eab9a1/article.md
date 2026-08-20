---
schema_version: "1.0.0"
document_id: "13180df14dce22a6bd8640998bc1df68bf7d48d15669c0ec19dd18c515eab9a1"
company_key: "yc-frigade"
company: "Frigade"
source_id: "yc-frigade-news-import-b5bda2b6da4c"
canonical_url: "https://frigade.com/blog/easier-to-build-not-easier-to-use"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T23:38:54.110805+00:00"
fetched_at: "2026-08-05T03:48:27.623827+00:00"
content_hash: "sha256:cb5954d0c1ff1109ac9237eedd49561ae5013382874c789db2f4323daedd2f0c"
---

# AI made software easier to build. And so much harder to use

Unless you're still writing code the acoustic way (i.e. by hand), I'm guessing AI now writes a large share of the code you're shipping. One cool benefit I've personally enjoyed with this is that it pulls you toward best practices even when you did not ask. Scaffolding, auth, and validation, the boring parts that have to be correct, show up more or less right the first time. Three years ago a big fraction of engineering was rebuilding those same wheels by hand, messing them up, fixing them, and taking away some learnings each time. Now they come with your IDE or harness.


The reasonable expectation is that this makes the end product better to use. Cleaner code and better defaults, so users should be getting less lost, not more.


That is not what we see. Over the last month[we read thousands of questions](https://frigade.com/blog/ten-common-product-problems-2026) real people typed into the AI assistants that sit inside their products. These were very different companies, in payments, logistics, property management, and sales tooling, with almost nothing in common at the product level. I expected the questions to be mostly bug reports and feature requests.


Almost none of them were. The large majority were people trying to do something the product already supports and not finding the path to it. Billing math they could not reconcile, a permission model they could not read, a dashboard number nobody had defined for them, a setting they knew was in there somewhere. The short version is that it was comprehension and not capability. Products seem to have grown into infinite complexity because adding a feature can be done so effortlessly.


When a feature costs almost nothing to ship, you ship more features than you should. My best guess is a lot of this is driven by sales and an over-obsession with competitors' features. "We keep losing deals to X because they have feature Y. Let's ship feature Y".


Every one you add is another concept the user has to hold in their head, and one more branch in the interface to get lost in. For a long time engineering teams were the bottleneck that kept products lean and well-defined. Building was expensive, so you were forced to be selective, and plenty of marginal features never got made because nobody had the time. The marginal feature is now cheap enough that it almost always clears the bar, and it turns out the bar was doing more work than anyone gave it credit for.


Designing, scoping, and building a feature used to be the slow part. Now the slow part is the judgment to decide a feature should not exist, or should ship in a far smaller form than the one that is easy to build. That decision got harder precisely because building got easier. Saying yes always feels reasonable in the moment, and the cost of yes now lands almost entirely on the user's attention instead of on your sprint.


I miss opening a piece of software and finding it did *one* thing incredibly well. Products like that existed partly because someone had taste, but mostly because nobody could afford to build more. And I suspect the scarcity is where the taste came from in the first place. When you could only build a fraction of what you wanted, you had no choice but to figure out exactly what the customer needed and mercilessly cut everything that wasn't it.
