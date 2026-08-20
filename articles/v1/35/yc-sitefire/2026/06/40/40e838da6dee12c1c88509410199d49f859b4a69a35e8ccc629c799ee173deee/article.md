---
schema_version: "1.0.0"
document_id: "40e838da6dee12c1c88509410199d49f859b4a69a35e8ccc629c799ee173deee"
company_key: "yc-sitefire"
company: "sitefire"
source_id: "yc-sitefire-news-import-97a7181f264f"
canonical_url: "https://sitefire.ai/blog/google-ai-overviews-liability-munich-ruling"
published_at: "2026-06-10T22:00:00+00:00"
first_seen_at: "2026-07-24T00:58:46.205658+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:6250c32a79728c4c71fc39c6c9e4e7c80cdb181b7cd8699c991107da70877211"
---

# Google Is Liable for What Its AI Overviews Say: Inside the Munich Ruling

On May 28, 2026, the Regional Court of Munich I ordered Google to stop spreading false claims its AI Overviews had generated about two German publishers ([case no. 26 O 869/26](https://the-decoder.com/wp-content/uploads/2026/06/26_O_869_26_begl_Abschrift_Urteil_v_28_05_2026_Geschwarzt_Geschwarzt_Geschwarzt.pdf) ). It is the first court decision to hold Google directly liable for what its AI search feature says ([JUVE](https://www.juve.de/verfahren/punktsieg-fuer-lausen-google-haftet-fuer-falschaussagen-in-ki-uebersichten/) ).


We read the full 35-page judgment. The interesting part is not the order but the reasoning: it answers a question the industry has been dodging - when an AI writes something false about you, who is legally speaking?


## What Happened?


A German court banned Google from repeating eight false claims its AI Overview made about two publishers, under threat of fines up to EUR 250,000 per violation.


It started with a search suggestion. When users typed one publisher's name, Google's autocomplete suggested adding "Betrugsmasche" - scam. That suggested search produced an AI Overview opening with a confident "Yes, \[company\] is known for dubious business practices," followed by red flags and protective advice.


Here is the detail that decided the case: **none of the linked sources made these claims** . The AI had mixed the publishers up with genuinely shady companies and asserted connections that appeared nowhere. Its top cited source never even mentioned the publishers.


The publishers reported the overview through Google's complaint form, then sent a formal cease-and-desist letter. When nothing adequate happened, they obtained a preliminary injunction (einstweilige Verfügung), a fast-track court order - under four months from first notice. Google also pays 80% of the costs.


## Why Were Search Engines Never Liable for This Before?


Because a search engine only points to other people's content. The law treats it as an intermediary, like a librarian, not as the speaker.


German law distinguishes two roles. The **direct interferer** (unmittelbarer Störer) made the statement and answers for it fully. The **indirect interferer** (mittelbarer Störer) merely helped distribute someone else's statement and must act only after notice of an obvious violation.


Germany's Federal Court of Justice[placed search engines in the second category in 2018](https://dejure.org/dienste/vernetzung/rechtsprechung?Gericht=BGH&Datum=27.02.2018&Aktenzeichen=VI%20ZR%20489%2F16) : nobody can pre-check billions of pages, and without search the internet is unusable. The librarian shows you where the book is. If the book lies, you sue the author, not the library.


## So Why Is Google Suddenly the Speaker?


Because an AI Overview does not point at content. It writes new content, and Google alone controls the system that writes it.


The legal test is whether an operator has "adopted content as its own" (sich zu eigen machen). The court found an AI Overview crosses that line, for four reasons drawn from how the feature works:


1. It summarizes "in its own words and according to its own structure," opening with an affirming "Yes" rather than a list of links.
2. It invents its own editorial layout - summary, red flags, advice - that exists in none of the sources.
3. It makes claims no source made: "independent, new, and substantive statements,"[in the court's words](https://the-decoder.com/wp-content/uploads/2026/06/26_O_869_26_begl_Abschrift_Urteil_v_28_05_2026_Geschwarzt_Geschwarzt_Geschwarzt.pdf) .
4. Only Google "has influence over the AI's offering and the algorithms with which the AI operates." Whoever controls the system owns its output.


The policy argument collapses too. A search index is essential infrastructure; an AI Overview is "by no means absolutely necessary" for using the internet. And Google can check its own summaries against the sources they cite - it just did not, even after notice.


Role What it does Who answers for falsehoods


Search engine (librarian) Points to third-party pages The page authors


AI Overview (publisher) Writes its own self-contained answer **The operator**


A subtlety for every AI builder: some claims loosely traced back to anonymous user reviews. No help - the AI had reformulated them into assertions found verbatim nowhere. Paraphrase and recombination make the output your own statement.


## "Users Know AI Makes Mistakes" - Why Did That Defense Fail?


The law holds a speaker liable for self-contained statements even when readers could, in theory, verify them somewhere else.


Google argued users can click the cited sources and know not to trust AI blindly. German courts have long rejected that logic: a newspaper is liable for a misleading front-page teaser even when the story inside is accurate, because most people read only the front page - the headline-reader doctrine.


The court applied it one-to-one. An AI Overview reads like a finished answer with no signal of unreliability, so users have no reason to double-check. The data agrees:[Pew Research](https://www.pewresearch.org/short-reads/2025/07/22/google-users-are-less-likely-to-click-on-links-when-an-ai-summary-appears-in-the-results/) found only **1% of users** click a link inside an AI summary. The court added, dryly, that treating AI Overviews as unreliable would erase the usefulness Google advertises.


Two more defenses fell fast. Free speech protects opinions, but an AI's opinion is "not the expression of an acquired conviction, but the result of an algorithm," and opinions built on false facts lose protection anyway. "We updated the model" failed too: algorithms can regenerate claims at any time, so the law accepts only a penalty-backed cease-and-desist declaration (strafbewehrte Unterlassungserklärung). Google gave none.


## What the Ruling Does Not Do


It is not the final word, and it did not ban AI Overviews.


This is a first-instance decision in fast-track proceedings. Google can appeal, and a final answer from Germany's highest civil court would require separate main proceedings, years away. The order covers only these statements, but is not limited to Germany:[EU rules](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32012R1215) recognize such judgments across member states.


The court was also visibly careful: it rejected two requests, including one aimed at a claim the AI never made but readers might infer. And the world is not in line yet: a Georgia court dismissed a similar case against OpenAI in 2025 ([Walters v. OpenAI](https://reason.com/wp-content/uploads/2025/05/WaltersvOpenAISJOrder.pdf) ) because ChatGPT's error disclaimers meant no reasonable reader would take the output as fact - precisely the argument Munich rejected.


## Why This Matters Beyond One Case


German courts are converging on a single principle: whoever runs the model answers for what it says.


A[Kiel court said the same in 2024](https://www.gesetze-rechtsprechung.sh.juris.de/bssh/document/NJRE001589976) about an AI data portal that falsely reported a company as insolvent; the same Munich court held OpenAI responsible for its model's outputs in a[2025 copyright case](https://www.gesetze-bayern.de/Content/Document/Y-300-Z-GRURRS-B-2025-N-30204) . This ruling extends the line to the internet's biggest information surface.


The scale makes it consequential. An[analysis commissioned by the New York Times](https://www.nytimes.com/2026/04/07/technology/google-ai-overviews-accuracy.html) found AI Overviews answer correctly about 91% of the time, yet[56% of even the correct answers could not be traced to their cited sources](https://oumi.ai/blog/oumis-study-finds-50-of-ai-overviews) (Google disputes the methodology). With only 1% checking sources, the AI's answer is, for most people, the final word.


The so-what for any company: what an AI engine asserts about you is an authored statement - sometimes wrong, rarely verified, legally owned by the platform. If it is false, the Munich playbook works: document it, report it through the operator's form, send a cease-and-desist letter. Courts now treat silence as the operator's problem, not yours.


## Frequently Asked Questions


### Is the Munich ruling against Google final?


No. It is a first-instance judgment in preliminary-injunction proceedings, and Google can appeal to the Higher Regional Court of Munich. A supreme-court answer would require separate main proceedings. The injunction is enforceable immediately, with fines up to EUR 250,000 per violation.


### Does the ruling affect ChatGPT, Perplexity, and other AI tools?


Not directly, but the reasoning is not Google-specific. Any operator that generates self-contained answers by combining web content fits the court's definition of a direct interferer. German courts applied the same logic to an AI data portal in 2024 and to OpenAI's outputs in a 2025 copyright case.


### What can a company do about false AI-generated claims?


Document the output with screenshots, report it through the operator's reporting form, and follow up with a formal cease-and-desist letter. If the operator does not act, German law now supports a fast-track injunction. In Munich, the whole process took under four months.


## Key Takeaways


- The Regional Court of Munich I held Google directly liable for false AI Overview statements - a first (case no. 26 O 869/26).
- An AI Overview is Google's own statement, not a search result: it writes new claims, and only Google controls the algorithms.
- The search-engine privilege covers pointing at content, not systems that speak for themselves.
- "Users can check the sources" failed: only 1% ever do, and self-contained statements create liability regardless.
- The ruling is appealable, but German case law is converging: the operator answers for what its model says. US courts so far lean the other way.


## The Bottom Line


For twenty years the deal was simple: search engines point at the web and are not responsible for what the web says. Munich has ended that deal for generative answers. The moment a system stops indexing content and starts asserting things about it, it stops being a librarian and becomes a publisher, with a publisher's accountability.


AI answers are authored statements: sometimes wrong, rarely checked, and now legally owned by whoever operates the machine that writes them.


## Sources


### Primary Sources


- Landgericht München I. "Endurteil vom 28.05.2026, Az. 26 O 869/26" (certified copy). 2026.[https://the-decoder.com/wp-content/uploads/2026/06/26_O_869_26_begl_Abschrift_Urteil_v_28_05_2026_Geschwarzt_Geschwarzt_Geschwarzt.pdf](https://the-decoder.com/wp-content/uploads/2026/06/26_O_869_26_begl_Abschrift_Urteil_v_28_05_2026_Geschwarzt_Geschwarzt_Geschwarzt.pdf)
- Bundesgerichtshof. "Urteil vom 27.02.2018, Az. VI ZR 489/16 (search-engine liability)." 2018.[https://dejure.org/dienste/vernetzung/rechtsprechung?Gericht=BGH&Datum=27.02.2018&Aktenzeichen=VI%20ZR%20489%2F16](https://dejure.org/dienste/vernetzung/rechtsprechung?Gericht=BGH&Datum=27.02.2018&Aktenzeichen=VI%20ZR%20489%2F16)
- Landgericht Kiel. "Urteil vom 29.02.2024, Az. 6 O 151/23." 2024.[https://www.gesetze-rechtsprechung.sh.juris.de/bssh/document/NJRE001589976](https://www.gesetze-rechtsprechung.sh.juris.de/bssh/document/NJRE001589976)
- Landgericht München I. "Endurteil vom 11.11.2025, Az. 42 O 14139/24 (GEMA ./. OpenAI)." 2025.[https://www.gesetze-bayern.de/Content/Document/Y-300-Z-GRURRS-B-2025-N-30204](https://www.gesetze-bayern.de/Content/Document/Y-300-Z-GRURRS-B-2025-N-30204)
- Superior Court of Gwinnett County, Georgia. "Walters v. OpenAI, No. 23-A-04860-2, Order on Summary Judgment." 2025.[https://reason.com/wp-content/uploads/2025/05/WaltersvOpenAISJOrder.pdf](https://reason.com/wp-content/uploads/2025/05/WaltersvOpenAISJOrder.pdf)
- European Union. "Regulation (EU) No 1215/2012 (Brussels Ia), Art. 36."[https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32012R1215](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32012R1215)


### Secondary Sources and Databases


- heise online. "LG Munich I: Google ordered to pay for false statements in AI summaries." 2026.[https://www.heise.de/en/news/LG-Munich-I-Google-ordered-to-pay-for-false-statements-in-AI-summaries-11327217.html](https://www.heise.de/en/news/LG-Munich-I-Google-ordered-to-pay-for-false-statements-in-AI-summaries-11327217.html)
- JUVE. "Punktsieg für Lausen: Google haftet für Falschaussagen in KI-Übersichten." 2026.[https://www.juve.de/verfahren/punktsieg-fuer-lausen-google-haftet-fuer-falschaussagen-in-ki-uebersichten/](https://www.juve.de/verfahren/punktsieg-fuer-lausen-google-haftet-fuer-falschaussagen-in-ki-uebersichten/)
- The New York Times. "How Accurate Are Google's A.I. Overviews?" 2026.[https://www.nytimes.com/2026/04/07/technology/google-ai-overviews-accuracy.html](https://www.nytimes.com/2026/04/07/technology/google-ai-overviews-accuracy.html)
- Oumi. "Oumi's Study Finds 50% of AI Overviews Untrustworthy." 2026.[https://oumi.ai/blog/oumis-study-finds-50-of-ai-overviews](https://oumi.ai/blog/oumis-study-finds-50-of-ai-overviews)
- Pew Research Center. "Google users are less likely to click on links when an AI summary appears in the results." 2025.[https://www.pewresearch.org/short-reads/2025/07/22/google-users-are-less-likely-to-click-on-links-when-an-ai-summary-appears-in-the-results/](https://www.pewresearch.org/short-reads/2025/07/22/google-users-are-less-likely-to-click-on-links-when-an-ai-summary-appears-in-the-results/)
