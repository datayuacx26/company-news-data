---
schema_version: "1.0.0"
document_id: "78d54fd36a57231b0c443cba621518801b51fc462264f74a05a35f25b7e39cd6"
company_key: "yc-mercoa"
company: "Mercoa"
source_id: "yc-mercoa-news-import-107a1845dd35"
canonical_url: "https://mercoa.com/blog/my-first-two-weeks-at-mercoa"
published_at: "2024-07-05T00:00:00+00:00"
first_seen_at: "2026-07-22T04:05:05.455716+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:71603d00f59069d31079ae647db335fa0f5e196a8288abb14097d3c8ae0d1523"
---

# My First Two Weeks at Mercoa: Growing with the Company

# My First Two Weeks at Mercoa: Growing with the Company


What I've learned from my first two weeks as a Product Engineer at Mercoa


Ashwin Kumar - July 5, 2024


On a Friday afternoon about a month ago, I got a call from the CEO of Mercoa telling me to hop on a plane to San Francisco.


In the whirlwind of a week that followed, I spent my time by:


-


Scrolling through online packing lists


-


Driving from Ohio to Ann Arbor to visit my friends from UMich


-


Stuffing a suitcase through the night before boarding my 5am flight to SF


(Fun fact: Debugging a car engine is a *lot* harder than debugging code!)


When I landed in SF the day before my start date, I had no idea what to expect. To be completely honest, I still feel like I’m getting used to the city on my third week here! However, I *have* grown rapidly over the past two weeks, as an engineer and as an individual. In this writeup, I’ll share my thoughts on what I’ve learned and how I’ve grown from my first two weeks at Mercoa.


## About Me


I’m[Ashwin](https://www.linkedin.com/in/ashwin-a-kumar/) , an incoming Master’s student studying CS at the University of Michigan. At Mercoa, I’m known as a Product Engineer.


While that title’s neat on paper, in practice it's a catch-all term for the classic “wearing many hats” startup line. I push code like a traditional software engineer would, but I also:


-


Record and edit demo videos


-


Release features and live demo them to customers


-


Run customer discovery calls


-


Fix and respond to customer issues


-


Write blog posts (new this week alongside “make the blog!”)


Mercoa is a small team: besides me, the CEO and CTO are the only two other employees in the company. As such, I doubt this'll still be an exhaustive list in another two weeks. However, working on a small team doesn’t just lengthen my list of responsibilities. It forces me to change how I view tasks, let go of the pursuit of perfection, and bias myself toward action.


## Ownership - Why am I writing this code?


Last summer, I had a great experience working as a SWE intern at Deepgram, a voice AI company based in Ann Arbor. There, I built an internal QA pipeline for testing releases before deployment with one click, and I learned a *ton* doing it. By the end of the summer, I gave a presentation about it to the company’s entire engineering division that I felt proud of.


That tool was used exclusively by one team of about six engineers.


Truthfully, this feels like the expected level of contribution from most internships. In retrospect, I feel lucky to have built a project that actually adds value instead of being sunsetted after the internship. I worked hard to make it the best it could be because I felt a sense of ownership over the project – but *why* did I feel that way? Because my manager said it was valuable.


In my first two weeks this summer, I already shipped two customer-facing features: Live Preview for Invoices and Invoice Line Item Classification.


On my first day, the team let me know that several customers were interested in helping their customers send invoices. The key requirement was previewing the invoice while creating it, and we found that Stripe had one of the best live previewing experiences. So, I took inspiration from them and others to launch[live previewing](https://www.linkedin.com/posts/ashwin-a-kumar_im-happy-to-announce-my-first-week-interning-activity-7210006233292898304-Ae19?utm_source=share&utm_medium=member_desktop) on week one.


The next week, a customer needed to close a deal with a client who processed invoices with hundreds of line items, and had to tag each line item by hand for accounting. That led to my[second project](https://www.linkedin.com/posts/ashwin-a-kumar_today-mercoa-came-alive-for-the-first-time-activity-7212497434235584512-B4em?utm_source=share&utm_medium=member_desktop) : an invoice line item classifier that uses AI to learn from a user’s invoice history and automatically tag line items for them!


I feel a *much stronger* sense of ownership over these projects because rather than building an intangibly useful *output* , I’m building toward a tangible customer *outcome* .


I’m not saying I hate working on tech debt or internal tooling – trust me, I’ve done plenty of that here too. However, I have developed a much more personal relationship with the things I build because the things I build directly help someone reach their goals.


This Monday, one of our customers pinged me on Slack to ask why my line item classifier wasn't working. So, the *first* thing I jumped to do Tuesday morning was spend an hour debugging the customer’s issue, even pausing my original task to fix it. I felt an emotional response because it’s *my* feature, and I want it to work because it’s *mine* .


## Anti-Perfectionism - Done is better than perfect


My stronger sense of ownership compels me to work harder to make the best possible thing, which sounds good on the surface. But, this also directly contradicts my next learning: “Done is better than perfect.”


It’s a pithy phrase I’ve heard hundreds of times by now, and I’m sure you have too. But, I discovered a new depth to the concept when I prepared my first public launch: the LinkedIn demo video for Live Previewing for AR.


I’ll be honest – I hated that video after I made it. After we recorded the final take and edited it, I sat down at my desk and stared at my screen with a sinking feeling in my stomach. I didn't like thinking about how I’d actually have to post it for all my friends to see. I couldn’t stop thinking about all the mistakes I made that I could’ve fixed if I’d done just *one more take* .


Then, I posted it anyway. Over the next hour my phone kept buzzing from encouraging comments from my friends, inbound traffic on my profile. The next week, I posted a second demo video for a different feature. This time, I got more traffic and even some texts from friends who noticed that my first post wasn’t a one-off thing.


If I had done more takes, I would’ve put in much more effort to create almost the same video, save for a few awkward pauses and stumbled-over words. How would the final public response have differed? It wouldn’t have. LinkedIn tells me that the average view got through about 16 seconds of my 2 minute demo video. The real value-add was not posting something perfect – it was posting *something* .


This led to one of my biggest insights over the past two weeks: ship things you’re uncomfortable with.


I’ve heard a lot of senior people speak about how they look back on their younger selves. They talk about how they made a bunch of mistakes even though they naively thought they weren't at the time. This isn’t the whole story. I knew I was making a bunch of mistakes *in the present* , and I didn’t feel proud of my final product right before I posted it. The mini-superpower I unlocked was feeling that discomfort and shipping it *anyway* .


## Bias for Action - Ship, ship, ship!


This is the least surprising subheading you’ll read in a blog post written by someone working at a three person YC startup. For me, it has also been the most impactful.


I learn best by doing, and I find shipping things has a compounding effect. By completing one task, I learn some amount. This makes me a more efficient engineer, which means I can complete the next task faster. But, I also *learn more than I would’ve before* in doing so, and this cycle accelerates with each task I complete.


On top of the two public-facing launches I’ve had, I’ve also completed four other projects of varying sizes. As of this week, I've built two new internal tools, another customer-facing demo video, and the blog you’re reading this post on. I'm still far from great at executing these projects, but I'm also better at each new project I've worked on. I can already observe my own compounding, and I'll continue to as long as I maintain my bias for action.


## The Future


The part I find most interesting about that last point? None of it was new to me, and I suspect the same is true for you. I’ve understood compounding and felt its effects well before Mercoa. That’s how I did well in school, learned programming growing up, and grew during my undergrad at UMich.


So, my high-level plan for the future is simple – keep compounding.


Unfortunately, getting more specific requires much more foresight than I have. I’m writing this blog post on about two to three weeks of experience, and I’ve distilled some solid takeaways already. But, there’s still much more value I can distill over the rest of my time working here. This is the beginning of that journey, and I don’t know where it’ll go yet.


With that in mind, I’d like to invite your feedback as well! I plan to continue publicly releasing things, and I want to know what you think – so tell me! If you have things you liked about this post, things you disliked, or general thoughts you'd like to share, feel free to contact me atashwin@mercoa.com . Thanks for reading!
