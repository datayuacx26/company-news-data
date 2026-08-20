---
schema_version: "1.0.0"
document_id: "4aff584731bee2287a1289689d78381be02ff75645e1d6b81ad2cbd2b1490195"
company_key: "shutterstock-inc-common-stock"
company: "Shutterstock Inc."
source_id: "shutterstock-inc-common-stock-rss-4c794ed87dc5"
canonical_url: "https://tech.shutterstock.com/2018/10/30/improving-multilingual-text-support-in-shutterstock-editor"
published_at: "2018-10-30T04:00:00+00:00"
first_seen_at: "2026-07-20T23:18:13.615263+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:d7951d5675f9d77bf35f58345f640d52ba659808e76164072b8cf2102a245317"
---

# Improving Multilingual Text Support in Shutterstock Editor

While Shutterstock supports 21 languages, our image editing tool,[Shutterstock Editor](https://www.shutterstock.com/editor) , initially launched in English only. In order to deliver the same quality of experience for all 21 languages on Shutterstock Editor, we needed to make adjustments to how the tool handled characters in those languages.


Shutterstock Editor uses[Fabric.js](http://fabricjs.com/) (an open source project to which Shutterstock is a major contributor) to manage rendering and text editing. At time of implementing text tools, Fabric.js was at version 1.7 and lacked proper support for languages that have grapheme clusters.


While JavaScript can recognize[code points](https://en.wikipedia.org/wiki/Code_point) in the[Basic Multilingual Plane](https://en.wikipedia.org/wiki/Plane_(Unicode)#Basic_Multilingual_Plane) , it does not have native helper functions for handling grapheme clusters.


If we take, for example, the expression “once upon a time” in Thai, one of Shutterstock’s supported languages, and we ask an HTML canvas element to paint it, this is what is rendered:


It looks perfectly valid.


Within Shutterstock Editor, in order to give text objects a size for rendering, we` split()` the text in pieces and measure each of them independently to construct a total, aggregate size of the string.


Splitting this text means we get a list of code points of which it is comprised:


We can see above that we get 13 distinct characters, though the original text appears to be organized as 9.


The resulting bounding box is larger than appropriate and the cursor position is split across 13 characters (based on individual code points) instead of the intended 9 user-perceived characters (based on grapheme clusters). That meant the typing experience was broken as a result.


Typing the letter` a` when the cursor was in the above position ends up as such:


This is a somewhat lucky case because it is also possible for a character to be positioned in the middle of a grapheme cluster, which would result in an even stranger outcome.


In the following example if we try to enter a character after` รั้` , we position our cursor to the right of it, but for Fabric.js the second letter is interpreted as just the base character` ร` without the following additional combining characters` ั` and` ้` .


Inserting a new character in that location breaks the text sequence, resulting in loss of contextual meaning for the native browser rendering methods while also illogically rendering the combining characters independently.


Moving briefly away from Thai language support and grapheme clusters, it’s interesting to see that emojis also have what appears to be a similar problem:


It was clear that we needed to move from a` .split()` approach to one that operated with deeper grapheme cluster knowledge. Identifying grapheme clusters is not trivial, however—they are not things you can derive simply from a few ranges of code points. Fortunately, there are unicode specs for grapheme clusters and libraries that can split strings of text into arrays of grapheme clusters.


So we reorganized Fabric.js code to read full Unicode characters (code points). We did not want to overload the library with our own needs and massive dependencies from other packages, so we selected a Mozilla Developer Network solution for a very[simple grapheme splitter function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/charAt) that serves as a general splitter, and we updated the Fabric.js library to work on the basis of Unicode characters and not code units. These changes were one of the key features of the Fabric.js[2.0 release](https://github.com/fabricjs/fabric.js/blob/master/CHANGELOG.md#200) .


To fully support splitting on user-perceived characters including extended grapheme clusters and emoji sequences, we mixed 2 npm packages ([emoji-regex](https://www.npmjs.com/package/emoji-regex) and[grapheme-splitter](https://www.npmjs.com/package/grapheme-splitter) ) since no single package handled both languages *and* emojis.


Now, at every text input or change, we use Fabric.js to take the user text, split it into lines and grapheme clusters, producing the desired result as follows:


With these changes to Fabric.js, Shutterstock Editor is now able to operate on these characters as naturally as it did with English, completing support for all 21 languages (and even emoji 😄) in the tool.
