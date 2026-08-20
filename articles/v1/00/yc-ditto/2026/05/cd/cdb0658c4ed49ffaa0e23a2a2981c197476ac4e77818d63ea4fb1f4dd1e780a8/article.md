---
schema_version: "1.0.0"
document_id: "cdb0658c4ed49ffaa0e23a2a2981c197476ac4e77818d63ea4fb1f4dd1e780a8"
company_key: "yc-ditto"
company: "Ditto"
source_id: "yc-ditto-news-import-8ae80e1b68c3"
canonical_url: "https://www.dittowords.com/blog/introducing-ai-style-guides-customized-to-locale"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-23T07:46:02.800033+00:00"
fetched_at: "2026-07-28T21:46:32.935029+00:00"
content_hash: "sha256:4a334d711a5a1c20351b101d681c7c4fb7266ad0a3a3b87e25447d5c509e8d4e"
---

# Ditto Blog | Introducing: AI Style Guides, customized to locale

## **What it is**


Style guide rules in Ditto can now be scoped to a specific locale. You can attach a locale code to an individual rule, or to an entire style guide, so that Ditto's AI tools know which standards to apply when generating or reviewing text for a given language.


Locale-scoped rules work with both[Magic Translate](https://www.dittowords.com/blog/introducing-magic-translate) (as context for generating translations) and Magic Edit (for enforcing standards when text is manually edited in a variant). A rule that applies to` fr-CA` will never fire for` de` or` ja` — each locale only receives the rules that belong to it.


## **Who it's for**


Content designers, localization teams, or product managers who are responsible for maintaining copy quality across multiple languages. If your team has documented standards that differ by locale — how dates are formatted, how formal the tone should be, which product terms should stay in English vs. be translated, how long strings can be — this feature is how those standards get enforced at scale rather than caught in manual review.


It's also useful for teams with external translation partners. Rather than writing a new translation brief for every project, you can codify the standing rules in Ditto once, and they travel automatically into every translation that touches that locale.


## **Why it matters**


Translation quality isn't just about accuracy — it's about consistency with the product's voice and the platform's conventions. A French translation that's technically correct but uses informal register when your product is formal, or formats a date` MM/DD/YYYY` in a market that expects` DD/MM/YYYY` , creates friction for users even if every word is right.


Without locale-scoped rules, AI translation tools apply generic language standards. They don't know that your brand uses` vous` not` tu` in French, or that your German copy prefers shorter strings because your UI is tight. Locale-scoped style guides are how you inject that institutional knowledge into the generation layer, so standards are enforced before a translator ever sees the draft — not after.


## **How to use it**


*To scope a rule to a locale:*


1. Open your style guide in Ditto.
2. On any existing rule, open the rule settings and select a locale from the locale picker (e.g.` fr-CA` ,` de` ,` ja` ).
3. Save. That rule now only applies to variants with that locale code assigned.


*To scope an entire style guide to a locale:*


1. Open the style guide settings.
2. Assign a locale code to the guide itself.
3. Every rule in the guide becomes locale-specific. Useful if you maintain a separate style guide per language (e.g. a dedicated French style guide with all your` fr-CA` conventions in one place).


*How it interacts with Magic Translate:*


When you run Magic Translate on a text item, Ditto looks up the target variant's locale code, finds all style guide rules scoped to that locale, and passes them as context alongside the source string. The translation is generated with your standards already applied — you're reviewing for quality, not correcting for consistency.


‍


Prefer to see these features in action? Here's a quick (no sound) demo of how to build your own locale-specific style guide:
