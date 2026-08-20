---
schema_version: "1.0.0"
document_id: "67fd04bc73f42bf7e57b94df2cb177a65aad08607d8a37bbd282f5dc66210969"
company_key: "yc-authzed"
company: "authzed"
source_id: "yc-authzed-atom-b2bb1b68ff0a"
canonical_url: "https://authzed.com/blog/spicedb-playground-update"
published_at: "2026-05-12T12:00:00+00:00"
first_seen_at: "2026-07-20T23:20:06.042051+00:00"
fetched_at: "2026-07-28T20:50:59.865099+00:00"
content_hash: "sha256:09582d07087ce8f00d0b202d6f26ddae0196d44a16aa15784516bdf90c0da65f"
---

# The SpiceDB Playground just got an update

We shipped an update to the[SpiceDB Playground](https://play.authzed.com/) . The UI got a refresh, the tab system was reworked, and a handful of devex improvements landed based on user feedback. It's easier than ever to try SpiceDB directly in a web browser. Nothing to install!


My personal highlight: you can now edit your schema and relationships in the same view. No more flipping between tabs while you iterate. Just adjust a relation in the schema and tweak the relationships that exercise it without losing your place.


## What the Playground does


If you're new to the SpiceDB Playground, here's what you can do in there:


- **Explore example schemas.** A library of working ReBAC models you can open and read. Often the easiest way to pick up a pattern is to look at one that already works.
- **Write your own schema.** An editor for the SpiceDB schema language, with formatting and a graph visualizer for seeing how types and relations connect.
- **Define relationships and assertions.** Populate the model with the data it operates on, then write assertions that pin down what should and shouldn't be allowed. The assertions act as a regression suite when you change the schema.
- **Run real permission checks.** A build of SpiceDB is compiled into the page itself. Checks return the same answers a production cluster would.
- **Use the zed CLI in your browser.** The official command-line client is wired in too, so you can drive your model from a terminal if you'd rather work that way.
- **Share your workspace.** Schema, relationships, and assertions travel together in a link, useful for handing a teammate a model or sharing an example.


## Try it


If you're new to SpiceDB,[the Playground](https://play.authzed.com/) is the easiest way to start. We've added a few new example schemas in this update so there are more starting points to learn from.
