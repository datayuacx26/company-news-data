---
schema_version: "1.0.0"
document_id: "bdb1474e7e9aa9d7d506fab0b93df6961dab9cfd2b743fdd450cd7d841b14115"
company_key: "yc-ditto"
company: "Ditto"
source_id: "yc-ditto-news-import-8ae80e1b68c3"
canonical_url: "https://www.dittowords.com/blog/global-expansion-simplified-how-benetics-manages-product-copy-across-five-languages"
published_at: "2026-08-19T00:00:00+00:00"
first_seen_at: "2026-08-19T23:53:52.106378+00:00"
fetched_at: "2026-08-19T23:53:53.085671+00:00"
content_hash: "sha256:593af6fd52c669c916bf2c2d61cb9898ab287447d5885d5f7ab8f6c54cec4ec6"
---

# Ditto Blog | Global expansion, simplified: How Benetics manages product copy across five languages

**Company:** Benetics — a communication platform connecting construction sites and back offices


**Location:** Switzerland


**Interviewee:** Richard Ekwall, Head of Engineering


**Use cases:** Multi-language localization, product copy management, engineering-built API sync, design-to-dev handoff


‍


Benetics builds an app that keeps construction sites and back offices in sync. Workers on site get their tasks for the day; the office sees what's been completed, what's outstanding, and can push updated plans out to the field.


As a Swiss company, Benetics has operated across multiple language markets from the outset — Switzerland alone has three widely spoken languages. The team started in the German-speaking market, expanded into France, Germany, and Austria, and is now moving toward Spanish- and Italian-speaking regions, with Eastern Europe on the horizon. They support five languages today and expect to support ten or more within a year.


For a product like this, localization isn't a finishing step. It's a core part of building. And as Benetics added languages, the way they managed translations started slowing them down.


## The challenge: Copy trapped in engineering


Before Ditto, every translation for thousands of strings across their entire product lived in GitHub — one file per language, each translation key sitting next to its text. It was a system that worked at two or three languages and strained badly beyond that.


The core problem was that updating copy required an engineer. Someone had to write the code, get the files into GitHub, open a pull request, and move the change through the normal engineering review cycle. A single string change meant opening five different translation files and assembling a change list for review.


That created two compounding issues. First, the people best positioned to improve translations couldn't. Benetics has native speakers of nearly every language they support sitting on the team — but none of them could touch the workflow without going through engineering. Second, because every edit was heavy, small copy improvements simply didn't happen. They got deferred, then forgotten.


## The solution: A UI built for copy — that engineers preferred too


Benetics adopted Ditto at the start of 2026, looking for a friendlier interface to manage translations and update strings without an engineering detour.


The part that surprised them: the engineers switched over too. Rather than hunting for a string across multiple files, everyone — technical or not — started making edits directly in Ditto.


> "Most of the team, when they update a translation, they would now go to Ditto and just update there rather than have to go and search for the string they're trying to update in different files."


## How it works: The workflow Benetics built


Benetics runs a two-way system between Ditto and GitHub, built entirely on Ditto's API. This enables them to keep copy in sync from design to engineering handoff, and gives them the tools they need to get the language right for every market.


### **Part 1: Keeping design, engineering, and copy in sync**


Copy starts with design. Benetics' UX designer works in English while designing screens in Figma, then hands those files to engineering, who implement them and create the corresponding translation keys in GitHub. Before Ditto, keeping copy current meant opening several files by hand and building a change list every time something moved.


Now, GitHub stays the source of truth, but a two-way sync — built entirely on Ditto's API — turns Ditto into a working layer on top of it. Any key added or changed in GitHub syncs into Ditto immediately. A second nightly sync, around 4–5 am, pushes everything changed in Ditto back into GitHub. It’s timed so an engineer can review it fresh each morning without blocking anyone's work in between.


The handoff is driven by status. Marking a translation "final" signals it's ready to sync back — a ten-second action that replaced hours of manual work.


The engineering team built the entire mechanism in under a week, and it's run cleanly since.


> "We've had basically zero issues with the Ditto API… I think we built a sync mechanism in less than a week."


### **Part 2: Getting the language right in every market**


Once copy is flowing between systems, the second half of the workflow is making sure what lands in each language is right.


English and German are the base languages for design and exploration. Before translation starts, Benetics puts guardrails in place: locale-specific style guides for all five languages, enforced automatically as translations are drafted. This means catching issues at the source, not after.


With guardrails set, the team uses Magic Translate to instantly populate the remaining variants — Italian, Spanish, French — then has native speakers QA and refine the output.


## The impact: Copy became something the team invests in


Even though the team unlocked immediate efficiencies, the biggest improvement wasn't speed. It was that Benetics started treating copy as work worth doing at all.


When editing a string required a code change, improving copy was never worth the engineering time, so it didn't happen. Lowering that cost changed the team's behavior. Small fixes now get made in the moment instead of being pushed to a someday "fix-it" — and those small improvements add up to how the product feels to use.


> "We’re actually investing more in copy by having Ditto… this focus on copy is something that has radically changed in the past six months. And I don't think that would have happened without a tool such as Ditto."


Beyond the day-to-day, having every language centralized in one place makes it easy to spot when variants have drifted out of sync. And it reshapes what expansion looks like. Today Benetics has the advantage of a native speaker for every language they support. As they scale toward ten or more, they will be able to bring in a contractor to own a language variant end to end, work it to a final state, and sync it back, all from Ditto.


Notably, moving copy off engineers' plates didn't make it something engineers stopped caring about. Because changes are now easy, engineers make them too — fixing copy the moment they notice something off, rather than letting it gather dust in the backlog.


## Advice for other teams


When asked what other technical teams should know before adopting Ditto, Richard's answer was less about tooling than about mindset. Users experience the words in a product as directly as they experience the features. Strong functionality wrapped in weak copy reads like an app that hasn’t ever prioritized the user’s experience — and that impression undercuts all the work your team has put in to building it.


> "If your functionality is great, but the copy is not so great… that could be off-putting for users. The copy part actually is a huge aspect of that."


*Want to see how Ditto fits your workflow?*[Book a demo](https://calendly.com/ditto-team/ditto-chat) *with our product experts.*
