---
schema_version: "1.0.0"
document_id: "b769be99d96ad31f637288d26bdf2105eaf2decad15b15df2d52b0ed0e9157da"
company_key: "gen-digital-inc-common-stock"
company: "Gen Digital Inc."
source_id: "gen-digital-inc-common-stock-rss-b2c253ea0afd"
canonical_url: "https://www.gendigital.com/blog/insights/research/fake-hiring-pages-abuse-fifa"
published_at: "2026-06-11T18:19:51+00:00"
first_seen_at: "2026-07-20T03:32:39.128259+00:00"
fetched_at: "2026-07-28T21:11:25.860154+00:00"
content_hash: "sha256:9925972f90f1c34181baba083b57d84455d89741abc8b3935869e4160896d91f"
---

# Fake hiring pages abuse FIFA and other major brands to steal work credentials

By[Luis Corrons](https://www.gendigital.com/blog/authors/luis-corrons)


A fake job interview page is easier to trust than a fake ticket shop.


In samples reviewed by Gen threat researchers, scammers used branded hiring pages that looked like ordinary recruitment flows: a company logo, a recruiter profile, a 30-minute meeting, and a button to continue with Google or Facebook. There was no attachment to open and no software to install. The credential theft attempt sat inside a step many job applicants already expect: signing in to schedule a call.


FIFA is an obvious lure ahead of the 2026 World Cup. On May 22, ESET reported fake FIFA-themed websites that copied ticket and merchandise flows to steal money and personal data. Our researchers later found another FIFA-themed domain, hxxps://fifa-mg\[.\]shop, which followed a naming pattern not mentioned in that report. Domains like these can be short-lived, often appearing for hours or days before they are blocked, abandoned, or taken down.


Our researchers also found a different kind of FIFA lure: hxxps://fifahiring\[.\]com, a fake hiring page that encouraged users to schedule a call with an alleged FIFA recruiter. When a user tried to register with a personal Gmail address, the page asked for a business email instead. The page was steering victims toward credentials with more value: accounts that could provide access to a company environment.


#### The FIFA hiring page did not point to FIFA


The fake page looked believable at first glance.


It used FIFA branding, presented a recruiter identity, and copied parts of the experience users see on legitimate recruiting and scheduling platforms. The recruiter’s name and photo appeared to match a real LinkedIn profile, which made the page feel more credible. For publication, we should keep that person’s identity redacted.


The domain did not match FIFA’s real hiring infrastructure. FIFA’s legitimate careers pages are jobs.fifa.com and fifa.pinpointhq.com. The fake page used neither. It also loaded the favicon from the real FIFA recruitment site hosted at pinpointhq.com, giving the page a small visual cue borrowed from the legitimate service.


Other signals pointed in the same direction. The domain and certificate were newly created, from May 23. The site used commodity hosting infrastructure, including Vercel and Render, rather than a corporate environment. Those signs are not enough on their own, but they fit the rest of the evidence.


The login flow was the clearest part. Credentials were posted to:


fifeq2026eqbackeq\[.\]onrender\[.\]com/api/login


The Google sign-in prompt was not a real Google authentication window. It was rendered inside the page itself. The visible controls and links did nothing, and the page did not send the user into a genuine Google sign-in flow. According to the analysis, the only Google asset loaded was the favicon, apparently to make the fake window look familiar.


A page can look like it opened a Google login without ever handing control to Google.


Researchers also observed hxxps://careers-fifahiring\[.\]com, which later became inactive, and hxxps://fifajobs\[.\]com, which showed the same content and was indexed by Google.


#### The recruitment lure went beyond FIFA


The FIFA page was one version of a wider recruitment phishing pattern. The brand can change. The flow stays familiar: recruiter profile, meeting prompt, business email, fake sign-in.


Researchers found similar fake hiring pages abusing other major brands, including **Heineken, Hilton, Coca-Cola, Netflix, PepsiCo, Delta,** and **Spotify** . In the Heineken case, the page appeared to use the identity of someone who, according to LinkedIn, really works as a recruiter at the company.


Additional hunting found two other recruitment-themed kits:


hxxps://aquent-talent-jobs\[.\]com
hxxps://hays-talent-opportunities\[.\]com


The Hays-themed version targeted Facebook credentials instead of Google credentials. Researchers assessed that the Aquent and Hays pages shared code, although they differed from the earlier FIFA examples.


This flexibility makes the campaign harder to frame as a single World Cup scam. FIFA provides the timely hook, but the mechanism is recruitment phishing. A scammer can swap the logo, recruiter name, and sign-in provider while keeping the same basic path to the victim’s credentials.


#### Why fake recruitment pages work


Recruitment gives attackers a clean excuse to ask for things that might otherwise raise suspicion.


A candidate expects to click a scheduling link. They expect a recruiter profile. They expect a calendar prompt. In some cases, they may also expect to use a work email, especially for corporate, contractor, or partner-facing roles.


The fake login window turns that ordinary flow into credential theft. Users are used to “Continue with Google” and “Continue with Facebook” buttons, but the button itself proves nothing. A genuine authentication flow should take the user to the provider’s domain and controls. A fake page can draw a convincing copy while keeping the victim inside the attacker-controlled site.


The business email prompt is also a warning sign. A page that rejects a personal email and asks for a work address may be trying to raise the value of the stolen account.


#### What job seekers should check before signing in


Start from the company’s official careers page, not from a link in a message, ad, search result, or forwarded post. For FIFA, that means checking jobs.fifa.com or fifa.pinpointhq.com, not trusting a domain because it contains “fifa,” “career,” “hiring,” or “jobs.”


Look closely at the domain before entering credentials. Scam pages often add plausible hiring words around a brand name: career, talent, jobs, hiring, portal, or connect. A polished page on a new domain is still a new domain.


Treat the sign-in step as the real checkpoint. If a page offers Google or Facebook login, check whether the flow actually opens a real provider-controlled authentication page. A fake window embedded inside the original site, broken controls, inactive links, or a provider logo used only as decoration should stop the process.


Use security software that includes web and phishing protection. These tools can block known malicious pages before they load, which is especially useful when scam domains look polished, appear in search results, or are shared through otherwise legitimate channels.


#### What companies can do


Recruitment pages are public by design, which makes them easy to copy.


Security teams should monitor for newly registered domains combining their brand with hiring-related terms. They should also watch for pages that reuse favicons, recruiter photos, job descriptions, and calendar-booking language from legitimate hiring flows.


HR and recruiting teams should make the official hiring path easy to verify. Candidates should not have to guess whether a domain is legitimate. A clear careers page, consistent recruiter communication, and warnings about unofficial scheduling links can remove some of the ambiguity attackers rely on.


#### Conclusion


The fake FIFA hiring page worked because it did not feel unusual. It looked like a normal step in a hiring process.


Attackers do not need to invent new behavior when they can copy one people already trust. A branded scheduling page, a recruiter profile, and a familiar sign-in button can be enough to move a victim from interest to credential entry.


The decision point is the login. Before entering a work email or password, check the domain, check where the authentication flow really goes, and verify the role through the company’s official careers site. Security software with web and phishing protection adds another layer, especially against short-lived scam pages that may only stay online for hours or days.


**IOCs:**


hxxps://fifahiring\[.\]com
hxxps://careers-fifahiring\[.\]com
hxxps://fifajobs\[.\]com
hxxps://fifa-mg\[.\]shop
hxxps://heineken-careers\[.\]com
hxxps://aquent-talent-jobs\[.\]com
hxxps://hays-talent-opportunities\[.\]com
fifeq2026eqbackeq\[.\]onrender\[.\]com/api/login


More on this topic


[Fake invoices are moving from inboxes to shopping apps – From Research](https://www.gendigital.com/blog/insights/research/fake-invoices-shopping-apps)


[Inside Vidar’s ABE Bypass: From Memory Scanning to APC Injections – From Research](https://www.gendigital.com/blog/insights/research/inside-vidar-abe-bypass)


[Your flight was cancelled. Is the refund message real? – From Research](https://www.gendigital.com/blog/insights/research/flight-cancelled-refund-message-scam)


[Luis Corrons](https://www.gendigital.com/blog/authors/luis-corrons)


Security Evangelist at Gen


At Gen, Luis tracks evolving threats and trends, turning research into actionable safety advice. He has worked in cybersecurity since 1999. He chairs the AMTSO Board and serves on the Board of MUTE.


Follow us for more
