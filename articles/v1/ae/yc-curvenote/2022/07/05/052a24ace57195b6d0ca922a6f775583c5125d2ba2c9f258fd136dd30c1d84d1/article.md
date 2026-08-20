---
schema_version: "1.0.0"
document_id: "052a24ace57195b6d0ca922a6f775583c5125d2ba2c9f258fd136dd30c1d84d1"
company_key: "yc-curvenote"
company: "Curvenote"
source_id: "yc-curvenote-news-import-2221551dc8c3"
canonical_url: "https://curvenote.com/blog/share-research-online-in-60-seconds"
published_at: "2022-07-25T00:00:00+00:00"
first_seen_at: "2026-07-23T06:57:34.135292+00:00"
fetched_at: "2026-07-28T21:40:00.658555+00:00"
content_hash: "sha256:f283da6770e315bb875fb594f71a98a32978402b018b62c40609d39d611b75af"
---

# Share research online in 60 seconds

# Share research online in 60 seconds


July 25, 2022


Today we are launching a new way to easily share scientific content online — we are calling it the[Curvenote Launchpad](https://curvenote.com/demos/publish-from-github) and it is free and[open-source](https://github.com/curvenote/curvenote) . All you need to start is GitHub or GitLab repository.


## Curvenote Launchpad


¶


The launchpad service is available at[https://try.curvenote.com](https://curvenote.com/demos/publish-from-github) and can turn any repository with Markdown` .md` and` .ipynb` files into a rich interactive website in under 60 seconds.


Figure 1: The Curvenote Launchpad service to turn an existing GitHub repository with


We’ve been working to ingest and support a wide variety of repository layouts, so the tool will automatically descend into subfolders to find markdown and Jupyter Notebooks and present a default Table of Contents.


The service will give you a unique URL, something like` https://launchpad-3j8c8sj2.curve.space` that you can share and will remain active for 5 days.


Want to publish a longer lived website?


It’s easy to configure your repository and publish your content, with no expiry, on our hosting service for free.
Read more[in our documentation](https://curvenote.com/docs/web/launchpad) .


## Awesome by default


¶


We are working on making content your content as awesome by default as we can, and here are a few of the features you can expect to see so far:


### Jupyter Outputs


¶


Curvenote sites support the rendering of Jupyter output cells. As well as the fast server-side rendering of rich text outputs and static images like those produced by` matplotlib` or` seaborn` , the sites can host interactive plots from` bokeh` ,` holoviews` ,` altiar` and` plotly` provided the notebooks are saved with their outputs when the site is generated.


### BibTeX support


¶


If the repository contains BibTeX` .bib` files, the service will load the references and pick up on any citations you’ve made in your Markdown files or Markdown notebook cells, we currently support[MyST Markdown](https://mystmd.org/) for these sorts of references.


### References from DOIs


¶


If you have included any Markdown or HTML links to a DOI, these will automatically be made into hover citations as shown below, and the reference will be added to the reference section at the bottom of the document.


Figure 3: Example screenshot is taken from[https://jarmitage.curve.space/egu22/presentation](https://jarmitage.curve.space/egu22/presentation)


### Titles


¶


Launchpad will guess at the best titles to use in Table of Contents based on folder names, file names, and the markdown content itself. That means if a top-level heading (` #` ) is at the top of your markdown` .md` file or in the first cell of your notebook, that is what will be picked up, and if not the filename will be parsed to produce a capitalized string. If you get repeated names in your Table of Contents, then this is something that you might want to tweak by inserting new headings or[frontmatter](https://curvenote.com/docs/web/frontmatter) in your files.


### Starter Frontmatter


¶


Curvenote websites have a[whole collection of frontmatter settings](https://curvenote.com/docs/web/frontmatter) available. All are relevant for scientific publications, and the launchpad service will populate a few of these based on your GitHub profile or organization information. If you move on to[set up a permanent website](https://curvenote.com/docs/web/launchpad) , you[can configure much more information](https://curvenote.com/docs/web/frontmatter) .


Figure 4: Setting up frontmatter to show on your articles, including organization information, GitHub links, authors and affiliations.


### Homepage


¶


The first page of the website produced is taken in priority order from` index.md` ,` readme.md` ,` main.md` ,` *.md` ,` index.ipynb` ,` readmd.ipynb` ,` readme.ipynb` , or` *.ipynb` (not case sensitive) and usually, you will have a` README.md` at least. You can override that as the homepage by adding an` index.md` to your repo before launching.


## What’s next


¶


Curvenote Launchpad is an easy way to[share Juptyer Notebooks, research notes and other markdown content](https://curvenote.com/demos/publish-from-github) with a quick, disposable link. If you like the service and the result, we’d love you to share it more widely, with a tweet ❤️.


Launchpad is also a great way to quickly try out Curvenote’s site generation capabilities on your content before setting up a permanent website with your own URL. Curvenote’s publishing tools are open source, and our mission at Curvenote is to free science from the PDF. Let us help you do that by getting your[preprint, supporting materials or thesis online](https://curvenote.com/docs/web) .


## Related Articles


- [Top takeaways from JupyterCon 2023 Curvenote and ExecutableBooks were at JuptyerCon 2023 in Paris, between all the amazing announcements & talks, here are our main takeaways. events jupyter](https://curvenote.com/blog/jupytercon-2023-takeaways)
- [Permissions, website updates, MDPI journal template and LaTeX diffs for submission We show off an example of an archeology article, a sneak peek and the new website, and discuss creating LaTeX diffs for pdfs. website tables figures jupyter weeknote](https://curvenote.com/blog/2021-08-16-permissions-tables-figures-diffs)
- [OpenRxiv & Curvenote’s new Reader launch took to the stage at FORCE11 2026 At FORCE11 2026, Curvenote showcased the newly launched Curvenote Reader deployed over all of openRxiv to enthusiastic feedback from scientific publishing leaders — with callouts on the innovation from the opening and closing keynotes as a meaningful step forward for open science. open-science conference presentation](https://curvenote.com/blog/force11-2026)
- [Curvenote Reader brings a better reading experience to half a million openRxiv preprints Curvenote Reader is now live across half a million bioRxiv preprints - bringing inline citations, zoomable figures, and connected context directly into the reading flow. Find out how we modernized one of science's biggest preprint repositories without disrupting a single workflow. open-science reader partnerships news](https://curvenote.com/news/openrxiv-launches-reader)
- [Curvenote on the Data Engineering Podcast Rowan Cockett joined the Data Engineering Podcast to talk about reproducibility, open science, and the future of scientific communication. open-science podcast](https://curvenote.com/blog/data-engineering-podcast)
- [A Constellation of Open Research The openRxiv community came together to share plans for the future of the preprint ecosystem. We reflect on the vision and community energy that will shape the next few years, and how technologies like Curvenote are helping support a new generation of open research publishing. presentation open-science preprints openrxiv](https://curvenote.com/blog/czi-openrxiv-meeting-2025)
