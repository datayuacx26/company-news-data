---
schema_version: "1.0.0"
document_id: "d1623e97e2d66115ccd5477dd75875faf546ac76ca4981764882b02aa78ab5a3"
company_key: "yc-tweeksio"
company: "Tweeks.io"
source_id: "yc-tweeksio-news-import-2df9936a897a"
canonical_url: "https://www.tweeks.io/changelog/2026-05-monthly-changelog"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-22T17:36:29.125136+00:00"
fetched_at: "2026-07-28T22:07:10.300477+00:00"
content_hash: "sha256:3cf15f01dcabe5f794785982389370167305bdf515ddb08ec99cf449fc771089"
---

# Changelog: May 2026

A fit-and-finish month. The Tweeks panel can no longer be buried by overlays that tweeks themselves create, and the extension now stays completely off captcha pages, so a tweek can never interfere with a security check. We also published a guide to[building email automations from any website with TW_email](https://www.tweeks.io/blog/email-automations-from-any-website-with-tw-email) .


## Improved


- **Straight to the code** — The View Code button on tweek pages now opens the editor with the code tab already selected, instead of landing you on the overview. Handy for checking what something like[Google Gemini Custom Background Colors](https://www.tweeks.io/t/C8lP7urF) actually does before installing it.
- **Share pages match the site** — Shared tweek pages now use the same top bar and footer as the rest of the website, so you're never stranded on a dead end.
- **Smoother installs from the website** — Installing a tweek, bundle, or profile from the web now hands off to the extension more reliably, including when you click install before the extension is fully ready.
- **Easier screenshot uploads** — Dashboard screenshot uploads now accept JPG and PNG files up to 4MB and shrink oversized images automatically instead of rejecting them.
- **Version pickers that handle history** — The version history and diff selectors in the editor now fit the screen, scroll properly, and tell you when older versions exist beyond what's loaded.
- **Leaving feedback when uninstalling** — The uninstall page asks better questions and takes less of your time.


## Fixed


- **The panel stays on top** — Tweeks that generate their own overlays could cover the Tweeks panel and its notifications. The panel now keeps itself above anything a tweek adds to the page.
- **Hands off captcha pages** — The extension now stays entirely away from captcha pages run by providers like Cloudflare, reCAPTCHA, and hCaptcha. The popup shows a read-only state on those pages so it's clear why tweeks aren't running.
- **Back where you left off** — Opening a tweek from Discover that no longer exists now returns you to your filtered Discover view instead of losing your place.
- **Firefox sign-in** *(Firefox only)* — Fixed an issue where Firefox could drop your signed-in session while the extension was idle in the background.


## Reliability


Generation got steadier at startup this month. Fewer generations fail before they even begin.


## Extension Version Coverage


This changelog covers extension versions 0.0.7.4 through 0.0.7.8. All changes are cross-browser unless noted otherwise.
