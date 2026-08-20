---
schema_version: "1.0.0"
document_id: "d8c135a5c81ad3d333ae481147eadb4eb4eb2dc6e74374d04d5d3d7d53c563e4"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-chrome-extension-ai"
published_at: "2026-06-07T00:28:10+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:49:23.535371+00:00"
content_hash: "sha256:0341d92256c3b8a05ec173042a429982ca72d328289bd376076cccfa404afc60"
---

# How to Build a Chrome Extension With AI

## 5 Chrome Extensions You Can Build This Way


**1. Productivity tracker.** Records time spent on each domain. Background service worker tracks active tab time. Blink stores the log per user and surfaces it in a weekly report popup. No local storage limits — the database syncs across devices.


**2. Reading list with smart categories.** One-click save to a categorized list. Content script grabs page title, URL, and a text excerpt. Blink stores the list and lets the user tag and filter entries. Syncs across Chrome profiles.


**3. Tab manager with saved sessions.** Snapshots all open tabs into a named session. Restores them later. Background service worker captures the current tabs. Blink persists sessions by user account — so a work session saved on one computer opens on another.


**4. Price tracker for Amazon and other retailers.** Content script scrapes the price on any product page. Blink stores the price history and sends an alert when the price drops. Users log in once — alerts go to email or appear in the popup.


**5. Custom new tab page with notes and tasks.** Replaces the default new tab with a personal dashboard. Blink provides the task/note API. The popup fetches and renders from Blink on each open. No third-party todo app required.


All five use the same architecture: extension frontend (AI-generated JavaScript) + Blink backend (auto-provisioned database, auth, API).


## Step-by-Step: Build Your Chrome Extension With AI


1


#### Describe your extension — one sentence, specific


Good: "Build a Chrome extension that highlights all prices on any page and saves them to a history list." Bad: "Make a shopping helper extension." The more specific your prompt, the more accurate the AI-generated code. Write the one-sentence description before you open any AI tool.


2


#### Build the backend with Blink


Go to[blink.new](https://blink.new/) and describe the backend your extension needs: "Build an API with user auth, a table to store saved items per user, and an endpoint to fetch the user's history." Blink generates the database schema, API endpoints, and auth layer automatically. No config. No Supabase account. Copy the API base URL — you'll need it in the next step.


3


#### Generate the extension code with AI


Open Cursor, ChatGPT, or your preferred AI coding tool. Paste your one-sentence description and add: "Use Manifest V3. The background service worker should call \[your Blink API URL\] with the user's auth token. The popup should show results from the API." The AI generates:` manifest.json` ,` popup.html` ,` popup.js` ,` background.js` , and optionally a` content.js` . These four files are the entire extension.


4


#### Wire the auth flow


The popup handles login: user enters credentials, the extension calls Blink's auth endpoint, and gets back a JWT token. Store the token in` chrome.storage.local` . Every subsequent API call from the background service worker includes this token in the Authorization header. The AI can generate this wiring — describe: "Add a login flow that stores the JWT in chrome.storage and passes it in API headers."


5


#### Load the extension and test it locally


Open Chrome. Go to` chrome://extensions` . Enable **Developer mode** . Click **Load unpacked** and select your extension folder. The extension icon appears in the toolbar. Test every flow: login, the main action, data persistence. Fix errors by pasting them into your AI tool with the prompt: "Here is the error. Fix the relevant code."


6


#### Submit to the Chrome Web Store


Zip your extension folder. Go to the[Chrome Web Store Developer Dashboard](https://chrome.google.com/webstore/devconsole) . Pay the one-time $5 developer fee if you haven't already. Upload the zip, write a description, add screenshots, and submit for review. Chrome's review team takes 1–3 business days for new extensions.


## The Chrome Web Store: One Honest Paragraph


Google reviews every submitted extension. The two most common rejection reasons are: requesting permissions the extension doesn't actually use (keep your` manifest.json` permissions minimal — only what you actively call), and unclear privacy disclosure for extensions that handle user data. Extensions with user accounts need a linked privacy policy. A simple Notion or Google Doc page qualifies. The review process itself is not difficult. It's the permissions and privacy policy that catch first-timers off guard.


Submitting your AI-built Chrome extension to the Chrome Web Store — one-time $5 fee, 1-3 business day review


Blink


For more on AI-powered app building, see the[best AI app builders guide](https://blink.new/blog/best-ai-app-builders) and[vibe coding for beginners](https://blink.new/blog/vibe-coding-for-beginners) .


## Frequently Asked Questions


Yes — a small amount. The AI generates most of the code, but you'll read it, understand what it does, and debug issues. The core files are short:` manifest.json` (15–30 lines),` popup.js` (30–80 lines), and` background.js` (40–100 lines). The AI handles the message-passing boilerplate. You handle the iteration — pasting errors back in and refining until it works. If you've never touched JavaScript before, budget an extra day for that learning curve.


The extension UI (popup and content scripts) can work offline for actions that don't need the backend — like highlighting elements on a page. Features that require data persistence, auth, or sync need a network connection to reach the Blink backend. Most premium extension features fall into the network-required category: saving data, syncing across devices, user accounts, and analytics.


Google reviews every extension before publishing. Reviews take 1–3 business days. Common rejection reasons: overly broad permissions (requesting` tabs` or` history` without using them), missing or vague privacy policy, and unclear purpose. Request only the permissions you call in code. Link a privacy policy if your extension collects any user data — even just an email address for auth.


A web app lives at a URL and requires the user to navigate there. A Chrome extension lives in the browser toolbar and reacts to what the user is doing on any page. Extensions have access to browser APIs — open tabs, page DOM, network requests — that web apps don't. The backend powering both can be identical: Blink's database, auth, and API work the same whether the frontend is a web app or a Chrome extension popup.


Yes. The Chrome Web Store has no revenue cut (unlike the iOS App Store's 30%). The most reliable model is freemium: free with a daily usage limit, paid for unlimited access. This requires tracking usage per user in a database — with Blink, that's a single database write per action. Extensions with user accounts and premium features regularly generate $5,000–20,000/month in the top 10% of their category.


Manifest V3 (MV3) is the current Chrome extension standard. Google stopped accepting Manifest V2 submissions in June 2025. Your extension must use MV3. The main changes from V2: background pages became service workers (which sleep when inactive), network request modification uses a declarative API, and Content Security Policy is stricter. Any AI-generated code should default to V3 — verify` "manifest_version": 3` in the generated` manifest.json` .
