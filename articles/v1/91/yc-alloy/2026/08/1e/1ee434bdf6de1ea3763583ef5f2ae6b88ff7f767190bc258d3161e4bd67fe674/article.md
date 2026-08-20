---
schema_version: "1.0.0"
document_id: "1ee434bdf6de1ea3763583ef5f2ae6b88ff7f767190bc258d3161e4bd67fe674"
company_key: "yc-alloy"
company: "Alloy"
source_id: "yc-alloy-news-import-baf7e6c723e9"
canonical_url: "https://alloy.app/library/how-to-clone-a-website"
published_at: "2026-08-06T00:00:00+00:00"
first_seen_at: "2026-08-12T14:28:24.001277+00:00"
fetched_at: "2026-08-12T14:28:24.832595+00:00"
content_hash: "sha256:b6c4ac67c36875d7502503d362046954f4a86937346c3298fff5933c4dc07e8d"
---

# How to Clone a Website: 4 Methods Compared (2026)

Cloning a website means making a working copy of it — the HTML, CSS, JavaScript, images, and fonts that make up what you see in the browser. People do it for good reasons all the time: archiving a site before a migration, studying how a well-built page works, keeping an offline copy of documentation, or capturing a product screen to prototype a redesign.


There are four practical ways to do it in 2026, and they produce very different results. This tutorial covers each one honestly — including where the classic tools break down on modern JavaScript-heavy sites — so you can pick the method that matches what you actually need the clone for.


**Key takeaways:**


- HTTrack and wget produce static offline mirrors — great for archiving, but they break on JavaScript-rendered apps and can't copy anything server-side.
- Manual methods (save-page, devtools) work for a single page when you just need its HTML and CSS.
- AI cloning tools capture the rendered page — including logged-in views — and give you an editable copy instead of a frozen snapshot.
- No method copies a backend. Databases, APIs, and server logic stay on the original server.
- Cloning for learning, backups, or prototyping is fine when you have the rights; republishing or impersonating a site is not.


## First, decide what kind of clone you need


The right method depends on one question: do you need a **frozen copy** or an **editable one** ?


- **Offline archive.** You want the site preserved as-is — for backup, offline reading, or reference. HTTrack or wget is the right tool.
- **One page's code.** You want to study or reuse a single page's HTML and CSS. Your browser can do this on its own.
- **An editable working copy.** You want to change the page — redesign it, prototype on it, A/B-test a variant. You need a tool that produces something you can modify, not a folder of frozen files.


## Method 1: HTTrack — the classic website copier


[HTTrack](https://www.httrack.com/) is a free, open-source website copier that has been the default answer to "how do I download a whole website" since the late 1990s. It crawls a site link by link and saves a browsable mirror to your disk, rewriting links so pages work locally.


On Windows, the WinHTTrack GUI walks you through it: create a project, paste the URL, click through the defaults. On macOS or Linux, install it with your package manager (` brew install httrack` or` apt install httrack` ) and run:


```text
httrack "https://example.com/" -O "./example-mirror" "+*.example.com/*" -v


```


That mirrors` example.com` into` ./example-mirror` , restricted to URLs on the same domain (the` +*.example.com/*` filter stops it wandering off into every external link). Open` index.html` in the mirror folder and you can browse the site offline.


**What HTTrack is genuinely good at:** archiving mostly-static sites — blogs, documentation, brochure sites, old sites you're about to migrate. It handles link rewriting well, resumes interrupted downloads, and its filters give you fine control over what gets crawled. It's also deliberately polite: transfer rates are throttled by default, so mirroring a large site can take hours. Leave those limits in place — they exist to avoid hammering the server.


**Where it breaks down:** modern web apps. HTTrack saves the HTML the server returns, but single-page apps built with React, Vue, or Angular ship a nearly empty HTML shell and render everything with JavaScript afterward. Mirror one and you'll get a blank page or a loading spinner. It also can't follow you behind a login, can't capture content that loads on scroll or click, and — like every method here — copies zero server-side code. Forms, search, and checkout in a mirror are decorative.


## Method 2: wget — the one-liner mirror


If you're comfortable in a terminal,[GNU Wget](https://www.gnu.org/software/wget/) does the same job as HTTrack with a single command and no project setup:


```text
wget --mirror --convert-links --adjust-extension --page-requisites --no-parent https://example.com/


```


Each flag earns its place:


- ` --mirror` — recursive download with infinite depth and timestamping, i.e. "take the whole site."
- ` --convert-links` — rewrite links so the local copy is browsable offline.
- ` --adjust-extension` — save pages with` .html` extensions so they open cleanly.
- ` --page-requisites` — grab the CSS, images, and scripts each page needs to display properly.
- ` --no-parent` — never crawl above the directory you started in.


Add` --wait=1 --random-wait` to space out requests on larger sites; wget also respects` robots.txt` by default, and you should let it.


**wget vs HTTrack:** wget is faster to invoke and already installed on most Linux and macOS systems (or` brew install wget` ); HTTrack gives you a GUI, resumable project files, and better filtering for messy crawls. For a quick mirror of a static site, wget wins on simplicity. Both share exactly the same blind spots: JavaScript-rendered content, authenticated pages, and anything server-side.


## Method 3: Manual — save-page, devtools, and raw HTML/CSS


When you only need one page, your browser is already a website cloner:


1. **Save Page As.** Press` Ctrl+S` /` Cmd+S` and choose **Webpage, Complete** . The browser saves the HTML plus a folder of assets. Quality varies — dynamically loaded content and some styles frequently go missing.
2. **A single-file extension.** Extensions like SingleFile bundle the fully rendered page — inlined styles, images and all — into one self-contained HTML file. For preserving exactly what you saw, this beats native save-page.
3. **Straight from devtools.** Open devtools, copy the root element's` outerHTML` from the Elements panel, and pull stylesheets from the Sources or Network tab. This is the most educational route: you see precisely how the page's[HTML, CSS, and JavaScript](https://developer.mozilla.org/en-US/docs/Learn_web_development) fit together, which is the whole point if you're cloning to learn.


The catch is the same as ever, plus one more: what you save is a snapshot of one rendered moment. Scripts that expect a server, live data, or[same-origin API access](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) will fail when reopened from disk, and maintaining a hand-saved page quickly becomes archaeology. It's a fine method for study and one-off references; it's a poor foundation for anything you plan to keep working on.


## Method 4: AI cloning — capture a live page and edit it


The newer approach doesn't crawl source files at all.[Alloy's website cloner](https://alloy.app/website-cloner) captures the **rendered page** — the real DOM, styles, and content as your browser displays them — and turns it into an editable copy. Because it clones what the browser actually rendered rather than what the server initially sent, the JavaScript-app problem that defeats HTTrack and wget doesn't apply.


The workflow:


1. **Capture.** Paste a public URL at[alloy.app/website-cloner](https://alloy.app/website-cloner) , or use the[Chrome extension](https://alloy.app/guide/browser-extension) for any page you can see in your browser — including logged-in dashboards and internal tools that no crawler can reach.
2. **Edit.** The capture opens as a pixel-perfect, editable version of the page. Change copy, colors, layout, or whole components by selecting elements or describing the change in AI chat — no need to reverse-engineer someone else's CSS first.
3. **Share or export.** Send the result as an interactive link, or export the underlying HTML and CSS to use in your own project.


This is the method built for the *editable copy* use case: redesigning a page of your own product, prototyping a change on a live site, or turning a captured screen into something stakeholders can click through. Our guide to[prototyping changes on an existing web app](https://alloy.app/library/how-to-prototype-changes-existing-web-app) walks through that workflow end to end. Capturing is free, and it takes seconds rather than a crawl's hours — though for archiving an entire hundred-page site in one pass, a crawler is still the better fit: captures work page by page.


## Which method should you use?


Your goal Best method Why


Archive a whole static site HTTrack or wget Crawls every page, rewrites links, browsable offline


Quick mirror from the terminal wget One command, pre-installed almost everywhere


Study one page's HTML/CSS Save-page / devtools Free, instant, educational


Clone a JavaScript-heavy app Alloy Captures the rendered page, not the empty shell


Clone a page behind a login Alloy Chrome extension Captures what your authenticated browser sees


Edit or redesign the clone Alloy Produces an editable prototype, not frozen files


## A short word on legality and ethics


Cloning technology is neutral; what you do with the clone isn't.


**Generally fine:** backing up or migrating a site you own; keeping an offline copy for personal reference; studying a page's code to learn; capturing a screen of your own product — or a competitor's public page — to prototype and discuss internally. These are the learning-and-prototyping uses this tutorial is written for.


**Not fine:** republishing someone else's site or design as your own — the content, code, and artwork remain copyrighted even though your browser can download them. And cloning a site to impersonate it — fake login pages, lookalike checkout flows — is phishing, full stop. It's criminal in most jurisdictions, and organizations like[OWASP document](https://owasp.org/www-community/attacks/) how these impersonation attacks work precisely so they can be detected and shut down.


In between, use judgment: respect` robots.txt` and a site's terms of service when crawling, keep clones of third-party sites private, and when in doubt, ask the site owner. If you're cloning your own product to iterate on it, none of this slows you down — which is, in practice, what most website cloning is for.
