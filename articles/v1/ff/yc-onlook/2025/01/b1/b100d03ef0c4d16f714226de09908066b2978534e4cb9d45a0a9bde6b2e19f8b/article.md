---
schema_version: "1.0.0"
document_id: "b100d03ef0c4d16f714226de09908066b2978534e4cb9d45a0a9bde6b2e19f8b"
company_key: "yc-onlook"
company: "Onlook"
source_id: "yc-onlook-rss-a3426f90edb2"
canonical_url: "https://onlook.substack.com/p/june-2024-update"
published_at: "2025-01-31T16:43:45+00:00"
first_seen_at: "2026-07-27T04:07:26.124598+00:00"
fetched_at: "2026-07-28T20:58:20.301648+00:00"
content_hash: "sha256:f97ee07bd3e04859d0ef5c4d8346c93287bd9b5fb22e869ba77a66cf8db9cb58"
---

# Our June 2024 Update

# Our June 2024 Update


### Declassifying our old Friends and Family updates


[Daniel Farrell](https://substack.com/@danielfarrell)


and[Kiet Ho](https://substack.com/@kietho)


Jan 31, 2025


Hey there, this is Daniel at Onlook!


If you’re receiving this update, it’s because either myself or Kiet Ho wanted you to know about what’s going on with Onlook.


As a reminder,


[Onlook](https://onlook.com/) is building a product studio (formerly an extension) that turns any product team into an engineering team. We make it easy for anyone to edit web products directly in production, and then effortlessly push their changes to GitHub without having to write any code themselves.


## TLDR


-


**Daniel at ConFig:** Attended Figma’s conference in San Francisco, networking with designers, customers, and investors.


-


**Onlook Studio Introduction:** Proof of concept for a desktop app, aiming for an “all-in-one” visual editing product.


-


**Soft-Launch Success:** New website with download buttons led to a steady stream of 2-3 new users daily.


-


**Generative AI Features:** Users are exploring AI for style and content changes, showing diverse use cases.


-


**Improved Onboarding:** Added walkthroughs to help users navigate the editor and dashboard, addressing previous drop-offs.


-


**Adoption Challenge:** Difficulty in converting designers into organizational champions for Onlook – we’re going to shift to getting developer buy-in first.


## Announcements


### Daniel went to Figma’s conference (ConFig) in San Francisco


Connecting with designers, meeting with customers, chatting with investors (some of you on this list! So nice to meet you in person) running into old friends, and of course riding a few Waymos – the trip was well worth it. Daniel networked with designers and engineers and recruited many of them for follow-up conversations in the coming weeks.


### Introducing Onlook Studio


We’ll have more updates below, but while Daniel was at ConFig, Kiet built a proof of concept for a desktop app we’re calling Onlook Studio (


[GitHub](https://github.com/onlook-dev/onlook/) ). The Chrome extension we built will continue to be live as a handful of users get value from having the editing experience on-hand with their workflow, but we’re excited to make a more “all-in-one” visual editing product with Onlook Studio.


Enjoy this quick video demo:


### **** Soft launching the extension on the new website introduced a steady stream of new weekly users


Last month we redesigned


[our landing page](https://onlook.com/) and added buttons on the site for people to download the extension. 2-3 people download the extension every day. Now, there’s still the funnel of them pinning the extension, signing up for an account, and starting to edit that still needs to be optimized, but it’s great to see more sustained interest in what we’re building, even if it’s not a large volume as of right now, and as we’re not actively promoting the product.


Subscribe for free to receive more posts on how we’re building Onlook


### Users are discovering and using the Generative AI features


Our default experience is still to use the familiar editing sidebar, but users have started to introduce Generative AI into their workflow for more abstract changes. Prompts have included style changes like “increase this by 50%” and “add 30px padding”, but also net-new changes like creating new text or new elements (functionality we don’t have just yet). In any event, it’s been insightful to see what types of changes people are using the tool for in plain language.


## Product


### A new onboarding tour


Last month we found that our self-serve onboarding was a challenge for people to go through. This month, we added a walkthrough on both the editor and the dashboard side to help guide people into designing. This was to address the drop-off of users after they installed the extension and started using it to see if we could make using Onlook easier for new users.


### Onlook Studio – Our desktop app / browser for products


Some of the key points of feedback from users over the past 6 months were that the editor experience didn’t feel as “solid” as they were expecting, the process of building with the extension had too many steps, and it is difficult to share the output of what was written to get buy-in from engineers. Many design apps have an obvious pipeline from creating to exporting, and while it was possible to make that in the Chrome Extension, the form-factor always meant that you had to jump across pages to get to where you needed to go.


While Daniel was at ConFig, Kiet built a proof of concept for Onlook Studio. It’s a desktop app with a more familiar editing experience to designers. When you log in, you get an infinite canvas where each frame is its own “browser” window. It’s like we took each of your browser tabs and put them on the same surface. So, if you want to iterate, duplicate, and experiment with concepts, it’s as easy as duplicating frames or selecting elements to copy, paste, and remix. With Onlook Studio it’s not only easier to build and experiment, but also easy to write changes extremely reliably to the codebase.


This all seems like a pretty dramatic change to what we’ve been building, but with this custom browser experience comes so much more control over how code can be written. The good news is that most of the work that was done on the Chrome Extension is transferable to the new studio experience.


Because we’re also in control of the editing experience, we can share a live link to all of the changes in real-time, so it’s easy to preview your edits and collaborate with colleagues who might not have the Studio.


## Challenges


### A handful of users trickle back every week, but don’t use the product for more than a few edits each session


The launch of AI styles helped us better understand how people are using the tool (“Make this text larger, increase the padding, etc.”), but the core activation experience of shipping usable code is still very much at the end of the editing experience. Onlook’s promise is to better close the gap between design and engineering. To do so, we need to deliver on the end-to-end experience of designing


*and* shipping code.


### Most of our users don’t have access to the codebase / don’t advocate to adopt Onlook


One of the biggest challenges has been to convert designers into champions within their organizations to get Onlook installed on the codebase. Part of that is the editing experience, part of that is the challenge of getting a codebase setup.


## Next Month


### Continue to validate the Studio approach


Our bet with the new Studio experience is that it’ll offer a much more streamlined experience for building products. While most of the month will be spent laying the foundation for that experience, we’re going to use it to both chat with designers who used our Chrome Extension, as well as validate the approach with developers. We’re also going to experiment with our positioning for this product.


### Recruit more developers to try Onlook


While we’ve struggled to recruit designers to attach a codebase to Onlook, we’re going to shift our focus towards getting developers to add Onlook to their codebases. Getting their buy-in was always a requirement anyway.


### We’re open-sourcing Onlook Studio


It’s generally good practice to open-source products that tie to a codebase – it shows we’re not shady and can encourage other people to contribute. With that, we’re publicly releasing the (very) early version of Onlook Studio to share with the developer community. Check it out and


[leave a star](https://github.com/onlook-dev/onlook/) if you have a GitHub account!


## Thanks & Asks


-


Thanks to Erik Nelson for continuing to share resources and for cheering us on from the sidelines.


-


If you’re a developer or have a GitHub account, please star the


[Onlook Studio GitHub project](https://github.com/onlook-dev/onlook/) – it goes a long way towards earning a good reputation in the community.


-


Know any frontend developers or engineers who would be good to chat with about our new Studio experience? Reply to this email and let us know!


Best,


Daniel & Kiet


Cofounders of Onlook


---


Check out our other declassified updates to read more about our progress from the first year of building Onlook:


[Our May 2024 Update](https://onlook.substack.com/p/may-2024-update)[Daniel Farrell](https://substack.com/profile/15503249-daniel-farrell) and[Kiet Ho](https://substack.com/profile/46760661-kiet-ho)


·


December 11, 2024


[Read full story](https://onlook.substack.com/p/may-2024-update)


[Our April 2024 Update](https://onlook.substack.com/p/our-april-update)[Kiet Ho](https://substack.com/profile/46760661-kiet-ho) and[Daniel Farrell](https://substack.com/profile/15503249-daniel-farrell)


·


September 4, 2024


[Read full story](https://onlook.substack.com/p/our-april-update)


[Our March 2024 Update](https://onlook.substack.com/p/march-2024-update)[Kiet Ho](https://substack.com/profile/46760661-kiet-ho)


·


May 7, 2024


[Read full story](https://onlook.substack.com/p/march-2024-update)


Thanks for reading Onlook! Subscribe for free to receive new posts and support my work.
