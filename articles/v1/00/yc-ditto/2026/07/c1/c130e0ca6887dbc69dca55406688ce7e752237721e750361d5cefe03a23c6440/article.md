---
schema_version: "1.0.0"
document_id: "c130e0ca6887dbc69dca55406688ce7e752237721e750361d5cefe03a23c6440"
company_key: "yc-ditto"
company: "Ditto"
source_id: "yc-ditto-news-import-8ae80e1b68c3"
canonical_url: "https://www.dittowords.com/blog/from-four-months-to-four-hours-how-convatec-built-a-content-system-to-manage-complex-reviews"
published_at: null
first_seen_at: "2026-07-24T07:50:02.056295+00:00"
fetched_at: "2026-07-28T21:37:39.039431+00:00"
content_hash: "sha256:14f0e6448468a0e7d006af0e1d9c61a63d83791eb85c480db98966fca712708a"
---

# Ditto Blog | From Four Months to Four Hours: How Convatec Built a Content System to Manage Complex Reviews

**Company:** Convatec


**Industry:** Medical devices / digital health


**Use cases:**


- Review and approval workflow for regulated product copy
- Multi-language expansion
- Design-to-dev handoff


Convatec is a global medical products company, focused on helping people manage chronic conditions. Their digital product team builds and manages portfolio of apps to support the patients using their medical devices. This year, Mo Morales, Sr. UX Design Lead, and the rest of the digital team set out to build 2 new patient-facing mobile apps. And because of the regulations in medical tech, this meant that every single word — every label, button, error message, and onboarding prompt — had to go through review by medical, legal, and regulatory teams before it could ship.


## **The problem: Tedious copy-paste review processes**


The first patient-facing app that Mo’s team built out had nine major features and roughly 6,000 text layers. Getting the copy approved meant exporting screenshots of every screen from Figma, building a PowerPoint deck for each feature, adding text boxes next to each screenshot so reviewers could annotate, circulating all nine decks, collecting feedback, reconciling contradictory comments across medical, legal, and UX — and then manually updating the Figma file.


Then doing it again for translations.


"We can't go through that process again," Mo says. "We needed something more sophisticated."


The first app took four months to clear the review and approval process. The team was already building a second app — a provider-facing product launching just a few weeks later — when it became clear the old approach wouldn't scale: Across multiple apps, multi-language expansion, and a fast-moving team.


## Evaluating Options


Mo audited five different copy management tools before making a call. The requirements were specific: Figma-first, designer-friendly, and built for a small but extremely cross-functional team in a regulated space. One competitor's Figma plugin was breaking design components. Others were built for marketing teams, not product teams.


Ditto stood out for two reasons: the product fit, and the people.


“Morris was accessible and helpful during our trial of Ditto," Mo says. "This access to professionals has made all the difference.”


For a small team operating in a regulated space, the difference between a team that responds and one that doesn't isn't a nice-to-have. It's the difference between adopting something and abandoning it.


## **Building a Cross-Functional Workflow**


Once the team committed to Ditto, it was time to rebuild the process from the ground up.


**A naming system that finally connects everyone.** The team established a strict numerical taxonomy for screen naming, and mapped it directly to developer IDs on text items in Ditto. Before Ditto, engineers were naming text elements randomly, making it nearly impossible to track where a string lived in the app. Now, they’ve established nomenclature that maps from Figma design frames, to text items in Ditto, to Ditto dev IDs that get integrated into the codebase. When an app has nine features and 6,000 text layers, knowing where things live is foundational — and it carries all the way from design to implementation.


**Linking to simplify review.** A single feature can have 10+ different screen states. Rather than asking reviewers to slog through every variation, the team used Ditto's linking to reduce the review set down to only the unique screens — stripping away redundancy and surfacing what actually needed a decision. Because of this process in Ditto, Mo was able to strip away 75% of the design variations and edge cases, to only share the unique screens with legal reviewers. Then, when teams would suggest an edit on one screen, Ditto would automatically connect those changes to every instance.


**Live review sessions that resolve conflict in real time.** Mo ran a four-hour working session with all reviewers — medical, legal, and UX — together in Ditto at the same time. Contradictory feedback, which had previously generated lengthy async threads and unresolved disagreements, now got settled in the room. Comments landed in the activity log. Changes were tracked. The audit trail was built-in.


**Propagation that saves implementation time.** Once a string was approved, the team linked it through every subsequent use across the entire design file. This meant no manual updating or copy-pasting, and one developer ID for the reused string, to streamline integration with development.


## **The result: four months to four hours**


By running this new review process for the second app entirely in Ditto, Convatec cut their review timelines from "basically four months down to four hours."


Being able to hand off dev IDs to engineering — staying in sync on strings across the whole stack — was, in Mo's words, "a huge advance for us." In a regulated space, every decision needs a record. The activity log, comment threads, and assigned owners in Ditto aren't just organizational conveniences; they're (legally-required) documentation.


"The evolution of our process since introducing Ditto has been fantastic," Mo says. "I'm very pleased with the capabilities and how it's integrated into how we've been able to build a process around it."


## Phase 2 of Convatec’s Ditto Workflow


The Convatec team is already thinking about the next layer.


They're currently managing US and Brazil locales for the first app, with Italian and Spanish coming. They're working to build out Ditto variants for all translations, so translated copy can follow the same streamlined review process. Ditto’s ability to preview localized copy variations right in designs, without duplicating screens, was one of Mo’s reasons for choosing Ditto in the first place. Because localized copy is held to their same standards, and deserves a dedicated slice of their one holistic workflow.


And then there's the first app — all 6,000 text layers of it — waiting to be fully brought into Ditto for an iterative, quick-release approach going forward.


"I'm beyond pleased with the rate of feature upgrades, the quality of those feature upgrades," Mo says, "and I have a lot of excitement about the roadmap ahead."


The Convatec team didn't just adopt a tool. They built a content system — one with a shared naming taxonomy, a live review process, a design-to-dev handoff that actually works, and a foundation strong enough to support two apps, multiple languages, and continued growth.


For teams operating in highly regulated spaces, where every word has to be right and every decision needs a record, a cross-functional content system makes both speed *and* quality possible.


‍


*Want to see how Ditto fits your workflow?*[Book a demo](https://claude.ai/claude-code-desktop/local_425399b2-568a-4653-a791-d31ceb1530ba#) *with our product experts.*
