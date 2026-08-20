---
schema_version: "1.0.0"
document_id: "5d77c653b198f78cb2b4bd6b505b0b1d98848131778cdf25ad36738b3c4f9550"
company_key: "atlassian-corporation-class-a-common-stock"
company: "Atlassian Corporation"
source_id: "atlassian-corporation-class-a-common-stock-news-import-af8c2eac0472"
canonical_url: "https://www.atlassian.com/blog/how-we-build/scaling-localization-at-atlassian-keeping-translation-at-the-pace-of-ai-era-development"
published_at: "2026-08-06T14:37:52+00:00"
first_seen_at: "2026-08-13T01:30:56.386294+00:00"
fetched_at: "2026-08-13T01:30:58.023305+00:00"
content_hash: "sha256:dc852716add0b357bd5addd49cf70aae943d5263fd520ec360639ab37c646fb2"
---

# Scaling localization at Atlassian: keeping translation at the pace of AI-era development

*When developers started building with AI assistants, they began shipping features several times faster. That acceleration created a new challenge: every feature added text to translate, and the volume grew faster than our localization process could absorb. We responded on both ends: AI drafts translations, and tooling fixes source code before it reaches translation.*


### AT A GLANCE


- Translation volume grew **272% year over year** in the second half of FY25.
- AI now drafts translations, while professional translators still approve every string before it ships.
- i18n tooling helped clear **20,000+ existing issues** by fixing source-code patterns before they reached translation.
- Together, this reduces avoidable rework and creates a path to lower localization costs by **up to 50%** over time.


Atlassian ships product in more than 20 languages. Every translated string passes through a system that combines code, context, translation technology, and professional review so the product reads naturally and says what we actually mean. Professional translators bring the judgment that makes a customer in Tokyo or São Paulo feel like the product was built for them.


In the second half of FY25, the volume of words we sent for translation grew **272% year over year** as AI-assisted development accelerated. Every experiment adds strings; every new product adds more. Growth like that is welcome, but it strains the process. When demand grows faster than the translation pipeline can handle, deadlines slip and costs climb.


**So we made two bets:** **we used AI to draft translations and built tooling to fix source code before translation begins. Together, they make speed useful. Clean source gives AI the context to produce a strong first draft, while professional reviewers focus on the judgment machines cannot provide** .


## How we did it


##### Bet one: let AI draft the first pass


We moved product translation to a simple model: AI drafts, a human approves. The machine produces the first pass of every string using our translation assets. It then goes to a professional translator in the translation management system (TMS), who reviews it and signs off before anything ships. Quality stays in the translator’s hands.


The end-to-end AI translation workflow. AI drafts the first pass; every translation is still approved by a human in Smartling.


We ran this across two streams. One uses an internal AI translation system built at Atlassian. Before it drafts a string, it gathers what that string needs to be translated well: the developer’s notes, similar strings we have translated before, glossary terms, and the style rules for each locale. The other stream routes translations through an external AI vendor hub. Both feed the same human review step, and nothing ships without a translator’s approval.


The winning context-building approach from our trials: string-specific and reference context feed the prompt that produced vendor-beating drafts. The output is a draft that still goes to human review.


We did not take this on faith. Before rolling anything out, we ran a blind quality trial across several languages, and the AI-drafted, human-reviewed strings held their own against our best external vendor, so we rolled it out.


We track draft quality using Translation Edit Rate (TER): how much a human reviewer needs to edit the AI draft before it ships. Low TER is a useful signal: it often means the reviewer is polishing, not rewriting. We still rely on professional review because edit distance alone cannot measure quality. Across early production runs, TER has ranged from around 10% to just under 50%, depending on the language.


Those edit rates already unlock meaningful discounts on translation cost. Because every reviewer edit feeds back into the model, the translation edit rate (TER) continues to fall, pushing more volume into deeper discount tiers. Over time, we estimate this cycle can drive **up to 50% savings** .


The continuous quality loop: human edits become training signal that improves every future draft.


#### Bet two: fix the source before it ever reaches translation


AI can only work with the information it receives. When source text is incomplete, ambiguous, or assembled in a way that only works in English, the uncertainty reaches every language. A skilled translator can often investigate the intended meaning, but that adds time and still leaves room for inconsistency. The better approach is to make the source global-ready at the point where it is authored.


Source strings tend to break in familiar ways: untranslated text left in code, sentences stitched together from fragments, missing plural rules, or strings with no context. We built an internationalization validation system to catch those problems in the developer’s editor and again in code review, with guidance on how to fix them.


Take a text that says how many work items a user selected. The quick version works in English, but breaks elsewhere:


```text
// Before: assembled in code, breaks outside English
<span>{count} {count === 1 ? 'item' : 'items'} selected</span>


```


The system guides the developer toward one plural-aware message:


```text
// After: one string, plural-aware, ready for any language
formatMessage(messages.workItemsSelected, { count })
// {count} items selected
```


The code fix is only part of it. The AI also needs to know what the string means. A thin note gives the translator and the model the source string, but not enough context:


```text
// Weak: no idea what is counted, or where this appears
{
id: "workItemsSelected",
message: "{count} items selected",
description: "Selected count",
}


```


A useful note says what the string is, where it shows up, and what the variable stands for:


```text
// Strong: enough context to translate it correctly
{
id: "workItemsSelected",
message: "{count} items selected",
description:
"Toolbar label above a list, showing how many work items the user has selected. {count} is the number selected.",
}
```


The same work-item string shows why this matters downstream. Handed the string with a vague note, the AI translator has to guess what is being counted, the same way a human would. In German it reaches for a generic word and loses the plural rules:


```text
// Weak context: the model guesses
German:  "{count} ausgewählte Elemente"   // generic "elements", plural rules flattened


```


With real context attached and the glossary mapping “work item” to the Atlassian term *Vorgang* , the draft comes back in the right sense and grammar, and the reviewer polishes instead of rewriting:


```text
// Real context + glossary: the model gets it right
German:  "{count, plural, one {# Vorgang ausgewählt} other {# Vorgänge ausgewählt}}"
```


The descriptions were just one slice of a much bigger cleanup. Once we could see every problem across our frontend code, we pointed AI at the backlog: fix in bulk, open the changes as a PR, let a developer review. That combination cleared **over 20,000 existing issues** and knocked the backlog down sharply from its peak. The tooling catches the problems and keeps them from coming back, AI handles the repetitive code fixes, and a developer signs off on every change.


## What changed


- **Faster starts for translation:** AI gives translators a stronger starting point by combining translation memory, glossary, style guidance, and product context.
- **Cleaner source text:** developers get guidance before ambiguous, English-only, or non-global-ready strings reach translation.
- **Better use of human expertise:** professional translators spend less time untangling avoidable issues and more time on meaning, tone, and local fit.
- **A feedback loop:** reviewer edits show us where to improve the context and guidance used for the next round of translations.


## The lesson: scale the source and the workflow together


Translation does not become scalable just because AI can produce a draft quickly. It becomes scalable when the source is ready, the model has context, and professional translators make the final quality call.


If we had to do it again, we would start in the same place: fix the source, automate the first pass, and keep professional review where quality matters most. That is what lets localization keep pace with AI-era development without lowering the bar customers expect in every language.
