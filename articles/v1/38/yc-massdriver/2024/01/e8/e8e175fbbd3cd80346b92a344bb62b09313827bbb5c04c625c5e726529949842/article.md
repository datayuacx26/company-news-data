---
schema_version: "1.0.0"
document_id: "e8e175fbbd3cd80346b92a344bb62b09313827bbb5c04c625c5e726529949842"
company_key: "yc-massdriver"
company: "Massdriver"
source_id: "yc-massdriver-rss-63dfbe6093ab"
canonical_url: "https://www.massdriver.cloud/blogs/tales-from-production-misaligned-text"
published_at: "2024-01-31T00:00:00+00:00"
first_seen_at: "2026-07-24T10:43:35.035955+00:00"
fetched_at: "2026-07-28T22:26:16.511012+00:00"
content_hash: "sha256:b54c079dd5e0a7f4e692b4a63547ec02386ed6fc9d1b7efa69578f5b6b9bb1ee"
---

# Tales from Production: Misaligned Text

I just had a random memory from a production bug like 18 years ago, chuckled, and figured I’d write it down before I forget it again.


We had this tool that would let you visually design ads, a photoshop lite if you will, in the browser. (Yes, this was very hard to build - and there was a “no Flash” requirement. It sucked.)


Well, one day, when rendering ad images, the text would randomly be slightly off from where you had placed it.


It only seemed to happen sometimes to some people, we thought users were accidentally moving the text. But sure as heck, more people started complaining about it. Every once in a while you’d overlay some text on an image, hit save, and the text would move.


The project manager on the team spent the better part of a week trying to recreate the problem and he finally figured out that it was *one “* ***specific*** *” font.* (It was not.)


He could recreate it. He put some words over an image, bam, words move a little left or right.


Eventually, he noticed that the font even *looked* different.


Ya see, kids, there was no widespread implementation of the *canvas* element at the time, nothing in Internet Explorer 6 would let you turn a bunch of HTML layers into an image. So the way the product worked, we sent an XML description of the layers to the backend and it would render an image using imagemagick parsing the layer details from the XML.


The fonts that we had available in the browser were not the fonts installed on our RedHat Linux servers!!! 🎉


When the font wasn’t present, ImageMagick would use a system font instead.


To do some of the layering we had to generate bounding boxes around text (like if it had a BG color). We calculated that in the browser using the letter kerning of the font. So depending on if it was left, center, or right justified it would “shift” a little bit when the kerning math calculated a smaller or larger box that was needed by the font on the backend.


No one was a font nerd. The team couldn’t tell the difference between any given font.


The devs didn’t.


The Ops team didn’t.


The PM didn’t even notice.


Heck, customers designing stuff didn’t even notice. They reported “text moves when saving”, not that the font was wrong.


We ended up taking the common fonts across all “major” browsers (IE6, FF1.5, Safari) and limiting the UI to only having those as an option. We included the font files in the code repo and during deployment would ensure they were installed.


Installing the fonts … in a pre-DevOps world, whose responsibility was that?
