---
schema_version: "1.0.0"
document_id: "82b378fdc88270175e5e52715bc5cc7fdf42a0117fb07ee3c71407de3577b498"
company_key: "yc-draftwise"
company: "Draftwise"
source_id: "yc-draftwise-news-import-f9e9b1f848aa"
canonical_url: "https://www.draftwise.com/blog/legal-ontology-part-2-what-an-ontology-actually-structures-inside-a-law-firm"
published_at: "2026-07-13T00:00:00+00:00"
first_seen_at: "2026-07-21T16:59:12.101501+00:00"
fetched_at: "2026-07-28T21:22:05.726331+00:00"
content_hash: "sha256:3874fb62abf247d132a719362f8258550bfad4500070f3ee1aa1f79ea21927b5"
---

# Legal Ontology Explained: What It Structures | Part 2 | Draftwise

In my last[post](https://www.draftwise.com/blog/what-is-a-legal-ontology-and-why-does-it-matter) , I wrote about what an ontology is and why the concept matters for a law firm. If the first post answered "why should I care," this one is closer to "what does the work actually consist of," because that question is where most of the real skepticism tends to live, and it deserves a direct answer.


Start with a distinction that's easy to state and easy to underestimate: a law firm's experience lives in two different places. Some of it lives in documents. Firms have spent real money organizing that part, through templates, filed precedent, and document automation. Some of it lives in people, in what a partner remembers about how a particular counterparty negotiates, or which associate handled a similar structure two years ago. Firms have generally not invested in that part because there wasn't an obvious way to do so. The knowledge isn't missing. It's just hard to reach unless you happen to know who to ask.


An ontology doesn't create that knowledge. It gives it a form that can be reached without knowing who to ask.


## Search Is a Poor Substitute for Ontology


Search has gotten better, and that's worth acknowledging rather than dismissing. Finding a relevant clause across a large set of documents is faster and more accurate than it was a few years ago. But search, even good search, has a limit that isn't about speed or accuracy. It retrieves text that matches a query. It doesn't know that the fund mentioned in one document, the LP mentioned in another, and the side letter mentioned in a third are the same set of related things, and it can't reason across that relationship, even if all three documents happen to come back in the results.


That limit only becomes visible on certain kinds of questions, the ones that are relational rather than simply about locating a passage. Something like: which LPs across a fund portfolio hold both an MFN right and a restriction on a particular kind of investment, and how would those two provisions interact if a specific new investment were made? No single document answers that. Answering it means holding several connected facts in view at once and reasoning across them, which is a different kind of task than finding a match for a query.


That's the real distinction I'd draw between search and structure. Search asks whether a firm's data can be found. An ontology asks whether it can be reasoned over. Those are related questions, but they're not the same question, and a lot of the confusion around this topic comes from treating them as if they were.


I'd also stop short of saying search and ontology are opposites, because they aren't. For much of what a lawyer needs day to day, a good search is genuinely sufficient, and building structure for its own sake would be wasted effort. The honest way to put it is that search covers most of the ground, and an ontology covers the part that search structurally can't, the relational questions. Knowing which category a given question falls into matters more than picking a side.


## Ontology Is Not Data Modeling


Building a legal ontology starts with deciding which things actually matter to the work: counterparties, deal structures, rights, restrictions, obligations. That part is largely definitional, and it looks different depending on the practice. What matters more, in my experience, is the level at which the modeling happens.


We build ours at the clause level rather than the document level, and that choice wasn't incidental. A partner asking about a keyman provision isn't asking about a document that happens to contain one. They're asking about the provision itself: how it's worded here, how it compares to similar provisions elsewhere, what pattern it fits into across the firm's other matters. Clauses are closer to the actual unit lawyers think in day-to-day. Modeling at the document level would technically be an ontology, but it would miss the granularity that makes it useful for how the work actually gets done.


Concretely, that means an LP's MFN right isn't stored as a passage of text that happens to say something about most-favored-nation treatment. It's represented as an attribute of the relationship between that LP and that fund, tied to a specific commitment level and specific conditions. Once it's represented that way, it can be queried on its own terms, compared against similar rights granted elsewhere, and connected to whatever else touches it.


## Ontology for Law Firms Is Different from Ontology for Enterprises


Most of the enterprise software that popularized the idea of an ontology was built for a different kind of data than what a law firm has. A manufacturing company's ontology connects things like parts, shipments, and facilities, where the underlying data is largely tabular and numeric to begin with: a table of orders, a table of inventory, a table of routes. Modeling that world means mostly connecting records that already exist in a structured form.


Law doesn't start out structured that way. The underlying reality, an obligation, a right, a restriction, lives inside prose, in a contract or a side letter written in ordinary sentences, not in a spreadsheet. Getting to the same kind of connected, queryable model means first accurately extracting the relevant facts from that prose at scale, then connecting them, rather than just connecting rows that were already sitting in a database. That's a genuinely harder first step, and it's part of why building this for a document-heavy field takes different tooling than building it for a field where the source data was already numbers in tables.


I think this is underappreciated in how the concept is generally discussed. The relationship-modeling part gets most of the attention because it's the more visible, more explainable half of the idea. The extraction part, turning unstructured legal language into something structured enough to model, is where most of the actual difficulty sits for a field like law.


## Ontology Is the Key to Proprietary Advantage


There's a property of this kind of structure that's worth naming directly, because it's the part that changes how I think about it as an investment rather than as a tool.


A document management system doesn't get more useful as it grows. If anything, it gets harder to navigate because more files mean more to search through and a greater chance that the relevant one is buried. An ontology behaves differently because each new piece of matter doesn't just add a file; it adds data points to a network of things the model already understands. A new fund closing connects to LPs the model already knows about. A new side letter connects to rights and restrictions that the model has already seen in other forms. The structure gets more capable, not just larger.


That's the part I'd underline for anyone deciding whether this is worth the effort. The definitional work at the start, deciding what a Fund is, what an Obligation is, how they relate, is genuinely slow and not particularly glamorous. But it's a one-time kind of cost that compounds against every matter that comes after it, in a way that adding another folder to a document system never does.


I don't think that, on its own, makes the case for any particular vendor or product, ours included. It makes the case for treating this as infrastructure rather than as a feature. The firms that get real value out of it will be the ones that treat the modeling work as something worth doing carefully, not something to delegate entirely to a tool and check back on later.


There's a smaller, more technical benefit to this that's worth a brief mention, since it's likely to matter more as firms rely on AI tools generally. A model that has to search through everything to answer a question is doing more work than one that can go directly to the specific records it needs, and that difference shows up in both accuracy and cost. An explicit structure gives an AI system a narrower, more precise place to look before it starts reasoning, rather than asking it to sift through a much larger and less organized set of documents each time. That's a separate benefit from the ones I've described above, and it's likely to become more relevant as more of a firm's tools involve AI in some form.
