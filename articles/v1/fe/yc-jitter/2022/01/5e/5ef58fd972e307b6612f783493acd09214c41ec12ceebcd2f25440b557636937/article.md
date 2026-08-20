---
schema_version: "1.0.0"
document_id: "5ef58fd972e307b6612f783493acd09214c41ec12ceebcd2f25440b557636937"
company_key: "yc-jitter"
company: "Jitter"
source_id: "yc-jitter-rss-f185ab33cc65"
canonical_url: "https://blog.jitter.video/product-hunt-launch/"
published_at: "2022-01-28T15:13:03+00:00"
first_seen_at: "2026-07-25T10:15:39.360550+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:2ac9671fdabf7dc167f29bfd4996a170efb55df8a6065afccfeb640ef2779c85"
---

# How our Product Hunt launch helped us iterate fast to find product-market fit

(Article originally published on the Product Hunt Blog)


Earlier this year, Jitter launched and landed at #2 product of the day and #4 of the week with over 1,800 upvotes. Maker Sébastien Robaszkiewicz (Robi) shared the story behind the successful launch.


## Finding our 10+ year product


Before we got into Y Combinator (Winter 2020) in early 2020, we had just discarded our 3 previous ideas. What we knew was that we wanted to make something that would give people creative superpowers. We tried a simple animated social post editor, moved to an automatic design tool for ready-to-post for graphics, and also created a Notion product for super-fast presentations. Yet, we hadn’t found something that clicked. On the product side, we didn’t feel great about creating something that simply helped people produce more ads. Beyond that, we couldn’t envision any of these products evolving into a real company, with customers at scale. We just hadn't found yet something that we felt deeply passionate about — that we'd be willing to commit to for 10+ years.Fortunately, YC didn’t see that as a problem. One of the partners even told us that he thought the fact that we had launched three products over just a few weeks showed that we were good builders (fast iteration is typically seen as a good thing by YC). Nonetheless, the previous experiments we launched taught us that people really enjoyed playing with animations.


### From idea to launch in a week


For our 4th product idea, we decided to build a fun and simple app that would enable people to animate text easily. The idea was super simple:


1. Users enter some text
2. We generate a few animated templates for them
3. They can export the creation as a GIF or a videoTo get answers fast, we wanted to build the app in less than a week.We both have a background in Javascript, so we decided to make a web app: Étienne laid the foundations of the animation and rendering engine (we chose to use canvas2D to build it even faster), while I, Robi, started to code a few animated text templates.Five days in, and we had a convincing demo that was fun to use. We used the last two days to set up a payment system with Stripe (we wanted to test if people were ready to pay for such a service), install Intercom to talk to users, and prepare the Product Hunt launch material.


### From launch to starting over


We launched the app,[SnackThis Text Animator](https://www.producthunt.com/posts/snackthis-text-animator?ref=blog.jitter.video) , on Product Hunt and immediately got some amazing feedback from the community. We learned a lot by engaging in conversations in the comments and finally ended up #2 Product of the Day. It brought us a lot of visibility, and a lot of users to talk to.


SnackThisWe used the following days to understand what users were doing with the tool, and what other features they wanted. The two main demands came pretty clear: they wanted more templates and more customization capabilities.We spent the following weeks shipping more features (custom fonts, custom colors, etc.) and producing a few more templates.That’s when we realized: the product wouldn’t scale. How could our team of two keep up with the demand for new templates every day, while making the tool more capable?We had to go back to the drawing board.


## Solving the equation


Our problem was interesting. We launched a tool that people used (and paid for), but people wanted more templates and we didn't have the time to build them.We brainstormed lots of ideas to solve this issue. We could, for instance, collaborate with freelancers to create more templates, or grow the team with more creative coders. Or, build our own internal tool to create the templates visually.But wait, if we needed such a tool, wouldn't other people need it too? We dug in deeper and went back to our Text Animator users, especially those who needed more customization capabilities. They told us they wanted to have more layers in their creations (images, shapes, logos…), to customize the animation parameters, and to collaborate with their team.We took some time to make sense of all that information until we could reframe clearly what people needed: a tool like Figma, but for motion design.


### Launching Jitter, the modern animation tool


Fast forward a few months and we launched the beta version of[Jitter](https://www.producthunt.com/posts/jitter-beta?ref=blog.jitter.video) on Product Hunt, one year after the Text Animator. Once again, launching early on Product Hunt has proven to be extremely valuable for us. As our friends at veed.io put it: the consequences of launching a product just a few months later can be incredibly costly. We probably could have launched even earlier.


The cost of launching late. Credit: veed.io.


We'd love to share some advice to make the most out of your launch. We're not experts (and luck plays a part, too), but here's what worked for us:


- Get prepared a few days earlier to[create a great Product Hunt page](https://blog.producthunt.com/?ref=blog.jitter.video) .
- Get some beta testers involved beforehand to support you when you launch. If they use your product and find it useful, they’ll probably want to help you – but they don’t owe you anything, so don’t be pushy :)
- Make your product as easy to try as possible. You'll get much more interesting feedback if PH users can actually try the product than if they can only watch a video demo on YouTube – bonus points if you can try the product without even signing up!
- Be genuinely interested in getting feedback from the community – every comment is a great way to start an insightful conversationWe hope this can help you to make your next launch as insightful as possible! 🚀 💫


## Looking forward


Jitter


We’ve been using the past 8 months to listen to our users, and build the features that help them create professional-looking animations for videos, interfaces, social posts, and communication content even faster: animation presets,[Figma plugin](https://www.producthunt.com/posts/jitter-plugin-for-figma?ref=blog.jitter.video) , audio support… We are thrilled to have a fast-growing user base, and we want to support it as much as we can by:


- Building the best motion design tool (we have lots of new exciting features in the pipe and we can’t wait to roll them out!)
- Providing the best resources for aspiring and experienced motion designers (we just launched[Jitter Learn](https://www.youtube.com/c/jittervideo?ref=blog.jitter.video) on our YouTube channel)
- Encouraging the community to build on top of each other’s work (new community and collaboration features coming soon!)
