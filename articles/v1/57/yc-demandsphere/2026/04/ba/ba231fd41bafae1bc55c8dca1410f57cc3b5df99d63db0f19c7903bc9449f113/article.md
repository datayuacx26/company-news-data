---
schema_version: "1.0.0"
document_id: "ba231fd41bafae1bc55c8dca1410f57cc3b5df99d63db0f19c7903bc9449f113"
company_key: "yc-demandsphere"
company: "DemandSphere"
source_id: "yc-demandsphere-rss-b433e6d35ba4"
canonical_url: "https://www.demandsphere.com/blog/new-site-launched/"
published_at: "2026-04-05T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:28.972599+00:00"
fetched_at: "2026-07-28T21:56:48.286898+00:00"
content_hash: "sha256:8801c40af38c2b588f7d55ca2d476e2820f3542447cb40747c90631233de945f"
---

# New site launched, still making some improvements

If you’ve been to our site before, you might notice that the whole website has been revamped.


This update has been a long time coming, particularly because since our last site update a couple of years ago, we have added a whole new set of product capabilities to our platform around LLM visibility tracking.


I’m going to be doing a deeper walkthrough of the total migration at a later date because there are some things that readers may find useful.


The biggest change on this site vs. our previous site is that we have migrated from a WordPress-based site hosted on Kinsta to a Jekyll-based site hosted on Cloudflare Pages.


## Why Jekyll (and why Markdown with static site generators in general)


Interestingly enough, our original website, way back in 2010, was built on top of Jekyll. It was everything a solo founder needed: text-based, easy to manage, and version-controlled. This was way before the days of Cloudflare Pages but Github Pages was around and, if memory serves, that’s where I hosted it.


As we grew, I bought into the “common sense” wisdom that everything had to be moved to Wordpress in order to give marketing teams the ability to edit the site without needing to know how to work with repositories and touch this weird text format called Markdown.


Fast forward to 2026 and Markdown has become the primary language of AI on the web (and in general). Many more people are also comfortable with using Github and repositories and I’ve trained everyone on our marketing team to use both effectively.


In fact, our internal Help Center (behind login wall) is based on[Docusaurus](https://docusaurus.io/) , another Markdown-based static site generator fine-tuned for documentation.


## Jekyll vs. Astro


When it comes to static site generators (SSGs), there are multiple options.


The cool kid on the block right now is[Astro](https://astro.build/) . Astro looks great and has a lot of good features. It mostly came down to preference with me and I’d personally rather work with Ruby than JavaScript. Both are great options.


Interestingly enough, Cloudflare’s new “AI-native spiritual successor to Wordpress editor”[EmDash](https://blog.cloudflare.com/emdash-wordpress/) , is based on Astro.


So it’s great to see more recognition around the power of versioned Markdown paired with SSGs hosted on platforms like Cloudflare and Github Pages.


## The future is Markdown (and versioned context windows)


As you’ll hear me mention often in my talks, the idea of the context window, as a metaphor, extends well beyond generative AI sessions with LLMs.


Context windows are everywhere, always have been, always will be.


Markdown is a quite powerful format for hosting these windows and websites are an obvious fit here too.


I’ll wrap this post up on that note today, I’m tring to keep this one short.


Please pardon the dust as we make some final tweaks to the site but we’re excited as this will enable us to move much faster than before.
