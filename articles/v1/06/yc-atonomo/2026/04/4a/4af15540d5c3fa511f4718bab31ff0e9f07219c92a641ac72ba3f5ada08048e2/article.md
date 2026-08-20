---
schema_version: "1.0.0"
document_id: "4af15540d5c3fa511f4718bab31ff0e9f07219c92a641ac72ba3f5ada08048e2"
company_key: "yc-atonomo"
company: "Atonomo"
source_id: "yc-atonomo-news-import-1e84588cb145"
canonical_url: "https://www.codecanary.ai/blog/codecanary-featured-on-fondo-start-with-david-phillips"
published_at: "2026-04-17T00:00:00+00:00"
first_seen_at: "2026-07-21T08:26:03.097679+00:00"
fetched_at: "2026-07-28T21:56:44.009033+00:00"
content_hash: "sha256:870facbe4e0bb40b1c3d14384374c3e7ef618452092bb84aea2bbbd2e36cd71e"
---

# CodeCanary featured on Fondo START with David Phillips

Michael Egan, the CEO of CodeCanary, was recently invited to talk about fixing bugs automatically with AI on[the Fondo START podcast](https://podcasts.apple.com/ca/podcast/start/id1790754910) hosted by David Phillips, the Fondo CEO. Here's a transcript of the interview:


**David:** All right, welcome back folks. We've got Michael Egan, the CEO and co-founder of CodeCanary.


**Michael:** Thanks for having me.


**David:** Welcome to the show and congrats on the launch.


**Michael:** Thank you, very exciting.


**David:** Yeah, tell us a little bit more about who you are and what you're building.


**Michael:** Yeah, I'm Michael. We're building CodeCanary. We connect AI agents to your product analytics and codebase to always be trying to ship a new improvement to your product. So[we watch session replays](https://www.codecanary.ai/blog/watching-session-replays-with-ai) , we ingest your conversion funnels and we look for bugs, we look for UX friction, we look for opportunities to improve activation and conversion. And when we find something, we create a pull request with a fix, hopefully trying to improve it. You can also run A/B tests just to make sure.


**David:** I mean, this is crazy. So everything from identifying bugs to fixing the bugs and shipping the fix.


**Michael:** Yeah.


**David:** How quickly does this happen? How does this work? So if we, you know, I think we need to sign Fondo up for CodeCanary. So let's say I sign up Fondo, I connect our repo. What does onboarding look like?


**Michael:** You connect, you add our GitHub app, you add our Slack app and you set up your PostHog. We will then backfill it with a few days of PostHog recordings and then the next morning you should wake up to pull requests, wake up to improvements. It runs at about 4:00 AM and so the next day it will have identified however you configured it. It's customizable based on what exactly you want to look for, how aggressive you want to be. And based on that, it sends you suggestions the next day.


**David:** Wow. And how long does it take for CodeCanary to like understand your codebase from when you connect the GitHub app?


**Michael:** It's really fast. It can do it almost immediately. The biggest issue is giving it time to ingest all the session replays. What we try to avoid is making snap decisions off of single things we see. So we try to feed it a good number of session replays before it says, "I think you actually do have a bug," and try to really validate what it's saying because it still will take your engineer's time when you suggest something. So we want to make sure we're suggesting things that are high quality.


**David:** Wow. And so you did Y Combinator. And is this a pivot or evolution?


**Michael:** This is definitely a pretty dramatic pivot. And so we spent a few years building enterprise mapping software.


**David:** Tell us about that.


**Michael:**[We were in the batch in summer 22](https://www.codecanary.ai/blog/codecanary-is-a-ycombinator-funded-company) before the magic of LLMs. And what we decided to do was build custom AI models for enterprise mapping. So we sold to governments, national and state governments. We sold to engineering firms and mining firms. And we had this business where most of our money actually came from small subscriptions from individual professionals, but most of our time was spent trying to sell to enterprises. And so we had this situation where we kept asking people to build something that would just let us improve our self-service product more quickly and just run the A/B test, fix things in that flow. And no one would do it. And so eventually we decided just to do it ourselves. And I think later today or maybe next week we're going to be announcing that business is being run entirely by our agent. So we can see what's happening in a real SaaS business operated by CodeCanary.


**David:** Wow. So agents are taking over.


**Michael:** In this case, yes.


**David:** They're running your previous startup. Now your new startup. This is really exciting because there is, I mean, companies like PostHog have blown up. I mean PostHog is probably the number one out there. Shout out PostHog, supporter of the show. We use PostHog as well. And there's just tons of session replays out there and they're just kind of sitting on a shelf with most of these companies. And so yeah, what was the world like before CodeCanary and how much better is it now?


**Michael:** I think the world before CodeCanary was you would set up session replays and you would watch it when you had a chance. But in reality, everyone wanted to watch them and didn't. Ourselves included. And they're very valuable. It contains more or less all the information about what your users are doing. But it would take more time than there is in the day to actually ingest it all. But now with CodeCanary, we can ingest it and create the actionables for you. And you can still go in and watch them. We'll send you a link to the session replay where something interesting happened. But it does all the filtering because 99% of the time in a session replay it's not interesting. It's just your app working as expected. But finding that 1%...


**David:** Hopefully.


**Michael:** That's true. If you get in and it's immediately not, you probably have a bigger fish to fry. But CodeCanary can fix it. Exactly. But yeah, now we surface that 1% of interesting material and get you a head start on the fix.


**David:** Wow. And you're used by B2B companies and B2C. What are you most excited about that you're seeing your customers doing?


**Michael:** It's hard to say. I think what we've realized is when you have something as general as ingesting product analytics and shipping PRs, it can do so much stuff. I think for me personally what's most interesting is this autonomous A/B testing because it's so just attributable to money and revenue. And it finds something, it ships a test, and either you're now forever making more money or it can learn from that and ship a new test. And I think that's just a really good system to have in place for a startup where you always have something more relevant to be doing than a small A/B test on a part of the funnel. But those small improvements stack up over time so powerfully.


**David:** Totally. Changing your conversion rate pretty dramatically. So in this future now with CodeCanary and AI code gen, what does the future of engineering teams look like? Is this solving problems that we didn't have time for before? Now we can solve more problems. Is this replacing jobs? What do you see as the future of engineering teams?


**Michael:** I definitely think it's best as solving things that you need to solve but aren't your top priority. And this is why we wanted it. We weren't trying to automate my co-founder's engineering role away. He had a lot of stuff to do. But we had so many ideas about how to improve conversion, we had bugs that would come up, and this would block us from the actual important stuff that we cared about and that our customers cared most about, that are worth having a human engineering team really diving into and making sure something high quality comes out of that. So I definitely think it will just allow engineering teams to really focus on what matters most without having to worry about these smaller things that you really need to do but aren't the highest priority.


**David:** That's so exciting. So you launched last week. You have a new announcement coming out in the next day or so. Yeah, what are you most excited about for the future of CodeCanary?


**Michael:** I think what we're most excited about is honestly a future where we can automatically review pull requests. So right now you can, if you wanted to, set up CodeCanary to automatically just ship whatever it says to ship. And that's[what we're doing](https://www.codecanary.ai/blog/codecanary-uses-codecanary-to-improve-itself) . But I wouldn't recommend that. But as these AI tools get better and as AI code review gets better, we really are moving towards something where a user comes in your product, CodeCanary reviews what they did, comes up with something to change, and then these very impressive code review tools can get to a point where you have the confidence to just ship what it wants to improve. And then you really have kind of[closed a loop to infinite self-improvement](https://www.codecanary.ai/blog/recursive-self-improvement-is-possible-for-apps) . And with every new user your product gets a little bit better. And I think that will make companies much more powerful.


**David:** Pretty incredible. And the progress is amazing. And you mentioned the pivot. So for other companies, you know, we at Fondo, we pivoted two years after YC. How did you, you know, how do you advise other founders on like when to know when to pivot or how to approach that with their investors? Yeah, do you have any advice for founders?


**Michael:** I think from the investor side, we were lucky to have very supportive investors who we said we're confident in this and they said I trust you guys. For us, it was really just my co-founder and I took a walk and we just really felt that this was a much more exciting thing to work on. And it's nothing against mapping, I love maps. But it just felt so much more interesting and exciting. And really the battle you're fighting in a startup is not continuing to do it. And so if it would make you much more excited to work on it, I think that is when I think it's worth doing.


**David:** Yeah, amazing. Well congratulations on the launch and all the success. Where can we find you online? Dylan, I'm going to pull up codecanary.ai.


**Michael:** Yeah, you can find us on our website. We have some very new social media. CodeCanaryAI on Twitter, CodeCanary on LinkedIn. And most announcements come from my co-founder Brendan Ashworth as well as myself.


**David:** Amazing. Well thanks so much for coming on, Michael. And we got to book a demo.


**Michael:** All right, take care. Thank you.


You can find the START podcast on:


- [Spotify](https://open.spotify.com/show/1CHxZLMn9OqIMbuOKOkJ9j)
- [Fondo blog](https://fondo.com/blog)
- [Youtube](https://www.youtube.com/live/-tulZ8ORwy4?si=yZOsjndVnfvCNDqV&t=1879)
