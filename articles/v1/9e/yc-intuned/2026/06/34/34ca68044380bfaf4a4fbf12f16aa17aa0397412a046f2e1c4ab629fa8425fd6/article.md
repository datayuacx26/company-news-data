---
schema_version: "1.0.0"
document_id: "34ca68044380bfaf4a4fbf12f16aa17aa0397412a046f2e1c4ab629fa8425fd6"
company_key: "yc-intuned"
company: "Intuned"
source_id: "yc-intuned-news-import-ba3d72b83236"
canonical_url: "https://intunedhq.com/blog/how-bot-detection-works"
published_at: "2026-06-02T00:00:00+00:00"
first_seen_at: "2026-07-25T09:50:45.873485+00:00"
fetched_at: "2026-07-28T21:24:31.593744+00:00"
content_hash: "sha256:9bd166f99e1fbe1fcac540b7c8e43b2824a616945af387b396a092b913573662"
---

# How bot detection works (and why your automation gets blocked)

**Key takeaways**


- Bot detection is probabilistic scoring, not a single check. Dozens of weak signals combine into one risk score that maps to an action: pass, challenge, block, or serve degraded data.
- It works in layers: IP and network, TLS and HTTP/2 fingerprinting, browser fingerprinting, automation-framework tells, behavioral analysis, and CAPTCHAs.
- The strongest signals are contradictions, like a Chrome User-Agent with a Node TLS (JA4) fingerprint, or a CDP-driven browser exposed by` Runtime.enable` .
- Fixing one layer isn't enough. A residential proxy IP paired with a robotic fingerprint or a` navigator.webdriver` flag still gets caught.
- Reliable automation means covering the whole stack, and stay in sync with every browser and tooling update.


You build a scraper. It runs flawlessly on your laptop. You deploy it to a server, and the same code starts getting empty pages, 403s, or CAPTCHAs it never showed you before. Somewhere between your machine and production, a bot detection system started paying attention.


This post walks through how those systems work, layer by layer, from the network connection up to behavioral machine learning. The framing is deliberately from the builder's side: for each layer, what signal the detector reads, and why a legitimate automation often looks robotic enough to get flagged. If you run scrapers, RPA flows, or crawlers against protected sites, this is a map of what you're up against, and where most automations quietly fail.


## What is a bot detection system actually trying to do?


A bot detection system separates two categories of traffic, real users and unwanted automation, without blocking so many real users that the business suffers. It does that with probabilistic scoring. Dozens of individually weak signals combine into a single risk score, and that score decides what happens next.


No single signal is decisive. A datacenter IP isn't proof of a bot. An empty plugins array isn't proof either. What a detector does instead is gather signals across every layer below, weight them, and map the result to an action: serve the page normally, run a silent challenge, show a hard CAPTCHA, block the request, or serve degraded data. Hold onto that shape, many signals to one score to one action, because it explains everything that follows.


## Layer 1: where is the request coming from?


The cheapest signal to check is the network one. Before any JavaScript runs, a detector already knows your IP address, and it can look up that address's reputation, its network owner, and how its traffic pattern compares to normal browsing.


IP reputation comes from services like MaxMind and IPQualityScore, plus the detection vendors' own databases of addresses tied to proxies, VPNs, Tor exit nodes, and datacenter ranges. Most real browsing doesn't originate inside a cloud provider, so a request from an AWS or GCP range raises the score. That heuristic isn't airtight: privacy services like iCloud Private Relay egress through cloud infrastructure, so real users do sometimes appear on datacenter-adjacent IPs.


ASN classification goes a step further. An Autonomous System Number identifies the organization that owns an IP block, which lets a detector sort traffic into residential ISP, mobile carrier, or hosting company. A headless browser on a cloud VM carries a hosting ASN, and that single fact is enough for many sites to route it differently. It's also the whole reason residential and mobile proxies exist.


Two more network signals matter. Request rate and distribution: humans browse at irregular, organic intervals, while naive automation issues requests on uniform timers or reuses one User-Agent across thousands of addresses. And IP consistency within a session: a session that begins in Berlin and jumps to Singapore five minutes later isn't one person, though VPN switching does generate false positives here.


The catch for builders is that the network layer is the easiest to address, which is exactly why detection doesn't stop at it. Swap in a residential proxy and you've cleared the first hurdle, not the race.


## Layer 2: TLS and HTTP/2 fingerprinting


Before the first byte of your HTTP request is even read, your client has revealed a lot in how it negotiates the connection. TLS and HTTP/2 fingerprinting turn the mechanics of that negotiation into an identifier, and it's where HTTP-client-based automation usually gives itself away.


When a browser opens a TLS connection it sends a ClientHello: the cipher suites it supports, the TLS extensions it advertises, the order they appear in, its elliptic-curve preferences, and more. Chrome on macOS produces a near-identical ClientHello across millions of installs. A Python` requests` call or a Node` https` request produces a completely different one.


The strongest tell here is a contradiction. If your automation sends a Chrome User-Agent but its TLS fingerprint matches a Go or Node TLS stack, that mismatch is a high-confidence bot signal, much stronger than the User-Agent string itself ever was.


HTTP/2 adds another fingerprintable surface. When the connection opens, the client sends a SETTINGS frame advertising values like header table size, maximum concurrent streams, and initial window size. Real Chrome sends a specific set in a specific order. Pseudo-header order alone is a strong discriminator, and most HTTP libraries get it wrong.


Header ordering rounds out the layer. Real browsers send headers in a consistent, browser-specific order and include client hints like` sec-ch-ua` ,` sec-fetch-site` , and` sec-fetch-mode` . A default-configured HTTP client tends to reorder headers or drop ones a browser always sends; even a missing` Accept-Language` is a classic default-automation tell.


The takeaway for builders: spoofing a User-Agent is one line of code. Reproducing a coherent TLS-plus-HTTP/2-plus-header fingerprint is much harder without a real browser engine, which is why detection-resistant automation tends to run on real Chromium instead of raw HTTP clients.


## Layer 3: browser fingerprinting


Once a request clears the network checks, the detector ships JavaScript to the page and probes the browser environment. This is where the most signal volume lives, because a browser exposes hundreds of properties that vary by hardware, operating system, and configuration. A simplified version of what a detection script collects looks like this:


```text
// A trimmed-down sketch of what a fingerprinting script reads
const   signals   =     {
webdriver  :     navigator  .  webdriver  ,     // true under naive automation
plugins  :     navigator  .  plugins  .  length  ,     // historically 0 in old headless
languages  :     navigator  .  languages  ,     // missing or malformed in some builds
deviceMemory  :     navigator  .  deviceMemory  ,     // 0.25–8 in spec; Chrome now reports up to 32
cores  :     navigator  .  hardwareConcurrency  ,     // a 64 here on a server is a tell
renderer  :     getWebGLRenderer  (  )  ,     // GPU string; SwiftShader means no real GPU
}  ;
// these get hashed and compared against profiles of known real devices
```


The most obvious property is` navigator.webdriver` , which a browser sets to` true` when it's driven by WebDriver (Selenium, Playwright, Puppeteer). It's trivially removable, but it illustrates the whole category of "the automation announced itself."


Headless tells used to be easy. Older headless Chrome leaked through an empty` navigator.plugins` , a missing` window.chrome` , and a SwiftShader software renderer where a real GPU should be. Chrome's new headless mode, the default since around version 132, closed most of these, so the "just check for headless" advice from a few years ago no longer holds. Vendors keep finding fresh gaps, but the cheap ones are gone.


Several richer probes do the heavy lifting now:


- **Canvas fingerprinting** draws text and shapes to a canvas and reads the pixels back as a base64 string. The exact output varies by GPU, driver, OS, and font renderer. Cloud instances tend to produce clustered or identical canvas fingerprints, which is itself suspicious.
- **WebGL fingerprinting** reads the GPU directly through the` WEBGL_debug_renderer_info` extension. A GPU-less cloud instance reports something like` ANGLE (Google, Vulkan 1.3.0 (SwiftShader Device (Subzero)), SwiftShader renderer)` instead of a real card. Some browsers now gate this behind anti-fingerprinting settings.
- **Audio fingerprinting** runs a tone through an` OscillatorNode` and reads back the buffer. The floating-point result varies by the OS and browser audio stack (not the CPU model itself), and it's hard to patch consistently.
- **Coherence checks** look for internal contradictions: does the reported screen resolution, timezone, and language agree with each other, and with the IP's geolocation? A mismatch across these is a strong composite signal.


Hardware properties add a little more.` navigator.deviceMemory` and` navigator.hardwareConcurrency` report rounded RAM and CPU core counts, so a headless instance on a 64-core server happily reporting 64 threads stands out. (The old Battery Status API barely matters anymore; it's been removed from Firefox and Safari and only Chromium still exposes it.)


For builders, the hard part isn't faking one value. It's that the whole profile has to stay internally consistent, and a clean, plausible fingerprint usually means patching Chromium at the source rather than overwriting properties in page JavaScript, which detectors can themselves detect.


## Layer 4: automation framework detection


Beyond generic headless tells, detectors look for the fingerprints of the specific tools that drive the browser. This layer moves fast, because each version of Playwright, Puppeteer, or Selenium opens and closes different leaks.


The most important signal in the current generation comes from CDP, the Chrome DevTools Protocol that Playwright and Puppeteer use to control Chromium. CDP traffic isn't visible to a web page, but using it leaves side effects. Enabling CDP's runtime domain (the` Runtime.enable` command) changes how the browser serializes certain objects and error stacks, and a detection script can observe that difference. This serialization side-channel, not a magic global variable, is the current high-signal way to spot a CDP-driven browser.


Older techniques hunted for leaked globals, and those still get checked. The real ones Playwright has exposed are along the lines of` __playwright__binding__` and` __pwInitScripts` , not the` __playwright` or` __puppeteer` names that float around in older write-ups. Detection scripts walk the` window` object for whatever the current tool version happens to leave behind.


One widely repeated check deserves a correction, because building around it wastes time. The myth is that you can catch automation by reading` event.isTrusted` , on the theory that automated clicks are untrusted. That's wrong for CDP-based tools. Input dispatched through Puppeteer's or Playwright's normal APIs travels through CDP's Input domain, and the browser marks those events` isTrusted: true` , indistinguishable from a genuine click on that property alone.` isTrusted` is only` false` for events synthesized in page JavaScript with` dispatchEvent()` . So the check catches a naive script calling` element.click()` through injected JS, but not a properly driven Playwright session.


A couple of smaller artifacts round out the layer. Real Chrome populates` chrome.runtime` , and some headless builds initialize it differently. The Permissions API has historically behaved differently in headless (the classic` Notification.permission` versus` permissions.query` mismatch), though new headless narrowed that gap too.


The main point we wanted to highlight here is these leaks are version-specific, a stealth patch written for one Playwright release can stop working on the next. This is the layer that quietly breaks an automation after a routine dependency bump.


## Layer 5: behavioral analysis


Everything above can be judged from a single page load. Behavioral analysis is different. It watches what the client does over time, and it's the hardest layer to fake, because it requires reproducing the texture of human interaction rather than passing a static property check.


Human mouse movement is curved, variable, and noisy. It follows Fitts's Law, where the time to reach a target scales with the distance and the target's size, and it shows micro-corrections as the cursor settles near a click target. Programmatic movement tends to travel in straight lines or in mathematically smooth curves no hand produces. Detection systems capture the mouse event stream and run it through models trained on human and bot movement.


Scrolling tells a similar story. People scroll with variable momentum and pauses, while automation often doesn't scroll at all (it has no need to render content) or scrolls in suspiciously even increments. Click and form timing add more: a button clicked ten milliseconds after load, every single time, is automation, and human form-filling carries dwell time on fields, corrections, and a non-linear path through the inputs.


Detectors also read the session as a whole, not just one page. Which pages were visited, in what order, and for how long? Human sessions wander; bot sessions are purposeful and repetitive. Honeypots sharpen the picture further: invisible links and form fields that a human never sees and therefore never touches. Interacting with one is a near-certain bot signal with very few false positives.


This layer is also where detection of AI agents now lives. As legitimate automated agents proliferate, from search crawlers to shopping and research agents, vendors increasingly try to separate sanctioned agents (often identified by published IP ranges or signed identities) from unsanctioned automation, rather than treating all non-human traffic the same.


## Layer 6: CAPTCHAs


CAPTCHAs are the visible challenge layer, deployed when earlier signals are ambiguous or on high-value actions like login and checkout. The puzzle itself is rarely the hard part. What makes a modern CAPTCHA effective is that it's wired into the same risk engine as every other layer.


reCAPTCHA v2, the "I'm not a robot" checkbox, is backed by a risk assessment that already weighed layers one through five before you clicked. A trusted user with good history passes with a single click; a suspicious one gets the image grid. reCAPTCHA v3 never shows a challenge at all. It runs in the background and returns a score from 0.0 (likely bot) to 1.0 (likely human), and the site decides what to do with it: gate the action, step up verification, or quietly serve a degraded response.


Cloudflare ships two things that are easy to conflate. Turnstile is an embeddable widget you drop into a form as a CAPTCHA replacement. The full-page "Checking your browser…" interstitial is a separate, edge-level Managed Challenge that runs a JavaScript challenge before letting you reach the site. Both probe the browser environment, but they're deployed in different places and solve different problems.


The rest of the field fills in the gaps. hCaptcha offers image-classification challenges for sites that want an alternative to Google's data collection. AWS WAF CAPTCHA shows up in enterprise stacks, and GeeTest's slider and spatial puzzles are built specifically to frustrate CAPTCHA-solving farms.


The builder's lesson: solving the puzzle isn't enough. If you've already accumulated bot signals upstream, the CAPTCHA service can keep rejecting correct answers or escalate to harder challenges. The CAPTCHA is a symptom of your score, not the gate itself.


## How do the layers combine into a risk score?


No single signal is decisive, so detection systems combine them. Each layer contributes weighted evidence to a risk score, and a decision engine maps that score to an action. That's the entire system in one line: many weak signals, one score, one action.


The major vendors operate as middleware that sees every request before it reaches your origin. Cloudflare Bot Management, Akamai Bot Manager, DataDome, and HUMAN Security (formerly PerimeterX) are the names you'll meet most often, with Kasada and Imperva in the same space. Underneath, they maintain real-time fingerprint databases, machine-learning models trained on labeled human and bot sessions, reputation data for IPs and fingerprints, and the decision engine that turns a score into a response.


That response ladder runs roughly: pass the request, run a soft challenge (an invisible JS check or a Turnstile-style widget), present a hard challenge (an image CAPTCHA), block outright (a 403 or 429), or serve degraded data. The last one is worth knowing about. Some systems return a 200 with empty or subtly wrong data instead of an obvious block. Hard blocks and challenges are still the norm, but a "successful" run that quietly produces garbage is a real failure mode, because your automation reports success while the data is junk.


The layers also interact rather than firing in sequence. A clean fingerprint can partly offset a suspicious IP. High network risk can lower the threshold for triggering a CAPTCHA. The model is holistic, which is why fixing one layer in isolation rarely moves the outcome.


## What this means if you're building automation


Reliable automation against protected sites means addressing the whole stack, not just the obvious parts. Most automations fail because they handle one or two layers, usually a User-Agent string and maybe a proxy, and leave the rest untouched.


The practical map looks like this. Residential or mobile proxies cover the network layer. A real Chromium build with source-level anti-detection covers the TLS, fingerprint, and framework layers. Headful rendering and human-shaped timing cover behavioral analysis. And a CAPTCHA strategy covers the challenge layer. Each of those is a standing engineering project, not a one-time fix, because every one of them drifts as browsers and detection vendors ship updates.


That drift is the honest reason this is hard to maintain in-house. The framework leaks are version-specific, the fingerprints have to stay internally consistent across hundreds of properties, and a single Chrome release or Playwright bump can silently reopen a gap you'd already closed.


At Intuned, we take a code-first approach to browser automation: what you deploy is TypeScript or Python on Playwright that you can read, debug, and own. The platform handles the layers above for you, with stealth, CAPTCHA solving, and proxies built in, so anti-detection isn't a side project you maintain forever. If you're running automations against protected sites, see[how Intuned handles stealth, CAPTCHA solving, and proxies](https://intunedhq.com/docs/main/02-features/stealth-mode-captcha-solving-proxies) .


## Frequently asked questions


### Why does my scraper or Playwright script keep getting blocked?


It's rarely one thing. A script that works locally and fails in production is usually clearing the IP and User-Agent checks but failing somewhere deeper: a TLS or HTTP/2 fingerprint that doesn't match its claimed browser, a framework leak like the CDP` Runtime.enable` side-channel, or behavioral signals from robotic timing. The fix is to address the whole stack, not to keep tweaking one layer.


### Can headless Chrome be detected?


Old headless Chrome, easily, through tells like an empty plugins array and a SwiftShader renderer. New headless mode (the default since around Chrome 132) closed most of those classic tells, so detection now leans on TLS and HTTP/2 fingerprints, CDP side effects, and behavioral analysis rather than simple headless flags.


### What is TLS, JA3, and JA4 fingerprinting?


It's a way to identify a client from how it negotiates a TLS connection. JA3 hashed the ClientHello fields with MD5, but Chrome's 2023 randomization of TLS extension order broke the raw hash. JA4, which sorts the order-variable fields before hashing, is the current standard and what most major vendors now use.


### Do residential proxies defeat bot detection?


They address the network layer only. A residential IP paired with a Node TLS fingerprint, a` navigator.webdriver` flag, or robotic mouse timing still gets caught at a later layer. Proxies are necessary against IP-based detection, but they aren't sufficient on their own.


### What's the difference between a WAF and an anti-bot solution?


A web application firewall inspects request payloads to block known attacks like SQL injection. An anti-bot system scores whether the client behind a request is human, using fingerprinting and behavioral signals. They overlap and are often deployed together, but they answer different questions.


### How is an AI agent different from a bot, to a detection system?


Technically they look alike, since both are automated. The shift in 2026 is that vendors increasingly try to distinguish sanctioned agents, declared through published IP ranges or signed identities, from unsanctioned automation, rather than blocking every non-human request the same way.
