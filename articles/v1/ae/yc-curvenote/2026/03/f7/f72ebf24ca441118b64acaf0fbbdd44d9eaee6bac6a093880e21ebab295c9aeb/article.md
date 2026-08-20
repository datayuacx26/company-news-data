---
schema_version: "1.0.0"
document_id: "f72ebf24ca441118b64acaf0fbbdd44d9eaee6bac6a093880e21ebab295c9aeb"
company_key: "yc-curvenote"
company: "Curvenote"
source_id: "yc-curvenote-news-import-2221551dc8c3"
canonical_url: "https://curvenote.com/blog/data-engineering-podcast"
published_at: "2026-03-13T00:00:00+00:00"
first_seen_at: "2026-07-23T06:57:34.135292+00:00"
fetched_at: "2026-07-28T21:26:25.193690+00:00"
content_hash: "sha256:b892cb4908405cce1d61289c31eb73f260f86c3497c40e0e7a2a4603649d7398"
---

# Curvenote on the Data Engineering Podcast

# Curvenote on the Data Engineering Podcast


Rowan Cockett · March 13, 2026


I recently joined the Data Engineering Podcast to talk about Curvenote, open science, and why the way we share research needs to change. The conversation covered a lot of ground — from my background in geoscience visualization to the socio-technical challenges of making science more reproducible and reusable.


Listen to the full episode


🎙️[Listen to the full episode on the Data Engineering Podcast →](https://www.dataengineeringpodcast.com/episodepage/beyond-the-pdf-rowan-cockett-on-reproducible-composable-science)


## From Geoscience to Scientific Publishing


¶


My path to Curvenote started in geoscience. As an undergrad studying geology, I built 3D visualization tools for my peers that ended up going viral in the community. That led to a startup, an acquisition by a data science company working in mining and civil engineering, and eventually a PhD where I experienced firsthand the mismatch between how research is *done* and how it’s *shared* . I was building interactive widgets, reproducible workflows, and rich data explorations — and then flattening it all into screenshots for a PDF.


## The Core Problem


¶


Science has outgrown the paper. Twenty years ago, data could fit in a table on a page. Today we’re dealing with terabytes of data and complex processing pipelines, but the communication layer hasn’t kept up. Researchers still throw zip files on Zenodo without context, disconnect their data from their narrative, and take screenshots of interactive visualizations to paste into static documents.


What’s needed is an integration layer that brings data, code, and narrative together — the kind of literate, computational storytelling that tools like Jupyter notebooks enable, but carried all the way through to publication.


## Solving Both the Social and Technical Problems


¶


One of the things I talked about on the podcast is that this isn’t purely a technical challenge. There are deeply entrenched social norms around how science is shared, how credit is assigned, and how careers are built. That’s one of the reasons I helped start the[Continuous Science Foundation](https://continuous.science/) — to convene working groups tackling incentive structures, licensing, and attribution.


For example, the Journal of Open Source Software (JOSS) was created because researchers were spending years on critical scientific software but couldn’t get academic credit for it. We need similar social experiments for computational narratives, visualization widgets, and modular research outputs.


## Graceful Degradation and Long-Term Access


¶


A question that came up in the conversation was about maintainability — environments degrade, APIs break, links rot. Our approach is *graceful degradation* : design research outputs so they can fall back from an interactive widget, to a video, to a static image. At the moment of publication, you get the richest experience. Over time, the content can degrade to simpler forms while remaining accessible. If the work is important, like a global climate model, it is worth the time and attention to keep those reproducible and interactive components from degrading.


## The Open Exchange Architecture


¶


The biggest initiative I’m working on right now is the[Open Exchange Architecture (OXA)](https://oxa.ink/) , a collaboration with Tracy Teal, CEO of Open Archive (home of bioRxiv and medRxiv), Nokome Bentley (Stencila), and many other contributors including eLife, PLOS, and Posit. The goal is a new standard for sharing science that is computationally enabled, modular, and composable — something that could be as widely adopted as the PDF, but designed for the way research actually works today.


The key idea is *composability* : just like software developers can install packages and build on each other’s work, scientists should be able to bring a piece of someone else’s research into their own in a structured, citable way. There’s nothing like that for science today, and unlocking it would have multiplicative effects on the pace of discovery.


## What Keeps Me Going


¶


The most rewarding part of this work is watching researchers’ creativity get unlocked. When a scientist realizes they can create an interactive widget connected to real data — not just a static advertisement of their work — there’s this moment of genuine excitement. I’ve been on calls where researchers lean back in their chairs and laugh, thrilled to finally share their work in a way that feels *authentic* .


That’s the future we’re building toward at Curvenote: standing on the shoulders of giants, for real this time.


## Related Articles


- [OpenRxiv & Curvenote’s new Reader launch took to the stage at FORCE11 2026 At FORCE11 2026, Curvenote showcased the newly launched Curvenote Reader deployed over all of openRxiv to enthusiastic feedback from scientific publishing leaders — with callouts on the innovation from the opening and closing keynotes as a meaningful step forward for open science. open-science conference presentation](https://curvenote.com/blog/force11-2026)
- [Curvenote Reader brings a better reading experience to half a million openRxiv preprints Curvenote Reader is now live across half a million bioRxiv preprints - bringing inline citations, zoomable figures, and connected context directly into the reading flow. Find out how we modernized one of science's biggest preprint repositories without disrupting a single workflow. open-science reader partnerships news](https://curvenote.com/news/openrxiv-launches-reader)
- [A Constellation of Open Research The openRxiv community came together to share plans for the future of the preprint ecosystem. We reflect on the vision and community energy that will shape the next few years, and how technologies like Curvenote are helping support a new generation of open research publishing. presentation open-science preprints openrxiv](https://curvenote.com/blog/czi-openrxiv-meeting-2025)
- [Preserving Executable Research Content Curvenote is helping to lead a new FORCE11 working group, "Preserving Executable Research Content: Challenges, Frameworks, and Best Practices". The working group will bring together researchers from diverse scientific communities to address the challenges of preserving and archiving computational notebooks and interactive research articles and artifacts over time. open-science working-group](https://curvenote.com/blog/force11-preserving-executable-research)
- [Expanding Open Access: How Open Source Principles Can Transform Scientific Communication A panel discussion with Lorena Barba, Rowan Cockett, Karthik Ram and Arfon Smith explores how open source software practices can reshape the way we communicate scientific discoveries. Adopting open source tools and processes could drastically improve scientific communication, especially with the growing complexity and interconnectedness of research data. open-science open-source presentation panel](https://curvenote.com/blog/open-source-software-powers-open-access)
- [Embracing Continuous Science at FORCE11 At FORCE11 2024 Curvenote presented on our vision of Continuous Science, where the principles of continuous integration and deployment, well-known in software development, can be applied to scientific publishing to improve speed, reproducibility, and feedback loops. open-science conference presentation](https://curvenote.com/blog/force11-2024)
