---
schema_version: "1.0.0"
document_id: "f6bc2179b01ddb1f8a095095613f6137ab43ccfa98660167c5a48a820aa2a538"
company_key: "yc-blink-new"
company: "Blink"
source_id: "yc-blink-new-rss-0c236d2832c1"
canonical_url: "https://blink.new/blog/how-to-build-chrome-extension"
published_at: "2026-05-28T00:34:54+00:00"
first_seen_at: "2026-07-24T19:35:33.186925+00:00"
fetched_at: "2026-07-28T20:50:03.604597+00:00"
content_hash: "sha256:006cded19ca49b87fb4d13c6ccd6c005140e5dfffa5f86c980d230130b8f3837"
---

# How to Build a Chrome Extension With AI (No Coding Experience Required)

## The Files Every Chrome Extension Needs


A Chrome extension is a folder with four files:


```text
// manifest.json — the blueprint for your extension
{
"manifest_version"  :   3  ,
"name"  :   "Page Summarizer"  ,
"version"  :   "1.0"  ,
"description"  :   "Summarize any webpage with one click"  ,
"permissions"  : [  "activeTab"  ,   "scripting"  ],
"action"  : {
"default_popup"  :   "popup.html"  ,
"default_icon"  :   "icon.png"
}
}
```


**` manifest.json`** — the only required file. It declares what your extension is, what permissions it needs, and which files handle what.


**` popup.html` +` popup.js`** — the UI that appears when users click your icon. Standard HTML and JavaScript — no framework required.


**` content.js`** — the script injected into web pages. It has access to the DOM and can read or modify page content.


**` background.js`** — the service worker. It runs in the background and handles API calls, storage, and browser events.


Four files, all standard web technologies. No build step needed for simple extensions.


## Build It Step by Step


1


#### Define what your extension does in one sentence


Write this: "My extension \[verb\] the \[target\] so that \[user benefit\]." Example: "My extension summarizes the current page so I can skim long articles in 30 seconds." One sentence forces clarity. If you can't write one sentence, the extension isn't defined yet.


2


#### Generate your manifest.json with AI


Open[Blink](https://blink.new/) or any AI tool and paste your one-sentence description. Prompt: "Write a manifest.json for a Chrome extension that summarizes the current page. It needs a popup, a content script, and access to the active tab. Use Manifest V3." The AI generates the permissions, file references, and metadata. Request the minimum permissions possible — Chrome Web Store reviewers flag unnecessary ones.


3


#### Build the popup UI


The popup is an HTML file. Minimal structure:


```text
<!-- popup.html -->
<!  DOCTYPE   html  >
<  html  >
<  head  ><  title  >Summarizer</  title  ></  head  >
<  body  >
<  button   id  =  "summarize"  >Summarize This Page</  button  >
<  div   id  =  "result"  ></  div  >
<  script   src  =  "popup.js"  ></  script  >
</  body  >
</  html  >
```


Ask the AI to generate` popup.js` that sends a message to the content script when the button is clicked. The AI knows the Chrome messaging API — let it write the boilerplate.


4


#### Write the content script logic


The content script reads the page text and sends it back to the popup. Core pattern:


```text
// content.js — injected into the active page
chrome.runtime.onMessage.  addListener  ((  request  ,   sender  ,   sendResponse  )   =>   {
if   (request.action   ===   "getPageText"  ) {
const   text   =   document.body.innerText.  slice  (  0  ,   5000  );
sendResponse  ({ text });
}
return   true  ;   // keeps the message channel open
});
```


Ask the AI to extend this: extract only the article body (not nav or ads), clean whitespace, and send the text to an AI summary API. If you're calling an external API, that call belongs in` background.js` — not the content script. This is one of the Manifest V3 gotchas covered next.


5


#### Test locally with Developer Mode


Open Chrome and navigate to` chrome://extensions` . Toggle **Developer Mode** on (top right). Click **Load Unpacked** and select your extension folder. Your icon appears in the toolbar immediately.


Every time you change a file, click the refresh icon on your extension card. For content script changes, reload the target tab too. Right-click your popup and select Inspect to open DevTools — use the console to catch errors early.


6


#### Package and submit to the Chrome Web Store


Zip the entire extension folder contents (not a folder inside a folder — zip the files directly). Go to the[Chrome Web Store Developer Dashboard](https://chrome.google.com/webstore/devconsole) and pay the one-time $5 developer fee. Upload your ZIP, add at least one screenshot at 1280×800, write a specific description, and submit. Review takes 2-3 business days for new extensions.


Chrome extension submission: package your files, pass Web Store review, and reach 3 billion Chrome users


Blink


## Common Gotchas to Know Before You Ship


**Manifest V3 service workers don't run forever.** Chrome terminates idle service workers after about 30 seconds. If you need persistent state, use` chrome.storage` — not a global variable. The AI will write this pattern correctly if you specify V3 upfront.


**Inline scripts are blocked by CSP.** You can't use` onclick="..."` in your popup HTML. All JavaScript must live in external` .js` files. This is why` popup.html` links to` popup.js` — not inline handlers.


**Keep permissions minimal.** Requesting` <all_urls>` triggers an extra review and scares users at install time. Use` "activeTab"` instead — it grants access only to the current tab when the user clicks your icon.


**No remotely-hosted code.** Your extension can't download and execute JavaScript at runtime. All logic must be bundled in the ZIP. You can call external APIs — but the calling code must live inside your extension.


## When Your Extension Needs a Backend


Simple extensions — page readers, visual tweakers, quick tools — run entirely in the browser. No server needed.


Some extensions need more: user accounts, synced data across devices, secure API key storage, or usage analytics. If your extension stores summaries, tracks user history, or handles OAuth, you need a real API layer.


This is where[Blink](https://blink.new/) handles the backend automatically. Blink includes the database automatically — no Supabase account needed for the backend. You describe the API, Blink generates it with auth, database, and hosting included. Your extension calls the endpoint. No config, no DevOps.


A typical flow: content script grabs page text → sends it to your Blink backend API → backend calls the AI model, stores the summary with the user's account → returns the result to the popup. The extension stays lean; Blink handles the data layer.


If your extension needs a backend API, Blink handles database + auth + hosting in one platform. Build this with Blink — database, auth, and hosting included. No config needed →[blink.new](https://blink.new/)


## Chrome Web Store Submission Requirements


Review takes 2-3 business days. Prepare these before you submit:


**Required assets:**


- Extension ZIP (files at the root, not inside a subfolder)
- Icon at 128×128 PNG
- At least one screenshot at 1280×800 or 640×400


**Common rejection reasons:**


- Vague description that doesn't explain what the extension does
- Permissions that don't match the described functionality
- Missing privacy policy when the extension handles any user data
- Broken functionality at submission time


Updates to already-published extensions review faster — often within 24 hours. The $5 developer fee is one-time, and publishing extensions is otherwise free.


## Frequently Asked Questions


With an AI builder, you don't need to write JavaScript yourself. You describe what you want, the AI writes the code, and you test it. You do need to understand the basic file structure — manifest, popup, content script, service worker — so you can review the output and catch errors. That's learnable in an afternoon.


Manifest V3 is the current required format for Chrome extensions. It replaced V2's persistent background pages with service workers, tightened permissions, and blocked remotely-hosted code. Most tutorials from 2022 or earlier use V2 syntax that no longer works. If you're building in 2026, start with V3 and ignore older guides. The official[Chrome migration docs](https://developer.chrome.com/docs/extensions/develop/migrate) are the cleanest reference.


There's a one-time $5 developer registration fee. After that, publishing is free — no per-extension fee, no annual cost. If your extension is paid, Chrome Web Store takes a 5% revenue share. If it's free, there's no ongoing cost to keep it listed.


Yes. Your extension can call any public HTTPS API. Add the API's domain to` "host_permissions"` in your manifest. The HTTP call should happen in the service worker (` background.js` ), not the content script. If you need to keep API keys secure or store user data server-side, Blink provides the backend API layer — database and auth included, no Supabase account needed.


New extensions take 2-3 business days. Updates to published extensions are usually reviewed within 24 hours. Extensions requesting broad permissions (like access to all URLs) may take longer. You'll receive an email when approved or rejected, with the specific reason for any rejection.


---


*Building something more complex? Check out[how to build an AI chatbot](https://blink.new/blog/how-to-build-ai-chatbot-website) ,[how to build a Discord bot](https://blink.new/blog/how-to-build-a-discord-bot) , or[vibe coding for beginners](https://blink.new/blog/vibe-coding-for-beginners) — all follow the same pattern of describing what you want and letting AI generate the code.*
