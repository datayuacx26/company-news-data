---
schema_version: "1.0.0"
document_id: "d17263e2bef275cea0725bcf95b53648b927c591c96cfcabf86fe97136a3e659"
company_key: "yc-1code-21stdev"
company: "1code (21st.dev)"
source_id: "yc-1code-21stdev-news-import-0b053d108858"
canonical_url: "https://21st.dev/blog/introducing-icons"
published_at: "2026-08-17T00:00:00+00:00"
first_seen_at: "2026-08-18T06:30:53.461768+00:00"
fetched_at: "2026-08-18T06:30:55.430611+00:00"
content_hash: "sha256:e5ab1077be2f6d6d6e89f6050aaddb0e7b68c4b8fc2339180733f0b974a0ef3e"
---

# Introducing Icons: 29,000 Icons, One Grid, One Click to Copy

# Introducing Icons: 29,000 Icons, One Grid, One Click to Copy


Seven icon families in a single searchable grid. Search by what the drawing means, set size and stroke before you copy, and see the same icon in every other family it exists in.


Serafim Korablev


[@serafimcloud](https://x.com/serafimcloud)


## Seven icon libraries, one grid


Picking an icon is rarely one search. It is a tab for Lucide, a tab for Phosphor, a third for the one set that happens to have "database restore", and then the slow realisation that the icon you found does not match the fifteen already in your project.


So we put them in one place. 21st Icons is 29,107 icons from seven design families, browsable in a single grid: Phosphor, Tabler, Hugeicons, Remix, Lucide, Heroicons and Material Line. Every set keeps its own licence, MIT, Apache-2.0 or ISC, and its own author credit. It is free to browse and free to use.


Filtering is the part that makes 29k usable. Narrow by family, by category, by style, outline, fill, duotone, bold, light or thin, or to animated icons only, and the count under the grid tells you how much is left.


## Search by what the drawing means


Icon names are a bad search index. Nobody types "arrow-uturn-left" when they mean undo, and no two libraries agree on whether it is "trash", "bin" or "delete".


So the catalogue is searched by meaning. Every icon carries generated descriptions of what it depicts, 193,000 of them across the catalogue, embedded and indexed. A query runs two channels at once: a vector lookup over those descriptions, and a lexical pass over names and tags for the times you do know the exact word. The two pools are merged and ranked together, so "backup restore" finds the right drawing in six families, and "lucide undo" still finds precisely what you asked for.


## Set it up before you copy, not after


The toolbar above the grid holds the decisions you would otherwise make in your editor: size from 16 to 64, stroke weight from 1 to 2.5, absolute stroke on or off, and the copy format. Every icon in the grid redraws live as you turn the knobs, so you are choosing an icon at the weight it will actually ship at.


Then a click copies. In the shape you asked for:


- **React** , a drop-in component,
- **SVG** , the plain markup,
- **Vue** or **React Native** , when that is where it is going,
- or just the **name** , for when you already have the package installed.


The attribution rides along in the copied header: which set it came from, who drew it, the licence, and the source. Credit is not something you have to remember to add later.


## One icon, and everywhere else it exists


Open any icon and the panel beside the grid answers the question a search cannot: where else does this drawing live?


- **Other cuts** , the same icon in the set's other weights and optical sizes. 11,618 icons in the catalogue are alternate cuts of another one.
- **Same icon, other families** , the same idea as the other six families drew it. There are 49,082 of these equivalences, matched offline, so opening the panel is two index reads rather than a search.
- **Similar** , the nearest neighbours by meaning, for when the icon you found is close but not it.


Everything in the panel is a grid of real icons, and everything in it copies on click, exactly like the main grid. It is a way to reach the icons the grid is not currently showing, not a second thing to learn.


## Animated icons, playing where you can see them


2,542 of the icons are animated. They play on hover, in the grid, at the size and weight you set, one at a time so a wide row does not turn into a fireworks display.


The animated sets ship as real React components, so copying one gives you the animation, not a still frame of it:


components/ui/backup-restore.tsx


Icons with an animated twin are marked in the detail panel, so a static icon is one click from the moving version of itself.


## What we are adding next


Sets from the community, published the way components and themes already are, and per-project icon collections you can hand to an agent. The catalogue is the foundation: everything in it is structured data, not a sprite sheet, which is what lets search, the family map, and the copy formats all work off the same numbers.


[Browse icons →](https://21st.dev/community/icons)


## Published


Aug 17, 2026


## Read time


5 min


## Tags


Launch


Icons


SVG


## Share


## Links


[Browse icons](https://21st.dev/community/icons)[Animated icons](https://21st.dev/community/icons/animated)
