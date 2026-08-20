---
schema_version: "1.0.0"
document_id: "690c4bbb21d34fdb44dc326972aeb54ca82c09e7932f212b1055629cebf8245a"
company_key: "yc-icepanel"
company: "IcePanel"
source_id: "yc-icepanel-news-import-9cf2a09ec197"
canonical_url: "https://icepanel.io/blog/2025-12-15-new-releases-nested-groups-connection-via"
published_at: "2025-12-15T00:00:00+00:00"
first_seen_at: "2026-07-21T23:29:04.459524+00:00"
fetched_at: "2026-07-28T22:24:58.378758+00:00"
content_hash: "sha256:120037e1d4b9bd3c25b9d6341213ed6ae311154dfa46431f6daaea71008f746c"
---

# New — Nested Groups, Connection via, Duplicate objects, and more

We shipped some exciting new updates before we wrap up the year. Over the past month, our team tackled highly requested features, improved core actions, and fixed many paper cuts. Our fingers need a bit of a rest. With your support, we made significant strides in 2025 to make IcePanel the best tool for software architecture.


Happy holidays, and see you in 2026! ☃️


## What’s new? ✨


### Nested Groups 🪺


Create accurate deployment diagrams with nested Groups. Groups can now be nested 5 levels deep by changing the parent to another Group.


We also made a few other changes, including:


- Automatically adding the Group to the diagram when it’s created in a diagram.
- A new` Contains` field that shows objects 1 level below.
- Group nesting structure in the Model Objects list.


### Connection Via 🔀


We recommend modelling event-based flows by adding a technology on top of a connection, rather than using an object, to avoid the ‘hub-and-spoke’ design. We built on this practice with a new` Via` property on connections.


Create an object (for the topic or queue) and add it to the` Via` field on a connection. To get a global view of objects going in/out of the via object, select the object as the focus in the Dependencies view.


You can also filter by` Via` property in the Model Objects list and the Dependency view


### Right-click menu > Duplicate objects 🧱


You can now right-click in several areas of the app, such as diagrams and list views. The right-click menu gives several quick actions depending on the context.


Looking to create multiple variants or instances of the same object? Right-click the object, select ‘Duplicate object’, and give it a new name.


### Simplified org and landscape nav 👥


We simplified the organization and landscape dropdown, while still providing easy access for switching landscapes or organizations.


### Audit logs 📄


Scale customers now have access to audit logs. Find it in the Settings > Audit logs tab.


## Share your thoughts 💭


We’re excited to put all these updates in your hands. Drop us an email if you have any feedback to share —mail@icepanel.io 📩


Happy holidays & stay chill,


The IcePanel team 🧊
