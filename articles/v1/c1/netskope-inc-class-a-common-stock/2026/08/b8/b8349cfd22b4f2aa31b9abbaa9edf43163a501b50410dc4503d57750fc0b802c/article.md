---
schema_version: "1.0.0"
document_id: "b8349cfd22b4f2aa31b9abbaa9edf43163a501b50410dc4503d57750fc0b802c"
company_key: "netskope-inc-class-a-common-stock"
company: "Netskope Inc."
source_id: "netskope-inc-class-a-common-stock-rss-c0a3e1ef9778"
canonical_url: "https://www.netskope.com/blog/ai-sidebar-extension-monetizes-its-own-updates"
published_at: "2026-08-11T11:11:00+00:00"
first_seen_at: "2026-08-11T12:45:49.511558+00:00"
fetched_at: "2026-08-11T12:45:50.804346+00:00"
content_hash: "sha256:3fb2f7ddc791f41a4f841036b6d02b8ce592477808360356216cf1d48c91b6c4"
---

# AI Sidebar Extension Monetizes Its Own Updates

The Chrome extension “[AI Sidebar with DeepSeek AI](https://chromewebstore.google.com/detail/ai-sidebar-with-deepseek/inhcgfpbfdjbjogdfjbclgolkmhnooop) ” that Google removed from the Chrome Web Store in January 2026 for stealing AI conversation content resumed shipping code to enterprise endpoints in July 2026.


The extension released a benign update removing the data theft code and acknowledged its wrongdoing. After 2 weeks it pulled the rug again with a new update.


Netskope Threat Labs analyzed the new build. While it no longer contains the conversation-exfiltration code, it now contains a monetization payload that opens an affiliate link in a foreground browser tab every single time the extension updates and uninstalls. Additionally, it suppresses the redirection of DeepSeek users to ChatGPT.


Extension’s Chrome Web Store page


## Key findings


- A Chrome extension had been delisted for AI conversation theft. Recently, it was relisted and continued to deliver updates to the surviving install base through Google’s CRX content delivery network.
- The operator used a clean-then-poison update sequence. Version 1.7.2.0 distributed no malicious payload. Version 1.7.3.0 introduced it as a surgical 21-line malicious insertion of a monetization payload.
- The monetization payload opens an affiliate link, while abusing a race condition to own the uninstall destination.


## Phase one: an AI sidebar that read the conversation


The extension entered the Chrome Web Store as an AI assistant sidebar, and by the time it was removed, it had reported more than 300,000 installs and a 4.6-star rating. Its functionality was real.


In December 2025, OX Security[published research](https://www.ox.security/blog/malicious-chrome-extensions-steal-chatgpt-deepseek-conversations/) showing that pre-1.7.x versions of this extension scraped ChatGPT and DeepSeek conversation content from the page and posted it, base64-encoded, to` deepaichats\[.\]com` and` chatsaigpt\[.\]com` on a recurring interval. Since then, the code was removed before v1.7.x, and the extension’s own privacy policy now acknowledges it.


Extension’s policy page


## Phase two: the same extension ID, a new build


In July 2026, Netskope Threat Protection detected and blocked the latest version 1.7.3.0 of the` .crx` object arriving on enterprise endpoints from Google CDN.


The extension is actively listed on the Chrome Web Store as of August 2026, seemingly re-approved for distribution, and it distributes the malicious build: a user installing the extension today receives the affiliate-fraud payload.


We have observed hundreds of users acquiring the malicious version daily; it is ongoing and growing at twice the rate of the previous version. The extension being relisted is the primary driver, auto-update secondary.


Extension version release timeline


Before the operator inserted the monetization code, it shipped Version 1.7.2.0, a functional, benign extension to establish a clean update history.


It was distributed from July 20 to 31, 2026, nearly 2 weeks before version 1.7.3.0 replaced it with the additional functions: The diff between the two versions of` blueBackground.js` is a surgical 21-line insertion.


## What version 1.7.3.0 actually does


What replaced the exfiltration code is a monetization chain built on the extension update and uninstall events. The service worker declares a partner URL and opens it in a foreground tab whenever Chrome reports an update:


“Partner URL”: the redirect URL Update listener opening “Partner URL”


The \`active: true\` argument means that no user interaction is required. The link resolves through Bitly to Buzzy, an AI video generation service that runs a public affiliate program[paying 30% commission on referred subscriptions](https://buzzy.tapfiliate.com/) . We have no indication the service is aware its referral program is being driven this way.


Comments were left, not only indicating AI-developed code, but also explicitly indicating “deliberately not on install”, which tells you the design intent. Firing on install would produce one referral per user. Firing on update produces one per user per release, and the operator controls the release cadence.


The uninstall path is claimed the same way, and here the code documents its own tradecraft:


Uninstall listener opening “Partner URL”


Chrome allows exactly one uninstall URL per extension, and the last writer wins. The bundled legitimate code registers its own after an asynchronous configuration fetch, so the operator writes the value twice, five seconds apart, to land after it, which means that a user who removes the extension still generates a referral on the way out.


## The operator


“Extchange.com” is named twice in the package, as developer and as data controller, with` info@extchange\[.\]com` as the privacy contact. The domain was registered in February 2024. Registrant details are not publicly disclosed. It is consistent with an operator whose cover is legitimacy rather than anonymity, using the Aitopia white-label SDK, shipping a genuinely functional product with revenue hooks bolted on.


Note the footer: “Developed by Extchange.com | Technology partner: aitopia.ai” , while Chrome Web Store listing publishes the extension under the developer display name “DeepSeek AI” with contact` info@deepseek\[.\]ai` .


## Conclusion


The interesting part is not the payload (Affiliate referral fraud is a low-severity outcome), but it is the delivery: a publisher that Google removed for data theft in January was relisted on the Chrome Web Store and still shipping code to enterprise browsers through Google’s own CRX distribution infrastructure in July. All it took was a clean release to build a trustworthy release history for only two weeks, to then surgically insert a 21-line payload into an otherwise identical build.


The same mechanism that delivered an affiliate link would deliver anything else the operator chooses to compile next – while monetizing such updates.


Relying solely on marketplace curation is no longer enough to protect enterprise environments from malicious browser extensions. To effectively reduce your attack surface, adopt a proactive control framework:


- Maintain fleet visibility: Keep a complete inventory of all installed extensions across your organization.


- Enforce default-deny: Block extension installs by default, allowing them only through explicit policy exceptions.
- Inspect inline content: Perform inline inspections on extension packages, even when delivered via vendor-owned content delivery networks (CDNs).
- Extend remediation: Treat an extension’s removal from an official web store as the prompt to begin internal remediation, not as proof that the threat is resolved.
- Treat extensions as a software supply chain, as a publisher controls what ships to existing installs through the vendor’s own CDN.


For this extension specifically, remove ID` inhcgfpbfdjbjogdfjbclgolkmhnooop` from managed browsers and confirm it is not present in any force-install or allowlist policy.


## Netskope One


Netskope Threat Protection inspects .crx downloads inline as part of Next Generation Secure Web Gateway (NG SWG) traffic and detects version 1.7.3.0 of this extension as` Trojan.GenericFCA.Script.37952` with Netskope AV and through Netskope Advanced Heuristic Analysis.


Netskope Advanced Threat Protection identifies the covert behavior: silent tab injection and uninstall-URL hijacking.


Netskope NG SWG URL Filtering categorizes the Phase 1 exfiltration domains chatsaigpt\[.\]com and deepaichats\[.\]com as C&C Server and Malicious Site.


Customers running Netskope Enterprise Browser (EB) can enforce extension policy at the browser layer: allowlist (only approved IDs load), blocklist (named IDs are blocked), or force-install (approved extensions are pushed automatically). Either the allowlist or a targeted blocklist entry for` inhcgfpbfdjbjogdfjbclgolkmhnooop` would prevent this extension from running, independent of whether the CRX download was caught at the network layer.


## Disclosure


We reported the extension to Google’s Chrome Web Store before publishing this post.


## Indicators of compromise


**Type** **Indicator** **Notes**


Chrome Extension ID inhcgfpbfdjbjogdfjbclgolkmhnooop Trojanized extension (“AI Sidebar with DeepSeek AI”)


SHA256 2a57de8abd0d15e92784280e5d39906d69a2ae7bb9b2875c77c626e9f0794e05 v1.7.3.0 CRX, malicious


SHA256 d3e0045b4151b360fdc054c5a7a0364afb3d8aea12dfeb38f3374bb977a8c579 v1.7.2.0 CRX, clean predecessor


URL https://bit\[.\]ly/3RPe03x Affiliate redirect; opened silently on every update and on uninstall (affiliate redirect, not C2; retargetable, destination can change)


Domain deepaichats\[.\]com Prior C2 for conversation exfiltration (pre-1.7.x; now offline)


Domain chatsaigpt\[.\]com Prior C2 for conversation exfiltration (pre-1.7.x; now offline)


---


Information presented in this post is based on aggregate usage data collected by the Netskope Security Cloud platform relating to a subset of Netskope customers.
