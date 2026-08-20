---
schema_version: "1.0.0"
document_id: "b3e3a8c05844045b0fa8ff658205a453f470ee2790f502cd741ffcae8bf63d1f"
company_key: "yc-rune"
company: "Rune"
source_id: "yc-rune-news-import-1f6fc4a2bb8f"
canonical_url: "https://developers.rune.ai/blog/release-notes-october"
published_at: "2024-11-12T00:00:00+00:00"
first_seen_at: "2026-07-23T23:32:13.637334+00:00"
fetched_at: "2026-07-28T21:33:00.470256+00:00"
content_hash: "sha256:86ed25955522cc416bbeac45cda32e544a5a8efc242ce038d261ed93ecd58f86"
---

# New Updates! October 2024

## 🛠️ App Improvements​


- Implemented design changes to the game details screen, making it look even better and easier to navigate ✨
- 🔄 Added pull-to-refresh in the choose game screen inside rooms, serving up new recommendations to gamers each time!
- 🎨🛒 Improved our purchasing UI with better visuals and a smoother flow between avatar options.
- Updated our "choose game" UI for better alignment when favoriting and easier game selection 🎮👌
- Upgraded a bunch of navigation pathways and flows throughout the app, making Rune feel more polished 🌟
- 🔴 Added small and sleek red dots to encourage gamers to customize their avatar and names!
- Improved the way our app does over-the-air updates so everyone can get the newest designs & material seamlessly 🧵


## 🪲Bug Fixes​


- Went on a bug-busting hunt this month! Tracked down and prevented a plethora of bugs in voice chat and rooms 💥🐛
- Refactored Rune's alert code and inadvertently fixed a few app crashes! Shout out to Denis 🚀
- Fixed the Rooms tabs by moving them back inside the header, similar to the search layout so the app is a cohesive experience throughout!
- Updated the game share aspect ratio to square, ensuring it looks better and fits on all screens 🖼️
- Made sure that all comments from a blocked or reported user immediately hide after refresh 🚫
- Disabled the unlock room button in matching rooms to keep the gaming experience between you and your new friend!
- Added ignoring 'rooms ending' events in the room if the call hasn’t started yet, avoiding false error reports 📞
- Adjusted our emoji picker for perfect visual alignment, eliminating jumping when selecting or deselecting on Android 😊
- Fixed a few cases where room ends weren't notifying the app properly, busting a few tricky bugs!
- Resolved an issue where gem totals weren't updating after a name change 💎
- Fixed an issue with game version selection, making sure that in dev game versions are not available for normal users. Thanks @iamlegend235 in our Discord for reporting it.


## 💻 SDK Improvements​


- Added an` isNewGame` flag to` stateSync` event that sets to true whenever there's a new game session (start of new game, restart) allowing devs to more easily handle game restarts 🕹️
- Updated our persistence code to bust a bug where players leaving wouldn't trigger the persisted state in the Dev UI!
- Prevented game errors by disallowing Rune.actions from being called inside update functions or other actions.
- Refined our logs to now include whether a user is a player, spectator, or unknown for all client-side messages 🔍


Subscribe to our newsletter for more game dev blog posts


We'll share your email with Substack ⓘ Substack's embed form isn't very pretty, so we made our own. But we need to let you know we'll subscribe you on your behalf. Thanks in advance!
