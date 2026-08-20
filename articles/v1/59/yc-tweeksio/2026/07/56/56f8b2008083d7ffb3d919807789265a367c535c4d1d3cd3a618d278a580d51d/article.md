---
schema_version: "1.0.0"
document_id: "56f8b2008083d7ffb3d919807789265a367c535c4d1d3cd3a618d278a580d51d"
company_key: "yc-tweeksio"
company: "Tweeks.io"
source_id: "yc-tweeksio-rss-bbf1d949eada"
canonical_url: "https://www.tweeks.io/blog/save-x-twitter-articles-as-pdf-or-markdown"
published_at: "2026-07-08T00:00:00+00:00"
first_seen_at: "2026-07-20T23:24:17.378405+00:00"
fetched_at: "2026-07-28T21:08:44.176891+00:00"
content_hash: "sha256:e99f4ca4ca805d15f8723884ff7457df187cec522a721a382266b99d9d644ba3"
---

# How to Save X/Twitter Articles as PDF or Markdown

X Articles are the long-form side of X: headings, embedded images and video, and a limit of about 100,000 characters, which is enough room for a serious essay. People publish investor letters, technical deep dives, and multi-part arguments there. What X doesn't give you is any way to get one back out. There's no download option and no export setting. You can bookmark an article, which files it away inside the same app, or you can select-all and copy, and watch the headings, images, and embedded posts fall off on the way to wherever you paste.


That gap is what the[X Article Export Button](https://www.tweeks.io/t/bc2926d27d5a4c6885dfd49c) fills. It adds one button to the article header and does two things: save the article you're reading as a PDF, or save it as a Markdown file. Below is the setup and what comes out the other end, tested on Marc Andreessen's "SpaceX & the Sentient Sun".


## The Export Button


Once the tweek is installed, an Export pill appears in the header of every article read view, next to X's Focus mode arrows:


Click it and a dropdown offers the two formats:


That's the whole interface. The button shows up on x.com and twitter.com, both on article URLs and on the status permalinks where articles actually open when you click one in the timeline. It doesn't appear on ordinary posts or threads; this tweek is for articles.


## Setup


1. Install the[Tweeks extension](https://www.tweeks.io/get-extension) for Chrome or Firefox.
2. Open the[X Article Export Button](https://www.tweeks.io/t/bc2926d27d5a4c6885dfd49c) share page. The full source, 2,116 lines, is right there if you want to read what you're installing. Click install.
3. Open any X article. The Export button appears in the header within a second or two.


If you don't have an article in front of you, check the Articles tab on a writer's profile; anyone who publishes them has one.


## Save an X Article as a PDF


1. Open the article and let it finish loading. If it's long, scroll through it once so X loads every image; the exporter can only see what the page has.
2. Click Export, then PDF.
3. The button shows a loading state while images are fetched and the document is built. An image-heavy article takes a few seconds.
4. A PDF named after the article lands in your downloads. Ours came out as` SpaceX_the_Sentient_Sun.pdf` .


The result is a clean A4 document, not a screenshot of the page: title at the top, author with @handle, the source URL and export date, then the article.


The structure carries over. Section headings keep their hierarchy, bulleted and numbered lists stay lists, images are embedded at a sensible size, and links remain clickable, set in X's blue. If the article quotes another post, the exporter renders that post as a small snapshot image, so it still reads as a post instead of dissolving into the paragraph around it.


The useful property of the PDF is that everything is inside the file. Our test export runs 22 pages and 2.9 MB because every image in the article is embedded. If the piece is edited or deleted tomorrow, you still have all of it. That makes it the format for archiving, printing, and sending to people who don't have an X account.


## Save an X Article as Markdown


Same button, other option:


1. Open the article, let it load, scroll once if it's long.
2. Click Export, then Markdown.
3. A` .md` file named after the title downloads, almost immediately for most articles.


The export opens with a short metadata block, then the article in clean Markdown. This is the actual head of our test file:


markdown


```text
# SpaceX & the Sentient Sun


**Author:**   pmarca (@pmarca)
**Source:**   https://x.com/pmarca/status/2066523671456579828
**Exported:**   2026-07-07T00:46:50.204Z


---


![ Image  ]( https://pbs.twimg.com/media/HK24VGFbgAAYWHF?format=jpg&name=large  )


By [ @mikemcg0  ]( https://x.com/@mikemcg0  ) and myself


Elon Musk’s compensation package at SpaceX is structured around two targets. ...


# Working Back from the Future
```


Formatting survives the trip: the article's section headings become` #` and` ##` headings, quote blocks become` >` blockquotes, lists come through with their numbering rebuilt, inline links keep their targets, and emoji are preserved. Images sit exactly where they sat in the article, as standard Markdown image links pointing at X's media CDN. Video can't live in a text file, so each video contributes its thumbnail instead. Quoted posts are the one special case: they're captured as snapshot images and embedded directly into the file, so they display even with no connection.


One difference from the PDF is worth knowing: apart from those snapshots, the Markdown file references its images remotely rather than containing them. That keeps it tiny, our roughly 8,000-word export is 50 KB, and the images load as long as X keeps hosting them. But a link is not a copy. Archive with PDF; work with Markdown.


And working with it is the point. A Markdown export pastes into Obsidian or Notion with its outline intact, sits comfortably in a git repo, and is the format AI assistants read best. "Read this article and pull out every concrete claim about launch costs" goes a lot better when the model gets headings and lists instead of a wall of flattened text. If you save chat transcripts the same way, the[chat exporter we covered earlier](https://www.tweeks.io/blog/export-gemini-chats-to-markdown) exists for the same reason.


## PDF or Markdown?


If you're unsure, export both; each is two clicks. Otherwise: PDF is the archival copy, self-contained and readable by anyone. Markdown is the working copy, searchable, editable, and machine-readable. Decide by what you plan to do with the article after you've saved it.


## How It Works, and What It Can't Do


Everything runs in your browser. The tweek reads the article out of the open page, converts the text block by block, and builds the file locally with two open-source libraries, jsPDF for the PDF and html2canvas for the quote-post snapshots, fetched from the cdnjs CDN. Those library downloads and X's own image CDN are the only network traffic involved. The article text is never sent anywhere.


The honest limitations:


- It exports articles, not timelines. Ordinary posts and threads don't get the button.
- It can only export what your browser can display. If you can't read the article, neither can the exporter.
- Video exports as a thumbnail, not a playable file.
- It depends on X's page structure, so a redesign can break extraction until the tweek is updated. When an export fails you get an alert, not a silently broken file.


Because it's a tweek, the defaults are also just defaults. Open it in Tweeks and describe a change: name the files by author and date, use letter paper instead of A4, skip images for a text-only archive. Edits are one-sentence requests, and the[Tweeks vs. Tampermonkey](https://www.tweeks.io/blog/tweeks-vs-tampermonkey) post walks through how that works. X is also one of the most modified sites in the library, so there's a whole shelf of other tweeks for it;[browse x.com](https://www.tweeks.io/sites/x.com) .


If an article is worth rereading, it's worth a copy that doesn't depend on X keeping it up.


## Sources


- [X Article Export Button tweek](https://www.tweeks.io/t/bc2926d27d5a4c6885dfd49c)
- [About Articles, X Help Center](https://help.x.com/en/using-x/articles)
- ["SpaceX & the Sentient Sun", the test article](https://x.com/pmarca/status/2066523671456579828)
- [Tweeks for x.com](https://www.tweeks.io/sites/x.com)
