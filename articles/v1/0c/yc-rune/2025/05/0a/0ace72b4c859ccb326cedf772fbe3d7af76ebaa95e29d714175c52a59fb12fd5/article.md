---
schema_version: "1.0.0"
document_id: "0ace72b4c859ccb326cedf772fbe3d7af76ebaa95e29d714175c52a59fb12fd5"
company_key: "yc-rune"
company: "Rune"
source_id: "yc-rune-news-import-1f6fc4a2bb8f"
canonical_url: "https://developers.rune.ai/blog/release-notes-april-2025"
published_at: "2025-05-23T00:00:00+00:00"
first_seen_at: "2026-07-23T23:32:13.637334+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:a681616ce4a4dd492ea2c22900ef0bc0c0219b6a0f091f205cd44398bb794c30"
---

# New Updates! April 2025

## 🛠️ App Improvements​


- Enhanced the login screen to display the invited realm when the app is opened from a realm invite link 🌐📨
- 🤝 Improved onboarding from a friend link—new users auto-add their friend when creating a realm, and existing users see a fresh splash screen.
- 🗣️⏱️ Deployed new room card variations in realms and DMs to clearly show who’s in the room and how long they’ve been talking!
- Made the delete account screen more compact and styled it to support auto graphic resizing, while removing scroll to better support accessibility settings.
- Updated the leave group button to appear disabled for group owners, with an alert explaining why they can’t leave if clicked 🚫👑
- Refreshed the join room and join realm buttons with a bold new look to align with our current designs ✨
- Officially rolled out the use of “realms” across the app, instead of “groups”!
- 🏁 Introduced default realm names during onboarding to make getting started even easier!
- 💬🌟 Refreshed the Friends tab to be “Chats” with updated styling and a clearer “No Friends” prompt to create a realm!


## 🪲 Bug Fixes​


- Fixed spacing between room cards in the room list to tighten up the layout and remove excess gaps 📏
- Polished onboarding with fixes for realm creation prompts, alerts for friend links while in rooms, and smoother swipe-back from realm chat 🎯
- Implemented several fixes, including QR screen titles, improved layout for editing realm names, better realm link navigation, and support for long realm names in details.
- Adjusted room card avatar styling to prevent crowding when many users are in the room 👥
- Fixed stories and bottom sheet avatar functionality so now tapping correctly opens the user’s profile page!
- Prevented an app crash by adding safeguards to unprotected route references in the code.
- Removed multiple navigation calls and switched to a different history reset method to help prevent another app crash 🔄📱
- Enabled automatic refreshing of groupmates when users are added, removed, or join, ensuring the most up-to-date room info.
- Fixed a cold boot issue where push notification taps didn’t work due to a login state bug—now handled correctly when already signed in 🔔


Subscribe to our newsletter for more game dev blog posts


We'll share your email with Substack ⓘ Substack's embed form isn't very pretty, so we made our own. But we need to let you know we'll subscribe you on your behalf. Thanks in advance!
