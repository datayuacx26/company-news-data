---
schema_version: "1.0.0"
document_id: "0ef63dfe8927a445da1c7133eab457988f9f7ad0c4291924edbc9893adb3d0e9"
company_key: "yc-repaint"
company: "Repaint"
source_id: "yc-repaint-news-import-c75d4511f1b1"
canonical_url: "https://repaint.com/blog/ai-is-good-at-web-design-now"
published_at: "2026-06-15T00:00:00+00:00"
first_seen_at: "2026-07-22T11:30:23.764508+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:c9ece38bfda577966f4b31ad2b6456bb7cc8a39f8d296caf35ca9c3c888e8afb"
---

# AI Is Good at Web Design Now

For the last few years, most websites made with AI have been generic and mediocre. That's why so much of it gets called slop. But in the past couple of months, I've noticed a major shift that nobody seems to be talking about: AI has gotten dramatically better at web design.


I've got a better view of this than most people because I'm the ceo of[Repaint](https://repaint.com/) , an AI website builder. At this point, I've looked at ***thousands*** of websites people made with AI, and lately the good ones have gotten noticeably better. Here are a few that turned my head:


Pretty good, no? I certainly wouldn't call them slop. I'm pretty sure these are better than what most web designers would come up with.


What impressed me most was the aesthetic. Six months ago, AI models would never make websites that looked this unique. The AI simply didn't have enough taste. Before, you had to provide all the taste yourself with precise instructions to get a site like this.


But now the AI can get much further. Each of these sites still took a few days of iteration, but they had 80% of the final aesthetic within the first few versions. And they were made by business owners just talking to AI, not professional agencies.


## So what changed?


I wish the secret sauce was Repaint, but it was mostly the models. We use a fairly standard AI agent with models from Anthropic, OpenAI, and Google. We've built special tools for rebuilding existing websites, but nothing groundbreaking that would explain this jump in design quality. Any tool using the latest models has probably seen a similar improvement.


Right now, our main model for website design is Opus 4.8. Anthropic released several updates in quick succession:


-


Opus 4.6 - February 5, 2026


-


Opus 4.7 - April 16, 2026


-


Opus 4.8 - May 28, 2026


I started noticing the shift in early May, though it may have already been there when we were using Opus 4.6.


In our testing, models from OpenAI and Google have not improved at web design to the same degree. GPT-5.5 is smart, but it makes noticeably worse websites. If you're an OpenAI employee reading this, please make the next one better at websites.


## The Difference


The biggest difference is that the new models are much better at following design direction. To show what I mean, I had two agents build the same landing page from the same prompt. The first used Opus 4.5, an older model. The second used Opus 4.8, a newer model. Both had access to an image generation tool. Here's the prompt:


> Create a website for an architecture firm. It should be inspired by Bauhaus design, with a minimalist aesthetic. Mostly neutrals, with a few bold colors. It should use broad images that fill lots of space.


#### Older Model - Opus 4.5


This site looks polished at first glance, but almost all of that comes from the images. The design itself is just a simple layout with big photos. It ignored the bold colors and never developed a recognizable Bauhaus aesthetic. That was the limitation of older models: they could make something clean, but they couldn't really translate a design direction.


#### Newer Model - Opus 4.8


I'm not sure this site is objectively better, but it followed my directions much more closely. Opus 4.8 found clear ways to use bold colors and Bauhaus-inspired design throughout the site. The result feels genuinely distinctive, and it is obviously based on the prompt.


The new models can follow instructions across a much wider range of styles. You can google "web design styles" and find a million directions to try. Change the visual direction in the prompt, and the entire website changes with it.


These are all one-shot generations:


#### Brutalism


> Create a website for an architecture firm. It should feel inspired by brutalism, with a raw and heavy look. Use mostly concrete tones, black, and white, with one or two sharp accent colors. The layout can feel a little rigid and oversized, with broad images that take up a lot of space.


#### Playful Colors


> Create a website for an architecture firm. It should be inspired by playful postmodern design, with a bright and energetic look. Use bold color combinations, simple geometric shapes, and a mix of clean and quirky details. The layout should still feel polished, with broad images that take up a lot of space.


#### Retro Futurism


> Create a website for an architecture firm. It should feel inspired by retro futurism, with a bold and slightly cinematic look. Use dark neutrals, metallic tones, and a few bright electric colors. The layout can feel more experimental, with broad images that take up a lot of space.


A year ago, no matter what you asked for, AI would make something like this:


## How to make better websites with AI


AI still makes generic websites when you give it generic instructions. The difference now is that it can actually follow a strong design direction.


The quality of the result depends much more on the direction you give it. A specific visual style, described clearly and paired with one of the newest models, can produce something genuinely distinctive even if you aren't a designer.


That wasn't true six months ago.
