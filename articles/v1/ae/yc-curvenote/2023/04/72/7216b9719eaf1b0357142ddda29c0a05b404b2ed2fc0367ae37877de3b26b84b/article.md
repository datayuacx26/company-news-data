---
schema_version: "1.0.0"
document_id: "7216b9719eaf1b0357142ddda29c0a05b404b2ed2fc0367ae37877de3b26b84b"
company_key: "yc-curvenote"
company: "Curvenote"
source_id: "yc-curvenote-news-import-2221551dc8c3"
canonical_url: "https://curvenote.com/blog/mike-morrison-joins-curvenote"
published_at: "2023-04-12T00:00:00+00:00"
first_seen_at: "2026-07-23T06:57:34.135292+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:cf813429bbd780210d8c86050c2888e4571ee495c426af35a16f635e5b9ea7e4"
---

# Better scientific articles — and beyond

# Better scientific articles — and beyond


## Why I joined Curvenote


Mike Morrison · April 12, 2023


Press Release


Read the press release on[Curvenote Hires Mike Morrison to Redesign Science Communication](https://curvenote.com/blog./curvenote-hires-morrison-creator-of-betterposter.md) .


When[my redesign of the scientific poster went viral](http://npr.org/sections/health-shots/2019/06/11/729314248/to-save-the-science-poster-researchers-want-to-kill-it-and-start-over) ([#betterposter](https://twitter.com/search?q=%23betterposter&src=typed_query&f=top) ), people asked me how we could apply the same design principles to improve scientific articles. The answer is easy to explain, but hard to implement: make articles more machine-readable than HTML *first* , and then you can wrap whatever interfaces you want on them — create a *universe* of designs for different purposes 🎨.


Now,[joining Curvenote](https://curvenote.com/blog/curvenote-hires-morrison-creator-of-betterposter)


as full-time UX lead 💼, I’m super excited that I get to help actually implement this vision, and help improve the user experience of scientific articles, peer review, and scientific publishing.


## Part 1: The dream


¶


### A scientific article should adapt to your current information need


¶


Figure 2: And this assumes you’re stuck in a single paper.


New to the topic? The article should give you a deeper introduction. Working in a company? You should be able to find the ‘practical implications’ section faster. Doing a replication study? The article’s methods should be presented as a step-by-step.


### 💎 It’s not about articles — it’s about that piece of insight within an article that you actually want


¶


Scientists don’t always skim read articles start-to-finish 👁️. They are often interested in a particular tiny piece of the study 💎. How did they measure X? What does the effect of X on Y look like over time?


Figure 3: A single article is a bundle of individual insights. Readers may just want one particular insight in the bundle.


The goal of scientific publishing is to get the scientist to the particular piece of information 💎 they’re looking for *in less than 400ms* ⏱️ (called the[Doherty Threshold](https://lawsofux.com/doherty-threshold/) ). Scrolling through a bunch of 40 page PDF files 📄 to find the two sentences that answer your question is usually slower than 400ms.


### 🧱 You want to extract chunks of information across articles


¶


Science is published in a single article, but scientific insight is discovered by looking *across* articles. If you’re trying to understand how to measure a concept, you’re real question isn’t “How did they measure X in *this* study?” Your real question is “How did they measure X in *all* studies?” To extract and display that information easily, applications need those individual chunks 🧱 of info marked up in a way that’s easy for machines to read.


### ♾️ A million creative ways to explore science


¶


Figure 4: A network visualization of Asthma’s symptoms and risk factors


If you can get scientific articles published in a format where every important piece of the article is a machine-readable chunk, you can do *anything* design-wise. Every cool way you’ve seen science displayed becomes possible to apply to *every* article. A few examples:


-


🪄 Switch an article’s entire layout in seconds


-


🕳️🐇 Click[rabbit-hole](https://mystmd.org/guide/quickstart-myst-markdown#links-cross-references) links where you can dive into cited studies without leaving the page


-


*Generate* population-specific effect summaries


-


And of course everything HTML can do: Videos,[executable code](https://mystmd.org/guide/interactive-notebooks) ,[interactive charts](https://twitter.com/curvenote/status/1641405182800982016) , Google-ability.


> …any summary layout that would help you do your science, you can build.


*If* we can get scientific articles into a chunked, machine-readable format.


## Part 2: Making this happen is hard, for two reasons


¶


Figure 5: Word and PDF files are less machine readable than HTML; we need a format for science that is MORE machine-readable.


Right now, with next-generation science tools like[Elicit.org](http://elicit.org/) ,[Iris.ai](https://iris.ai/) , and[Scholarcy](https://www.scholarcy.com/) , you’re seeing the best that 🤖 AI algorithms like GPT can do to extract scientific information from PDF files that aren’t designed for machines. These tools would become at least ten-zillion times more helpful if the format of scientific articles wasn’t actively fighting their efforts.


### 🤖 We need a common markup language for scientific articles


¶


Figure 6: Clearly marking up the abstract paragraph in a machine-readable way makes Google Scholar Bot puke rainbows.


To get scientific articles converted into machine-readable chunks, we need a common markup language (more specific to science than HTML) for the robots to read.[JATS4R](https://jats4r.org/) is close to this, right now converting your Word doc to JATS4R costs them thousands of 💵, which they have to pass along in subscription fees and APC charges.


We want you — the paper author 👨🔬 — to be able to write your articles in a machine-readable format first, because then everything after your draft (review, publishing) gets way, way more efficient and cheaper.


### 📝 We need a WYSIWYG text editor so you can write papers in machine-readable markup


¶


Figure 7: A typical WYSIWYG text editor that actually produces HTML.


When you use Webflow or Wordpress, you’re composing a document in HTML without code. Having the perfect markup language for scientific articles isn’t enough if nobody can use it besides publishers. We need a scientific authoring tool that feels like that Word but writes the scientific markup. And that lets you, the author, mark up things like your author affiliation so you don’t have to type them over and over again.


## Part 3: Good news — it’s done


¶


### 🤖 Meet MyST, the open-source markup language for scientific articles


¶


Figure 8: MyST - An open-source markup language for scientific articles that’s enabling a new wave of scientific publishing innovations. See the[MyST Documentation](https://mystmd.org/guide) .


The open-source MyST Markdown language ([see #WhatIsMyST on Twitter](https://twitter.com/search?q=%23WhatIsMyST&src=typed_query&f=top) ), which Curvenote donates a lot of development time to, is easier to author in, designed for scientific articles, and (crucially) converts instantly between all the other scientific document formats (JATS4R, HTML, Word). And it has incredible features for scientists, like you can[copy-paste a DOI URL](https://twitter.com/mystmarkdown/status/1639235796145426432) and auto-import a reference.


### 📝 Meet Curvenote, the WYSIWYG text editor you can use to write machine-readable scientific articles


¶


Figure 9: Curvenote: Feels like word, exports machine-readable MyST Markdown for better scientific articles.


The Curvenote WYSIWYG editor lets you write in an MS-Word like interface with features just for scientists, that can be exported as open-source MyST Markdown, JATS, LaTeX\\LaTeX


L


A


T


E


​


X


, PDF, Word — whatever you want. It even lets you compose scientific articles in reusable chunks.


With these two tools alone as the core, we can start creating all-new interfaces for scientific publishing.


### 👨💻 Curvenote has the team to pull this off


¶


When I first met[the Curvenote team](https://curvenote.com/mission) , my first impression was that each of them was a visionary in their own right, and exactly team I always pictured science needing to solve this problem. I’m so excited I get to join this team and take a shot at solving the hardest design problems in science.


## 🔮 Better articles are only the beginning


¶


Since Curvenote has already built the core (a markup language and editor), we’re now focused on the million amazing innovations you can make when articles start life as machine-readable (vs PDFs). We’re already working on concepts for formless article submission systems (no more filling-out 24 author names), crazy-streamlined peer review, and links between scientific articles that are more useful even than standard web links.


But we’ll need all of your help to get the designs right, so please[HMU on Bluesky](https://bsky.app/profile/mikemorrison.bsky.social) and let me know about all your hopes, dreams, and frustrations for scientific publishing and distribution. **My job is to make your job easier.** 😊


## Related Articles


- [Curvenote Debuts Platform for Connected Publishing to Move Science Beyond the PDF Curvenote launches its Scientific Content Management System (SCMS) to transform fragmented research into connected, reusable components. Learn how this platform helps researchers collaborate faster, preserve credit, and publish interactively across labs and networks. news update scms](https://curvenote.com/news/curvenote-debuts-platform-for-connected-publishing)
- [SCMS: The next evolution of Curvenote Discover Curvenote's Scientific Content Management System (SCMS) that transforms research into connected, reusable components. Learn how SCMS keeps source history, metadata, and credit intact from notebooks through publication, enabling faster collaboration and connected publishing. scms update brand](https://curvenote.com/blog/curvenote-launches-scms)
- [From PDFs to Possibilities Welcoming product visionary Jillian Hale as we build a modular, dynamic future for research. Jillian’s background spans product strategy, communication, and platform design. She brings a unique blend of user-centered thinking and a deep respect for how scientific work gets done. update team](https://curvenote.com/blog/jillian-hale-joins-curvenote)
- [Curvenote Hires Mike Morrison to Redesign Science Communication Curvenote hires Mike Morrison as their newest team member focused on user experience research. Mike is a well-known science communication advocate with a background in UX design and a PhD in psychology. news company team](https://curvenote.com/news/curvenote-hires-morrison-creator-of-betterposter)
- [Curvenote Microsoft Word Export Exporting to Microsoft Word is now a single click in Curvenote! update editor](https://curvenote.com/blog/exporting-to-docx)
- [Introducing Curvenote The Curvenote brand embodies aspects of reuse, modularity and the connections behind ideas through building blocks that provide structure and can also be pulled apart, rearranged and used in unique ways. update editor](https://curvenote.com/blog/introducing-curvenote)
