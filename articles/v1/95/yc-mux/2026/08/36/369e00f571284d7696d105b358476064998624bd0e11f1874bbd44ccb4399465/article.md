---
schema_version: "1.0.0"
document_id: "369e00f571284d7696d105b358476064998624bd0e11f1874bbd44ccb4399465"
company_key: "yc-mux"
company: "Mux"
source_id: "yc-mux-atom-4708df60f240"
canonical_url: "https://www.mux.com/blog/is-this-video-mostly-of-feet-and-other-useful-things-you-might-want-to-ask-about-your-video"
published_at: "2026-08-18T22:22:34.865+00:00"
first_seen_at: "2026-08-19T00:08:06.632383+00:00"
fetched_at: "2026-08-19T00:08:08.198878+00:00"
content_hash: "sha256:8a3307d72ff25f54cb46d901336ee1484b56963331f17315aa412df94fb647f0"
---

# Is this video mostly of feet? (and other useful things you might want to ask about your video)

Published on


August 18, 2026


# Is this video mostly of feet? (and other useful things you might want to ask about your video)


By[Dylan Jhaveri](https://www.mux.com/team/dylan-jhaveri) • 6 min read •[Product](https://www.mux.com/blog/category/product)


---


This is one of the real questions we ask in our content moderation flow on[stream.new](https://stream.new/) – more on that later. I’m going to surprise you with how many uses there are for the simple capability of “asking a question” to a video. The low hanging fruit and obvious ways to use the[Ask Questions workflow](https://www.mux.com/docs/guides/robots-ask-questions) in Mux Robots is for content moderation.


But there are many more interesting and high-leverage ways that customers have started using this feature. Let’s dive in.


## Questions you might have for your videos


### Ask Questions for extra content moderation


This is super powerful in conjunction with the Mux Robots Moderation workflow, which tells you the scores for violent


and sexual


content. And where the title of this post comes from.


> Is this video mostly of feet?


It’s a yes/no question, and for[stream.new](https://stream.new/) we have to ask it because videos that are mostly of feet don’t trip the sexual


content filter. However, sometimes this content falls on the wrong side of the line for things we allow to be uploaded and streamed for free on that app, so we have to find a way to disallow it.


Without the simple Ask Questions workflow, this would be quite difficult to catch.


### Listing completeness and accuracy


Let’s say you are a vacation home rental company, and you want every video user to upload a listing to include the front entrance, so people can use it to identify the right property.


> Does this video show the front entrance to a rental property?


Or you have a marketplace where sellers upload videos of the item they are selling, and the number one complaint you get from buyers is that the video wasn’t even the actual item they purchased.


> Does this video show a physical item being handled or rotated by a person (as opposed to stock footage or photos of a screen)?


### Required disclosure


If you help creators make advertising videos for brand deals, and you require them to disclose up front that it’s an ad, you can ask that.


> Does the video state the required ad disclaimer at the beginning of the video?


### Proof of condition / proof of work


If you're a field service platform (HVAC, solar, appliance repair) and technicians upload a video when they close out a job.


> Does this video show the installed unit powered on and running?


The same idea can work for equipment rental returns, moving companies, and property management move-out documentation.


### Is this actually a video?


Podcast platforms, course platforms, and UGC apps all get this one.


> Is this video a static image or slideshow with audio over it?


### Support team routing


Let’s say you make a product that contains both a physical hardware component and a software component, and users send you videos when they open up support tickets. You can triage and route the ticket to the right support team with a question like:


> What does this video show
> - screen recording of software
> - video of physical device
> - other


## You’ll notice that Ask Questions is multi-modal


One thing you always have to ask yourself is: what context did the model have to know this information? And you, a discerning reader, will have noticed in the questions above that some of them can be easily answered purely by the transcript of the video, like “Does this video state the required ad disclaimer at the beginning of the video?”


And in fact, you may see other “video intelligence” out there that is really just “video transcript intelligence”.


But that is rarely good enough. With a transcript-only approach you’re missing tons of information. That’s why Ask Questions generates answers based on the transcript and the video frames.


What better way to demonstrate this capability than with the tried-and-true Big Buck Bunny? I’ve been avoiding this video for years because it’s ALWAYS the example, but for this multi-modalness of Ask Questions, it’s the perfect example because there is no dialogue. Let’s put it to the test.


*© copyright 2008, Blender Foundation / www.bigbuckbunny.org*


**Does anyone speak in this video?**


- answer: "no"
- Confidence: 1
- Reasoning: The video appears to be an animated silent short film with no dialogue or audible speech.


**How many animals bully the main character? (options: one, two, three, four or more)**


- answer: "three"
- Confidence: 0.9
- Reasoning: There are three distinct animals shown bullying the rabbit: a squirrel, a chipmunk, and a larger rodent-like creature.


**Is there violence in this video?**


- answer: "yes"
- Confidence: 0.9
- Reasoning: The video contains slapstick violence where the characters are subjected to traps, falls, and various comedic physical mishaps.


**Is this video appropriate for children under 10?**


- answer: "yes"
- Confidence: 0.9
- Reasoning: The violence is of a mild, cartoonish, slapstick nature typically found in children's animation, which is generally considered appropriate for that age group.


**Does the main character get revenge on his bullies?**


- answer: "yes"
- Confidence: 0.9
- Reasoning: The video shows the rabbit setting up traps that result in the bullies getting caught or hurt by the end of the sequence.


**What programming language is shown in this video?**


- answer: null
- Skipped: true
- Confidence: 0
- Reasoning: The video is an animated short and contains no programming code.


Nonsensical questions, or questions that can’t be answered with a reasonable degree of confidence, get skipped, which we find to be better than guessing or hallucinating.


With Big Buck Bunny, every answer had to come from the pixels — there are no words in this video to transcribe. And notice it's not just naming objects in frames: it counted the three bullies, and when we asked "Does the main character get revenge on his bullies?" it said yes, because it followed a ten-minute plot to its conclusion.


## Written By


### [Dylan Jhaveri – Director of Self Service](https://www.mux.com/team/dylan-jhaveri)


Software Engineer and cold water surfer. Previously startup co-founder. Trying to find the best cheeseburger in San Francisco.


## Leave your wallet
where it is


No credit card required to get started.


[Sign up Sign up](https://dashboard.mux.com/signup)
