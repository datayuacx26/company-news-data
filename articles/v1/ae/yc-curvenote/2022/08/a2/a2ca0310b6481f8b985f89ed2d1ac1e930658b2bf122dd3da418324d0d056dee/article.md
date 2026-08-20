---
schema_version: "1.0.0"
document_id: "a2ca0310b6481f8b985f89ed2d1ac1e930658b2bf122dd3da418324d0d056dee"
company_key: "yc-curvenote"
company: "Curvenote"
source_id: "yc-curvenote-news-import-2221551dc8c3"
canonical_url: "https://curvenote.com/blog/2022-08-01-hidden-pages-thebe"
published_at: "2022-08-01T00:00:00+00:00"
first_seen_at: "2026-07-23T06:57:34.135292+00:00"
fetched_at: "2026-07-28T21:20:09.527818+00:00"
content_hash: "sha256:97ba7ae4bccfade693e7eecfa9641551af090f20644f460f0ac6ae03e212e4a1"
---

# Hidden pages & Thebe Release

# Hidden pages & Thebe Release


## Week of Aug 1, 2022


August 1, 2022


## Hidden Pages


¶


Publishing Curvenote sites became even easier last week when we launched our new publish button in the Curvenote editor. Now we’ve added the ability to hide articles and notebooks within your project so that you can control which of these appear as pages on your published site. This allows you to keep drafts or working material within a project, without it being published to your website.


Figure 1: Editing block settings to hide a document when publishing to a website.


The hide option is located in the Article Settings dialog, and hidden articles in a project are identified with a visibility icon, as shown below.


Figure 2: Hidden notebooks in a project!


## Merged Repos together! 👩🔬


¶


We have consolidated many of our open source repositories to simplify the experience for development and make our GitHub footprint a little bit more sensible!


-


Command line tools and website utilities:


-


[https://github.com/curvenote/curvenote](https://github.com/curvenote/curvenote)


-


All of the work for our scientific editor:


-


[https://github.com/curvenote/editor](https://github.com/curvenote/editor)


This has simplified the install and build experience, and will lead to speeding up things for end users as well!


Figure 3: This did mean touching a lot of code to make everything work again!


## Thebe Release Candidate


¶


This week we published` thebe`` v0.9.0-rc.0` as part of our ongoing work on the executable books codebase. While` thebe-core` is essentially a headless connector library for Jupyter that allows any Javascript app to connect to and compute on sessions on a Jupyter server (we talked about this in[Themes & Interactive Computing](https://curvenote.com/blog/2022-06-27-themes-and-interactive-computing)


a few weeks ago),` thebe` providing that connectivity while also converting code snippets on a page into Codemirror cells for editing and execution.


We refactored` thebe-core` out of` thebe` to provide Jupyter connectivity in different application contexts, and now we’ve reintegrated that into` thebe` upgrading the latter to typescript in the process, as well as introducing a bunch of improvements including better messaging and JuptyerLite support.


Check in on the status of the release candidate to[integrate thebe-core](https://github.com/executablebooks/thebe/pull/554) .


## Related Articles


- [Publishing Computational Notebooks at AGU23 Curvenote launches Notebooks Now! at the American Geophysical Union Annual Meeting in San Francisco, where 20,000+ scientists descended on Moscone Center for five days of wide open science. publishing conference presentation](https://curvenote.com/blog/publishing-computational-articles)
- [One Click Publishing for Open Research Websites A Curvenote webinar taking attendees through publishing and updating research websites directly from the Curvenote visual editor webinar publishing editor](https://curvenote.com/blog/one-click-publishing-for-open-research)
- [Webinar: One Click Publishing This week we hosted a webinar showing off our new publishing in-app no-code publishing features webinar publishing weeknote](https://curvenote.com/blog/2022-08-15-one-click-publishing-webinar)
- [Connecting ORCID and Curvenote An orcid.org is a persistent digital identifier for researchers which is widely used. You can now conenct your ORCID account directly to your Curvenote profile. orcid integration weeknote](https://curvenote.com/blog/2022-08-08-orcid-integration)
- [Publish from Curvenote You can now publish directly from the Curvenote platform, including setting domains in the project settings. publishing weeknote](https://curvenote.com/blog/2022-07-18-publish-from-curvenote)
- [Thumbnails & Meeting in Person We added thumbnails to the article frontmatter, as well as met up with the whole Curvenote team in person for the first time! cli seo weeknote](https://curvenote.com/blog/2022-07-11-thumbnails-and-meeting-in-person)
