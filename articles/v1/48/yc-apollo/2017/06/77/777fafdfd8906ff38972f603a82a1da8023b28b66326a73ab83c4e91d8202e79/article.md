---
schema_version: "1.0.0"
document_id: "777fafdfd8906ff38972f603a82a1da8023b28b66326a73ab83c4e91d8202e79"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/graphql-just-got-a-whole-lot-prettier-7701d4675f42"
published_at: "2017-06-28T18:34:40+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T22:27:26.939710+00:00"
content_hash: "sha256:49ab1443a4cd72699d5b6d306080f40f170fc24e327ccb06c5ef786728fd5939"
---

# GraphQL just got a whole lot “Prettier”!

We’ve all seen the` prettier`[hype train](https://github.com/prettier/prettier#ride-the-hype-train) (and a lot of us are on it!) and while it began as an opinionated code formatter for JavaScript, it has quickly grown to take on other languages like Flow, Typescript, and CSS. As of` prettier@v1.5.0` , it now supports one of my favorites,[GraphQL](http://graphql.org/) !


Let’s let the GIF do the talking:


Prettier formatting[@GraphQL](https://twitter.com/GraphQL?ref_src=twsrc%5Etfw) inside template literals is everything I needed in my life 😍[pic.twitter.com/oScCMyC84W](https://t.co/oScCMyC84W)


— kitze 🚀 (@thekitze)[June 27, 2017](https://twitter.com/thekitze/status/879777868611452928?ref_src=twsrc%5Etfw)


Using` prettier` for GraphQL is more than just hitting the “prettify” button in GraphiQL — it’ll take into account things like maximum line-width, whether or not you like commas in your queries, breaking up long lists of arguments on to their own lines, and even works on queries embedded in your JavaScript.


If you haven’t given it a try, its[really easy to get started](https://github.com/prettier/prettier#usage) ; soon you’ll realize you and your team will stop having to nitpick things like spaces, braces, and commas and can focus on basking in the sweet glory of GraphQL!


---


## How It All Happened


GraphQL support didn’t just happen overnight, though. It took a little bit of prodding, some tweets, some Github issues, and a couple of developers who found it wildly entertaining to add support for every corner of the language (I’m looking at you,[Vjeux](https://medium.com/u/46fa99d9bca4?source=post_page-----7701d4675f42----------------------) and[Sashko](https://medium.com/u/803918030a60?source=post_page-----7701d4675f42----------------------) !) Here’s the rundown:


### It started with a tweet


While[Vjeux](https://medium.com/u/46fa99d9bca4?source=post_page-----7701d4675f42----------------------) had already given GraphQL support a shot, I only heard about this from this tweet:


Who wants to work on continuing this experiment with me?[https://t.co/3i2XYayxhQ](https://t.co/3i2XYayxhQ)[#GraphQL](https://twitter.com/hashtag/GraphQL?src=hash&ref_src=twsrc%5Etfw)


— Sashko Stubailo 🇺🇦 (@stubailo)[June 5, 2017](https://twitter.com/stubailo/status/871584723524505600?ref_src=twsrc%5Etfw)


Immediately, I knew I had to be a part of this. I’d spent way too long formatting my braces and wanted the power of` prettier` everywhere!


[Sashko](https://medium.com/u/803918030a60?source=post_page-----7701d4675f42----------------------) gets us in the room where it happens[with a short discussion](https://github.com/prettier/prettier/issues/1968) on how we can move forward with support.[Vjeux](https://medium.com/u/46fa99d9bca4?source=post_page-----7701d4675f42----------------------) gives us permission to steal his work, and I[take him up on his offer](https://github.com/prettier/prettier/pull/1982) , and soon thereafter initial support for GraphQL in` prettier` was shipped. It wasn’t perfect, but it was a start.


### Opening up the flood gates


Enter[Sashko](https://medium.com/u/803918030a60?source=post_page-----7701d4675f42----------------------) .


Sashko got a little excited. Okay he got a LOT excited.


Instead of having to wait for every part of GraphQL to be ironed out,[Vjeux](https://medium.com/u/46fa99d9bca4?source=post_page-----7701d4675f42----------------------) ‘s commitment to iteration made it really easy to get in and out and add things, little by little. I barely had a chance to give it a try myself!


With[support for comments](https://github.com/prettier/prettier/pull/2187) and[support for template tags](https://github.com/prettier/prettier/pull/2092) (by[Lucas Azzola](https://github.com/azz) ), we had everything we needed for the final launch.


### This is just the beginning!


Even with support for GraphQL launched, its not perfect. There’s bound to be corner cases with the existing language, and with new language constructs on the horizon we’re sure to see changes and improvements to how GraphQL can be formatted. Even in the first few hours of its release,[Vjeux](https://medium.com/u/46fa99d9bca4?source=post_page-----7701d4675f42----------------------) was hard at work making it work for the GraphQL Schema Language :


## Teamwork Makes the Dream Work!


Special thanks to[Vjeux](https://medium.com/u/46fa99d9bca4?source=post_page-----7701d4675f42----------------------) and[Sashko](https://medium.com/u/803918030a60?source=post_page-----7701d4675f42----------------------) for the legwork in getting this out the door. Even more thanks to those submitting issues and PRs to make sure what we built is addressing all the corner cases, too. We hope this tool is as useful to you as it has been for us!


Written by


Jon Wong


[Read more by Jon Wong](https://www.apollographql.com/blog/author/jnwng)
