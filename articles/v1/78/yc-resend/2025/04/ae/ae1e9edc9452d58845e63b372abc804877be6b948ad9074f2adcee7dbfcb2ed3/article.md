---
schema_version: "1.0.0"
document_id: "ae1e9edc9452d58845e63b372abc804877be6b948ad9074f2adcee7dbfcb2ed3"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/launch-week-behind-the-scenes"
published_at: "2025-04-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:58:06.165020+00:00"
content_hash: "sha256:378f73eb5cbe15275ef193beafd9dc1c812e8d5596346e960f00447bef81517d"
---

# Launch Week: Behind the Scenes

We just finished our **fourth launch week** .


Launch Weeks are the big peaks in our[Heartbeat Framework](https://resend.com/handbook/marketing/how-we-keep-momentum) . It's when we launch 5 new features in 5 days.


They create buzz and excitement around the product, but require a lot of planning and coordination across the team. As the biggest "sign of life", launch weeks are often the **most impactful way we share our momentum** .


Today, we wanted to pull back the curtain and share some of the details of how we run our launch weeks.


## Prioritizing the Right Products


We're constantly adding new features to Resend. The challenge is to pick the right ones to reveal during a launch week.


Over time, we've learned that ideal launch week products should be:


- **Large-impact** : features that impact a large set of users
- **Visual** : features that are especially visual often capture the most attention
- **Requestable** : either features many users have requested or features that solve a common pain point


During our[fourth launch week](https://resend.com/blog/resend-forward-4-wrap-up) , we launched 5 new features:


- Day 1:[new.email Public Launch](https://resend.com/blog/new-email-public-launch)
- Day 2:[Multiple Teams](https://resend.com/blog/multiple-teams)
- Day 3:[Broadcast API](https://resend.com/blog/broadcast-api)
- Day 4:[Multiplayer Editor](https://resend.com/blog/multiplayer-editor)
- Day 5:[React Email 4.0](https://resend.com/blog/react-email-4)


## Choosing the Art Direction


For recent launch weeks, we've created a[micro website](https://resend.com/forward) with its own art direction. It started with[Zeh Fernandes](https://resend.com/humans/zeh-fernandes) walking through several themes in Figma.


We settled on **Illuminism as the art direction** , which is a philosophical and cultural movement that emerged in the 18th century. This movement advocates for reasoning and critical thinking, which we believed was a fitting theme for our first Generative AI launch.


### Illustration drafts


Once we identified a direction, we gathered inspiration and illustrated drafts.


### Finding an illustrator


[Zeh Fernandes](https://resend.com/humans/zeh-fernandes) began exploring the art community for potential illustrators that specialize in our direction. We often turn to designers we've done work with in the past, but for this launch week, we identified[Dalibass Design Studio](https://www.dalibassdesign.com/) as a great fit.


All illustrations, from sketches to final shading and shaping, were done in Adobe Illustrator using a Wacom Intuos Pro M drawing tablet and Wacom Pro Pen 2 along with some basic Adobe Illustrator tools such as Ellipse tool (for creating perfect circles for machines and mechanisms).


View the full process on Dalibass Design Studio's[Behance post](https://www.behance.net/gallery/222685525/Resend-Launch-Week-Cover-Illustrations) .


## Building, Testing, and Polishing


Launch weeks often require weeks or months to build.


For this reason, we often build and soft-launch them behind feature flags. When the feature has been requested by users, we track those contacts alongside the feature in Linear. As we move closer to launch, we contact those users and ask them to help us test and refine the feature.


Not only does this approach help us build with real user feedback, but it also means on launch day we can enable the feature for 100% of users with confidence since we have a rollback plan in place.


### Announcing the Launch Week


A week before launch, we tweet out a preview of the launch week and encourage users to sign up for the waitlist.


We created a double opt-in flow that required users to confirm their email address before being added to the waitlist. Double opt-in both ensures we have a valid email address on our list and also protects domain reputation (by preventing us from hard bouncing emails).


While we don't currently offer a double opt-in feature natively, you can roll your own without much effort ([example repo](https://github.com/resend/resend-double-opt-in-example) ).


### Daily Content


We create a daily content plan to promote the launch week.


- Blog post introducing the feature
- Social post promoting the launch week
- Video introducing the feature
- Email to our waitlist audience


This year, we recorded[videos for each feature](https://www.youtube.com/playlist?list=PL6QNrh9-4_AYtjEYNGl_JPfNAQGIIqKCm) during our offsite in February in preparation for launch. This was the first time we used videos and found it reached a broader audience, thanks in large part to the YouTube algorithm.


We kept the setup very simple, using a tripod, a basic mounted microphone, and natural lighting.


Importantly, when we post about each feature, we create a thread including all the details with accompanying media. It's important that people can capture the entire experience without having to click through to the website to read the blog post.


### Launch Week Learnings


Once the launch week is over, we track the results.


The goal of a launch week is to increase awareness and adoption of the new features, so we focus more on impressions and usage instead of revenue metrics.


Currently, we've seen a 45% increase in impressions over the previous launch week. Here's a breakdown of the results by day between the previous week and this launch week.


Day 1 Day 2 Day 3 Day 4 Day 5 Total


82% -484% -54% 78% 47% 45%


We also run a retrospective to discuss what went well and what we could improve.


## Conclusion


While launch weeks often require a lot of planning and coordination, the entire process is a great way to build momentum and excitement around the product and to collaborate together on a fun and creative project within the company.


We hope you enjoyed this behind-the-scenes look at how we run things.


See you in the next one.
