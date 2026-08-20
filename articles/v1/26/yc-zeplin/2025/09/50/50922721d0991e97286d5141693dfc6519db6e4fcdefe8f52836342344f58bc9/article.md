---
schema_version: "1.0.0"
document_id: "50922721d0991e97286d5141693dfc6519db6e4fcdefe8f52836342344f58bc9"
company_key: "yc-zeplin"
company: "Zeplin"
source_id: "yc-zeplin-news-import-3304292ba89d"
canonical_url: "https://blog.zeplin.io/product-news/ai-design-review-and-ai-organize/"
published_at: "2025-09-09T18:03:00+00:00"
first_seen_at: "2026-07-22T21:02:03.888972+00:00"
fetched_at: "2026-07-28T21:59:45.283870+00:00"
content_hash: "sha256:bbbe81079a36ca3847b0e6e3ed312b6b7ead16bb8fb0f6151a6613e9c71db4df"
---

# Introducing AI Design Review & Organize — AI that actually helps

Over the past few months, we’ve been exploring how AI could actually help Zeplin users. A lot of AI features out there get added just for the buzz, and they end up feeling off. We wanted to avoid that. If we’re going to bring AI into Zeplin, it has to be genuinely useful for Zepliners.


We kicked things off[during our hackathon](https://blog.zeplin.io/life-at-zeplin/zeplin-ai-hackathon/) , experimenting with ideas that could make a real difference in our workflow. What started as experiments turned into features we use ourselves every day.


And today, we’re excited to introduce **AI Design Review** and **AI Organize** — now available in public beta! Two new features that take the friction out of reviewing and organizing designs.


Let's dive in.


## AI Design Review: An extra set of eyes to make sure your designs are dev-ready


The review process in Zeplin is a little different from the one in design tools. In Zeplin, reviews are focused on a very specific moment: are my designs ready to share with devs, and are they truly ready to be built?


That’s the lens we designed AI Design Review with. **AI Design Review won’t comment on UX choices** like “this flow isn’t clear” or “use a toggle instead of a radio button.” We believe those decisions belong in your design tool. Instead, **Zeplin’s review is more tactical, focusing on the details that matter *right before sharing designs with devs*** — things like:


- Did you use an existing component?
- Are your designs accessible?
- Are paddings and margins consistent? Did you follow your spacing tokens?
- Did you use any off-system colors?
- Any typos or inconsistent casing in your copy?


Your design peers may be too busy to notice that “this spacing is 11px but we use an 8px grid,” but AI has plenty of patience for that. With AI Design Review, you get a checklist of potential issues to fix before sharing with devs.


🍿[Watch demo →](https://www.youtube.com/watch?v=zfUBkklm6yw)


Currently, AI Design Review checks against **7 categories** :


- **Spelling and grammar:** Grammar issues, typos, inconsistent casing.
- **Layout:** Alignment issues or inconsistent positioning.
- **Accessibility:** Color contrast issues (AA or AAA).
- **Color, text style, spacing tokens, and components:** If you’re using a Zeplin Styleguide, AI will flag mismatches, token issues, or detached components you could clean up.


☝️ If you’re a Figma user, creating your Styleguide is super easy — just paste your Figma library link using our[Styleguide Sync](https://support.zeplin.io/en/articles/8895043-syncing-your-figma-library) feature.


### Making it yours


Of course, sometimes AI flags things you already know about but don’t want to fix right now. For those cases, we added the ability to ignore, delete, or adjust issues.


For example, we had to teach it: “Ignore the word Zeplin — don’t suggest Zeppelin.” (Sad, right? 🤦‍♀️)


### What else should AI review for you?


During beta, we’ll keep refining the review categories and adding new ones based on your feedback. Got an idea? Let us know!


## AI Organize: Tidy up your screens, instantly


One of Zeplin’s promises is to provide a structured workspace where your team can easily find what they need — without digging through design files, pages and canvas.


To get there, designers usually need to do some groundwork to tidy up their projects. That’s exactly the kind of repetitive task AI can help with.


🍿[Watch demo →](https://www.youtube.com/watch?v=8ABIOxbTYs0)


With **AI Organize** , you can select screens and let AI suggest a clean structure. Of course, we don’t just leave it to AI, we guide it with helpful context like:


- Screen, file and page names
- Frame positions in the design file
- Your existing project structure


We also guide it with our know-how about when to create sections, subsections or variants. AI Organize is especially handy for newly exported screens, assuming your existing projects are already well organized.


### AI gives you a starting point


AI isn’t perfect and sometimes has hiccups, but it gives you a strong starting point. To make adjustments easier, we’ve added some handy features: **after running AI Organize** , you can still reorder screens, sections, or variants directly in the suggestion view before committing.


We’ll keep making AI Organize better during the beta — let us know how it’s working for you.


## During beta, AI features will work within existing plan limits. 💸


As you can guess, running AI is costly. But during the beta, we didn’t want to introduce a new pricing model. Instead, you’ll be able to use the AI features within your current plan limits:


If your plan isn’t listed here, you might be on one of our deprecated plans. You can learn more about those[here](https://zpl.io/article/how-ai-features-work-with-your-plan) .


### It’s still beta — let us know how it goes. 👀


Today marks the start of the public beta, but we’re excited to keep improving these features even further. So, we’d love to hear what you think! We’re pretty active on[Discord](https://discord.com/invite/2nCYn2SSnJ) , but if you prefer email, you can also reach us at support@zeplin.io.


Until next time — またね! ✨
