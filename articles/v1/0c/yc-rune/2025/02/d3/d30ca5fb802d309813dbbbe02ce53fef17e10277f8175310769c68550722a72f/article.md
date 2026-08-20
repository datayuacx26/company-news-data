---
schema_version: "1.0.0"
document_id: "d30ca5fb802d309813dbbbe02ce53fef17e10277f8175310769c68550722a72f"
company_key: "yc-rune"
company: "Rune"
source_id: "yc-rune-news-import-1f6fc4a2bb8f"
canonical_url: "https://developers.rune.ai/blog/release-notes-january"
published_at: "2025-02-13T00:00:00+00:00"
first_seen_at: "2026-07-23T23:32:13.637334+00:00"
fetched_at: "2026-07-28T21:32:04.842955+00:00"
content_hash: "sha256:c3b27f6fb3e0910ef2da1c776971c37bbed94a952a617e6cd2931e2648e56854"
---

# New Updates! January 2025

## 🛠️ App Improvements​


- Added a fun and simple sound effect for gaining gems in the app, from daily rewards to quests 💎🔊
- 🎨 Introduced dynamic gradient height behind titles in game preview tiles to make game titles easier to read!
- Upgraded all login flow assets to be even sharper and more polished ✨
- Removed nonfunctional arrows from room info on the Room Details screen for a cleaner look!
- 🔢 Enabled one-time code autofill in keyboard suggestions for a smoother login experience.


## 🪲Bug Fixes​


- Fixed an issue causing weirdly small button text sizes by rerendering scalable text 🔤
- Prevented a bug where users weren’t showing up on cold-boot by ensuring we load and persist local users 👥
- Averted signed-in users with no wifi to a new no wifi screen so there’s no confusion and users can reinitialize the app then be returned directly to their previous screen! 📶
- 🔍Found and fixed a few more crashes related to unsafe casting of errors to objects.
- Added missing onScrollToIndexFailed handlers to prevent crashes 🚫📜
- Stopped a crash issue when the link email code error wasn't an object by removing unsafe casts.
- Resolve the problem where scrolling to the bottom wasn’t possible on searchable list screens when the keyboard was open ⌨️
- Upgraded our login flow health reporting for enhanced bug-busting in the future 📊
- Addressed an issue where gamers were occasionally added to a room twice on incoming calls by waiting for the user token 📞
- 🕒🔒Investigated and prevented failures during account activations by accommodating the longer time spent in sign-in state for first-time activations.
- Removed a 5-second delay before friend’s rooms are joinable which was causing problems where an incoming call was flashing briefly before the call interface rendered.
- ✅ Tweaked guest verification to behave more reliably when singing in takes longer than usual.
- Thwarted a race condition that could trap users in onboarding by ensuring the overlay loads at the right time 🚀
- Prevented a rare problem where gamers could get stuck right after entering their verification codes 🔐
- 👾🚫Blocked unnecessary gremlin screen from appearing when logging out and immediately logging back in!


Subscribe to our newsletter for more game dev blog posts


We'll share your email with Substack ⓘ Substack's embed form isn't very pretty, so we made our own. But we need to let you know we'll subscribe you on your behalf. Thanks in advance!
