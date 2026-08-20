---
schema_version: "1.0.0"
document_id: "3a4cfeacea06862d2e77021d9f431bcf130a6f5dba5f6fee59463dbb298efbce"
company_key: "yc-ditto"
company: "Ditto"
source_id: "yc-ditto-news-import-8ae80e1b68c3"
canonical_url: "https://www.dittowords.com/blog/introducing-magic-translate"
published_at: "2026-05-28T00:00:00+00:00"
first_seen_at: "2026-07-23T07:46:02.800033+00:00"
fetched_at: "2026-07-28T21:46:32.935029+00:00"
content_hash: "sha256:84d486c847ed02f66041996b85245e12868e5a92513544b940fe4913a0b4eff5"
---

# Ditto Blog | Introducing: Magic Translate

## **What it is**


Magic Translate generates a translation for any text item variant, right inside Ditto. You assign a locale code to a variant, open a text item, and Ditto produces the translated string in one click — no copy-pasting into a separate tool, no external translation service to manage.


The output isn't generic machine translation. Before generating,[Ditto pulls in any style guide rules scoped to that locale](https://www.dittowords.com/blog/introducing-ai-style-guides-customized-to-locale) — your approved terminology, tone of voice, formatting conventions — and uses them as context. If a character limit is set on the text item, that constraint is passed along to your AI generated translation, too. The draft lands directly in the variant's text field with a status of WIP, ready for a translator to review, refine, and mark Final.


## **Who it's for**


If you’re responsible for writing copy and generating translations, Magic Translate is your localization sidekick. Right now, getting a first draft typically means going outside Ditto — LLM in a separate tab, an external agency, a colleague on Slack. Magic Translate removes that trip. The draft comes back in the same place where the review and approval workflow already lives.


## **Why it matters**


The bottleneck in most localization workflows isn't the translation itself — it's the handoff. Getting strings out of the product tool, into a translation surface, reviewed, and back again takes time and creates drift. Every step is a place where the source copy might have changed, the context might be missing, or the character limit might not have been communicated.


Magic Translate collapses the first half of that loop. The string is already in Ditto. The context — what the text is for, how long it can be, what style rules apply — is already in Ditto. The translation comes back to the same place, with the same review workflow the team uses for base copy. Nothing leaves the system.


## **How to use it**


1. In the Variants page, open the variant you want to translate into and assign a locale code (e.g.` fr` ,` de` ,` ja` ).
2. Open a project and switch to that variant's view.
3. On any text item without a translation, click **Translate** . Ditto generates a draft using your locale-specific style guide rules and the item's character limit as constraints.
4. Review the draft, edit if needed, and set the status to Final when it's ready.
5. When engineers run` npx @dittowords/cli pull` , the translated strings export alongside base copy — same file structure, scoped to the variant's locale.


If you want Ditto to translate all new variants for an entire project, you can turn on auto-translate in the project settings.


1. Open project settings from the top right menu
2. Select **Variant Configuration**
3. Select a locale from the dropdown menu to automatically add to every text item in your project
4. Select the checkbox to **automatically translate variant text with Magic Translate.** Ditto will automatically populated translations for every empty variant with that locale.


‍


### Prefer to see the feature in action? Check out this quick (no sound) demo of Magic Translate:
