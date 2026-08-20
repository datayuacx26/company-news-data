---
schema_version: "1.0.0"
document_id: "ca02e879d8d10102cbed46b35f9edabc93dff6b9290e633c77116279dfde1abf"
company_key: "yc-onlook"
company: "Onlook"
source_id: "yc-onlook-rss-a3426f90edb2"
canonical_url: "https://onlook.substack.com/p/february-2024-update"
published_at: "2024-04-17T20:25:28+00:00"
first_seen_at: "2026-07-27T04:07:26.124598+00:00"
fetched_at: "2026-07-28T21:00:24.623123+00:00"
content_hash: "sha256:b1ae811bac78e8f2494db78db7cef011987ebf500211ee03ff3b63f14ea98575"
---

# Our February Update

# Our February Update


### Cinci visit, AI integrated, pushing PRs, and an improved Editor.


[Daniel Farrell](https://substack.com/@danielfarrell)


Apr 17, 2024


For our first post we wanted to share our monthly update we wrote for the end of February. It shows some of our thinking on the product and where we’re at with development. Enjoy!


---


As a reminder,


[Onlook](https://onlook.dev/?utm_source=newsletter&utm_medium=email&utm_campaign=Monthly%20Investor%20Updates) turns any product team into an engineering team by making it super easy for designers to edit designs directly in production, and then effortlessly push their changes to GitHub.


## TLDR


-


Daniel flew out to pow-wow in Cincy


-


We’re interviewing 3-4 teams weekly


-


AI is now integrated into the core product


-


Daniel pushed 4 PRs into production using Onlook


-


Editor experience is no longer a blocker for people to use the tool


## Announcements


**First team onsite**


Daniel left his apartment in smalltown NYC to experience the splendor of Cincinnati for 1 week with Kiet and Erik. He got a nice Cincinnati welcome with dinners at OTR classics like Abigail Street, the Eagle, Teak, etc. We also used this chance to focus on company values, culture, fundraising, and planning out our releases for Q1. Here are two pics:


Where Erik convinced us that Apple Vision Pros may be a respectable employee perk to offer


Feast on how much work we did right before we erased it all on this whiteboard


**User interviews**


The team has averaged 3-4 user interviews per week since last month. The feedback has been trending more positive as we’ve continued to improve the application, yet we still are looking for our early adopters to fully integrate our product into their workflow.


## Product


We think of the product in three stages:


1.


Editing


2.


Sharing


3.


Submitting code


This month we achieved the full “closed-loop” of the product so that the platform is usable from steps 1 to 3.


**AI entered the chat**


Last month, we shipped our translation layer built on top of state-of-the-art language models. To make it


*really* easy to understand, we made it so that when designers make a design edit, AI finds what to change in the code, and makes the change. Fun!


AI allows us to push code changes that work with the team’s codebase in their own setup, making Onlook the perfect plug-and-play solution for any team. We hope to continue building out our AI suite to allow designers to not only ship code but also ship


*better* UI code than the average engineer.


Here’s a change where the AI added a UI change but also fixed a subtle bug (can you spot the bug?).


**Daniel shipped 4 pull requests using Onlook**


With our release of pushing to code, our designer numero uno (Daniel) has made significant UI changes to the codebase using only Onlook without needing to clone the repo or run it locally.


To be specific, Kiet made a Feedback form, Daniel jumped in and styled it to look way better, and Kiet merged the code that Onlook produced into production all within a few minutes.


This is a huge unlock for the velocity of the team shipping and also validates the value of the platform. Currently, we still have to review the changes manually before pushing, but as the platform matures, we hope to cut this review process down to zero. (See Daniel’s GitHub contributions below)


**Editor joined the dark side**


As a side effect of following users' feedback, the editor got major improvements both on UI and functionality. It’s starting to grow out of its awkward teenage phase and is now in its underclassmen-in-college phase, striving for full maturity, but still with a few quirks and some personal skills that need a bit of polish. We’re hoping intramural soccer will help.


The editor now supports a layers view, undo-redo, multi-select, and even a mini code editor. Here’s an action shot.


(nice!)


## Next Month


In January our biggest challenge was the quality and usability of the editor. Now that we’ve solved that, our biggest challenge in February has been getting fans / users / advocates at companies to feel comfortable enough to add our product to their codebase. Despite the enthusiasm for the product, many designers drop off at the point where they need to configure their codebase. This is due to either lack of access to the codebase or to our onboarding process being too confusing.


Our goal for March is to onboard 3 teams to start making pull requests through Onlook. In order to do that, we’re going through our waitlist and searching for advocates at organizations who would be open to being an early adopter of our product.


To continue to make it easy for people to use Onlook, we’ll be adding an improved onboarding experience with demo projects as well as developer docs. We’re also adding more editing capabilities such as inserting new components like buttons, text, frames, etc. which will be a huge benefit to getting designers full control over the designs.


## Asks


-


Intros to product designers at smaller teams and Series A startups that would be open to adopting this tool. We’re focusing on designers with influence, familiarity with code, or the ability to access GitHub.


-


Intros to investors and angels in the dev tools / design / AI space who’d be interested in what we’re doing.


We still feel very early in terms of what our product is and can be, and we’re grateful for your patience and support as we continue to discover what Onlook is in these first few months. The biggest milestone has been us using the tool internally to make changes to the product, so we’re definitely excited to keep testing it internally and improving it. If you’d like to try it out, let us know and we’d be happy to share a demo!


Best,


Kiet & Daniel


Cofounders of Onlook with mega kudos and support from


[Erik Nelson](https://www.linkedin.com/in/eabnelson/)


Thanks for reading our update! Subscribe for more from the Onlook team and follow along as we build a new way to build.
