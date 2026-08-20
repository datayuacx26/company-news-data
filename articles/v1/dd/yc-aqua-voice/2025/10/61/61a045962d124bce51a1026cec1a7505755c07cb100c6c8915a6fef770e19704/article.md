---
schema_version: "1.0.0"
document_id: "61a045962d124bce51a1026cec1a7505755c07cb100c6c8915a6fef770e19704"
company_key: "yc-aqua-voice"
company: "Aqua Voice"
source_id: "yc-aqua-voice-news-import-16e95c4a8dc7"
canonical_url: "https://aquavoice.com/blog/vibe-coding-showdown-2025-base44-vs-replit-vs-bolt-vs-loveable-vs-rork-vs-v0"
published_at: "2025-10-30T00:00:00+00:00"
first_seen_at: "2026-07-24T07:27:24.141849+00:00"
fetched_at: "2026-07-28T21:59:41.762292+00:00"
content_hash: "sha256:aee223bc7354712b3a3382984ce222ec20066c83935e875c000245b638f503ba"
---

# Vibe Coding Showdown (2025): Base 44 vs. Replit vs. v0 vs. Bolt vs. Lovable vs. Rork

TLDR: Comprehensive test of the best vibe coding for real world use. Here are my results, broken down by category with winners at the end.


A lot of prompt-to-app tools demo great. The question is: can they build apps that actually work? The answer for most was **no** .


Here's the video version:


Scorecard


Tool


Auth


Posts


Images


Comments


Likes


DB


Frontend


Base44


✅


✅


✅


✅


✅


✅


Good


Replit


✅


✅


✅


❌


✅


✅


Okay


Bolt


✅


❌


❌


❌


❌


⚠️


Okay


Lovable


✅


✅


❌


❌


✅


⚠️


Weak


Vercel v0


❌


❌


❌


❌


❌


❌


Weak


Rork


⚠️


⚠️


❌


⚠️


✅


❌


Pretty


I did all this testing on October 27th, 2025.


Skip to the end to see the **Winners** .


## The Problem with AI Agents and "Vibe Coding"


The basic problem with all of these highly automated prompt-to-app tools is more than 90% of the time, the app just doesn't work at all.


About the only thing I've found the tools to be able to reliably do is make blogs or front-end only sites.


If I look at my history on Replit or Bolt or Lovable, I see a list of projects that failed to get off the ground.


This is a huge bummer, because one of the promises of the AI age is that it's supposed to be the golden age of the idea guy.


Here's a small sample of things that spent serious time and money trying to get work on Replit and Loveable that are currently pet rocks:


-


A physically accurate tide simulator showing Earth and Moon positions


-


A token on Solana called Zhom


-


A 2d missile defense simulation game


-


A bank backend in COBOL


-


A hacker news clone


-


A Factorio blueprint editor


-


A site tracking if Highway 1 is open (this kinda[worked](https://highway-one-open-yet.lovable.app/?utm_source=lovable-editor) )


-


A web version of Verdy du Vernois, 1876 — Beitrag zum Kriegsspiel


But before we get to the above, I think the rite of passage for any competent coding agent is to build a **functional social media site** . I could have picked Reddit, Hacker News, Instagram, TikTok, but I think **cloning Tumblr** is as good as any of those, and if it works I can imagine having more fun with it.


## The Test


I asked each tools to "build Tumblr". The features are well known. You need to be able to **sign up** . You need to be able to **post text, images, quotes, etc.** . You need to be able to follow different people's blogs. You need to have likes, reblogs, and a comment system.


Here's the prompt I used:


> Build a comprehensive web application that meticulously replicates Tumblr's entire functionality and user experience, aiming for a modern 2025, view-for-view clone. This should include core features such as user account creation and management, posting capabilities with multimedia support (images, videos, text), following and interaction systems (likes, reblogs, comments), notifications, and dashboard management. Implement the necessary frontend components, backend architecture, database schemas, and APIs required to achieve feature parity with Tumblr. Additionally, ensure responsive design, scalability, and security best practices so the clone is robust and user-friendly across multiple devices and user loads. Design should be mobile first but responsive and above all simple and minimal.


I used[Aqua Voice](https://aquavoice.com/) to dictate the prompts and the follow-ups, which you can try for free[here](https://aquavoice.com/download) . I highly recommend the "voice prompting" workflow to anyone who hasn't tried it yet.


Tools Used:


-


[Lovable](https://lovable.dev/) -[https://microblog-echo.lovable.app](https://microblog-echo.lovable.app/)


-


[Bolt](https://bolt.new/) -[https://tumblr-clone-web-app-jik5.bolt.host](https://tumblr-clone-web-app-jik5.bolt.host/)


-


[base44](https://base44.com/) -[https://reblogly-8233da62.base44.app/dashboard](https://reblogly-8233da62.base44.app/dashboard)


-


[Vercel v0](https://v0.app/) - Didn’t work


-


[Replit Agent v3](https://replit.com/~) -[https://tumblr-clone-2025-downtownfbrown.replit.app](https://tumblr-clone-2025-downtownfbrown.replit.app/)


-


[Rork](https://rork.com/) -[https://tumblr-2025-clone.rork.app/](https://tumblr-2025-clone.rork.app/)


You can check out the published versions of what each tool produced.


I'm a big fan of[Simon Willison's blog](https://simonwillison.net/series/gpt-5/) and his now famous test of asking models to create an SVG of a pelican riding a bicycle.


The pelican test isn't a comprehensive benchmark, but it's surprisingly high signal for how simple it is.


I'm hoping, "clone Tumblr," can become something like that for zero to one coding tools.


Here are the results.


## [Replit](https://replit.com/) (Agent v3)


Replit Agent v3 is very flexible. It takes a while to run (18 minutes in my case), but was second only to Base44 in terms of out-of-the-box functionality. When it was done, I had a functioning app with Replit auth, real file uploads that worked on the first try, text posts, and likes.


My two gripes were:


1.


the design kind of sucked


2.


the agent didn't implement comments and just put, "coming soon..."


Verdict: If you want control over your stack and are fine waiting a bit longer, Replit is a solid choice and probably the correct choice if TypeScript React node isn't ideal.


## [Bolt](https://bolt.new/)


Bolt gave me a login screen and got me signed in, but the minute I tried to post I got a database error. The design wasn’t anything too special, but the deal-breaker was backend reliability.


Worse, when I pushed for images, the tool defaulted to a “paste an image URL” pattern, which is a cheat. Tumblr doesn’t ask for a link to an image that already lives somewhere else. You have to hack around things that with bolt, and it's very hard to go from 0 to 1. In fairness, I could probably fix the DB errors with ten minutes more prompting, but the whole point of this test was out-of-the-box performance. Bolt failed that.


Verdict: So So at frontend, but annoying backend errors.


## [Base 44](https://base44.com/)


I'd never used Base44 before, but it worked better out of the box than any of the other tools. It had functional auth, text posts, image uploads, likes, and comments. The core stuff was all there.


The only gripe I have with it is it built me something a bit closer to Twitter or Instagram than Tumblr, but there's a lot of overlap and the design was functional if a little bit uninspired.


Base 44 seems to provide backend primitives to the model which avoid having to set up things like a Postgres database schema from scratch. That bet seems to have paid off, and while it might make scaling out more difficult and does mean some platform lock-in, in my mind the tradeoff is more than worth it for functional code.


Verdict: Clear winner, most functional out of the box.


## [Vercel v0](https://v0.app/)


I tried v0 back when it launched as a front-end only tool. It's since become a full prompt to app platform with super base integrations and all that. The interface was very pretty, but the code didn't work at all.


First, a migration script wouldn't run, and after clicking the fix it button a few times, I made it to a signup screen that was broken and the model couldn't get it working after three tries.


Verdict: great vibe coding UI, that didn't work at all. Pet Rock.


## [Lovable](https://lovable.dev/)


I went into the test expecting Lovable to win. I enabled their cloud backend, which is supposed to make backend tasks less brittle. Auth worked out of the box, so did text, posts, and likes. When it came to uploading images, it wanted a URL instead of an image file, which was disappointing given that image uploading is kind of the core feature.


The UI was also pretty terrible, which was surprising. For previous tasks, I'd found Lovable pretty good at frontend.


**Verdict:** maybe a bit better than Bolt, but only marginally. Most things didn't work.


## [Rork](https://rork.com/)


Rork made a strong first impression with a clean UI, both for the Vibe Coder and for the Tumblr clone. It had by far the best front end, but the backend was a bit of a dumpster fire.


After enabling "backend", I still couldn't get a persistent account system, persistent likes, working posts, or image uploads.


Verdict: Best frontend by far, but it stayed non-functional. We call this Boomer Mode ( treating non-functional mockups of an app as the app itself)


# Winners


Best Overall 🏆: **Base 44** (most working stuff out of the box)
Runner Up 2️⃣: **Replit Agent** (lame design but pretty good functionality)
Best Frontend 🦋: **Rork** (great looking UI, but couldn't get it hooked up to anything)


## Why is this on the Aqua Voice Blog?


Aqua's mission is to make voice a first-class input method for computers. The more things that can be done with text prompts, the more powerful voice is as a tool. We've put a ton of time and resources into optimizing the Aqua clients for Mac and Windows to be fast, contextually aware, easy to use, and great for technical speech.


The reason we decided to get in the[model training game](https://aquavoice.com/blog/introducing-avalon) was to get better at technical terms that other models were ignoring. Avalon is the only ASR model on the planet that can transcribe` Supabase` correctly.


But in order for voice to be a "dream interface," the AI agents have to hold up their end of the bargain. if we're honest, many of them do not. As Karpathy pointed out in his[recent Dwarkesh appearance](https://youtu.be/lXUZvyajciY?si=HQ_BaW4s2W3q9AZO&t=1834) , many agent implementations are way over their skis, promising a lot more than they can deliver.


We want Aqua users, most of whom are AI early adopters, to use tools that will actually work. So they can go from speech → prompt → working application, with minimum AI babysitting.


Yes, I actually tested all of these and wrote this myself.


### Learn More About Aqua


You can learn more about how Aqua integrates with Cursor, VS Code, Claude Code, and other AI tools in our[User Guide](https://aquavoice.com/guide) . We just launched a highly requested feature,[File Tagging](https://aquavoice.com/guide/file-tagging) , which automatically pulls in relevant files when dictating prompts in Cursor, Windsurf, and other VS Code-based applications. You can see more details about that here
