---
schema_version: "1.0.0"
document_id: "39a3a19e5f7baa2d6f9474b58024089aed3f513faa1a6272c7ace89723050f17"
company_key: "yc-tweeksio"
company: "Tweeks.io"
source_id: "yc-tweeksio-news-import-2df9936a897a"
canonical_url: "https://www.tweeks.io/changelog/2026-06-monthly-changelog"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-22T17:36:29.125136+00:00"
fetched_at: "2026-07-28T21:43:26.997349+00:00"
content_hash: "sha256:5149b0bfb1d0fa235cbf3900b02f01c58c46b81e3e4e3774e880e3cb02a32486"
---

# Changelog: June 2026

The big news: Tweeks is coming to iPhone, iPad, and Mac, and the Safari beta is open now on TestFlight. The rest of June went into making Tweeks fit you: onboarding ends with a Personalize step that sets how Tweeks behaves while you browse, the panel recommends tweeks for the site you're on and installs them in one click, and new installs ask for fewer browser permissions.


## New


- **Tweeks for Safari, in beta** — Tweeks now runs on Safari, including on iPhone and iPad. It's early, so we're soft-launching through TestFlight:[join the beta](https://testflight.apple.com/join/VmRKaPg7) and tell us what breaks.


- **Personalize Tweeks during onboarding** — A new Personalize step near the end of onboarding lets you set how Tweeks shows up: how often you want recommendations, which corner they appear in (with a live preview on the page), and where the panel handle sits. Accepting the defaults is one click. Onboarding also asks what you're here to do and adapts to your answer.


- **Recommendations in the panel** — The Tweeks panel can now recommend tweeks for the site you're on, featured ones first, and a badge shows how many are waiting. Install any of them in one click without leaving the page. You control how often recommendations appear, or turn them off entirely. Featured picks this month include[Remove AI Overview & AI Mode on Google Search](https://www.tweeks.io/t/b181f606d977491aad41e0f3) .


- **Share any script** — Scripts that only exist in your extension, whether imported or written by hand, can now be shared with a link like any other tweek.
- **Fewer permissions required** — New installs start with a smaller set of browser permissions. Clipboard, downloads, and notifications are requested only when a tweek actually needs them.
- **Preview features** — An opt-in channel for trying features we're still building. Turn it on during onboarding or in settings, and turn it off whenever you like.


## Improved


- **See what generation is doing** — Generation now reports each stage as it works, so you can watch a tweek take shape instead of staring at a spinner.
- **Tweeks MCP 1.1.0** — The MCP companion got a new release with more dependable browser automation for connected AI tools.
- **Link straight to source** — Public tweek pages support a` ?view=code` link that opens the source view directly.
- **Panel drag hint** — The panel now shows you it can be dragged to the other side of the screen.


## Fixed


- **Script guard whitelist** — Scripts you've whitelisted with the script guard stay whitelisted after they update.
- **"Not possible" details** — When generation determines a request isn't possible, the explanation now sticks around so you can read it later.
- **Tweek names in your language** — Fixed localized tweek names not displaying correctly.
- **Popup share routing** — Sharing from the popup now always opens the right page.
- **Browse links from the popup** — Per-site browse links now land on Discover with the right filter applied.
- **Settings readability** — Fixed low-contrast dropdowns in the settings page.


## Extension Version Coverage


This changelog covers extension versions 0.0.7.9 through 0.0.7.15. All changes are cross-browser unless noted otherwise.
