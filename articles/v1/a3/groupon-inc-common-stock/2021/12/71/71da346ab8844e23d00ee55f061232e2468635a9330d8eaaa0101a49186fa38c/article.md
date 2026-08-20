---
schema_version: "1.0.0"
document_id: "71da346ab8844e23d00ee55f061232e2468635a9330d8eaaa0101a49186fa38c"
company_key: "groupon-inc-common-stock"
company: "Groupon Inc."
source_id: "groupon-inc-common-stock-rss-99044b78d036"
canonical_url: "https://medium.com/groupon-eng/cx90-rethinking-redesigning-and-reimplementing-the-groupon-user-experience-59a03b6c306c"
published_at: "2021-12-22T16:57:17+00:00"
first_seen_at: "2026-07-20T04:36:53.869647+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:652783fd370e1652060111149c9f7766af77f76111e6c6a4ff18e1a74e1dd278"
---

# CX90: Rethinking, Redesigning and Reimplementing the Groupon User Experience

User Experience


User Interface Design


Agile


Product Analytics


# CX90: Rethinking, Redesigning and Reimplementing the Groupon User Experience


[Avleen Kaur](https://medium.com/@avkaur?source=post_page---byline--59a03b6c306c---------------------------------------)


7 min read


·


Dec 22, 2021


--


All Grouponers who haven’t been living under a rock this past year probably know what CX90 is. But for the few that missed out on this extraordinary project and the rest of the world, CX90 was a huge effort across the Product and Engineering Teams at Groupon where all of us came together to redesign our user experience and interface within a period of 90 days. It was an **“all hands on deck”** situation and probably one of the greatest collaborations we’ve seen across the organization where people from different backgrounds and areas of expertise joined forces to transform a simple vision into reality.


## Why?


The old Groupon interface focused primarily on a deal feed that allowed our users to scroll through the entire depth of our inventory either for all types of deals or for specific categories of interest. We soon realized that our users were missing out on all the wonderful opportunities we had to offer. We wanted to help them discover new experiences and[“Grab Life by the Groupon”](https://www.youtube.com/watch?v=Ii3dzAYzMog&ab_channel=Groupon) by going out of their comfort zones. Our new interface solved exactly this problem. We changed our feed from being deal focused to category focused in order to understand user intent and preferences, expose them to the breadth of the inventory that we had to offer, and to help them explore experiences they could potentially enjoy if only they took a chance. And to be honest, we were also long overdue for a design refresh. 😅


## What?


Since I’m a part of Web Search and Discovery team, also internally known as **Ion** , I’ll focus on my own experiences and that of my team. We were responsible for reimplementing the Groupon Home, Browse and Search pages both for desktop and mobile browsers.


For the Homepage, we transitioned from a plain deal feed and deal carousels to bigger and bolder modules with engaging text and images that highlighted the different categories from our inventory, allowing users to explore specific deals that might interest them instead of aimlessly scrolling through a generic feed. Each new module was unique in its shape, sizing and responsiveness to make sure that the page as a whole was easy on the eyes of the user. While the users got this fun new engaging experience as a result, us engineers had to find our own ways to enjoy the process. My team specifically suggested silly unique names for each module that we wanted to add into our codebase based on what it reminded us of, like Poptarts and Oreo Collections (we’re big on sweets 😁). Although our crazy ideas were rejected for the sake of simplicity and following consistent naming conventions, it promoted creativity and camaraderie within the team and provided us with good times to look back on when we were struggling to implement new features or debugging issues within our modules.


Left: Old Touch Homepage, Right: New Touch Homepage Press enter or click to view image in full size


Old Desktop Homepage — focused on a deal feed and deal carousels


New Desktop Homepage — focuses on different categories and shows engaging modules


For the Browse and Search pages, we wanted to make it easier for our users to find their perfect deal by exposing them to the different filtering options at their disposal. While our old interface provided the users with a row of filters at the top of the deal feed which they could then dig deeper into to find the option that suited their needs, our new interface exposes all the different options to them along the left-hand side of the page. Within this new left-hand navigation, the filters are ordered based on user interaction data, with the more popular filters shown higher up. Each filter exposes all its different options by default. Not only is this new design cleaner and more functional, it highlights user selections, keeps the filters sticky when the page is scrolled and aims to reduce any mental load by subtly showcasing the different options already selected and the many more that could help with filtering out irrelevant deals.


Press enter or click to view image in full size


Old Desktop Browse Page — filters were presented at the top of the deal feed Press enter or click to view image in full size


New Desktop Browse Page — filters are aligned to the left-hand side with the different options exposed


## The Impact


Now that you know the why and the what, it’s time to talk about the huge impact this has had, which means talking a little about the data analytics side of things. There’s one quote that I’ve heard from our Global Head of Project Management Organization,[Christine Stone](https://people.groupon.com/2021/christine-stone-global-head-of-pmo-engineering/) , on several occasions that has stuck with me, ***“If you can put data into a story, the story will stick.”*** So, that’s exactly what I’m going to do now.


Our data analytics team goes through an insane amount of data, measures several metrics and synthesizes the enormous information gathered into simple numbers that we can understand. The overall goal of CX90 was to improve user engagement more so than conversion. Hence, the team was focusing more on metrics like page views, bounce rate, click-through rates and general user interaction with the new modules. For both desktop and mobile browsers, the **bounce rate was reduced by 3%** in the new experience. This meant that now more users were staying on and exploring Groupon.


Since CX90 mobile was experimented on and rolled out over a period of several months, we had more data to reflect on. Users were shown two different templates on Browse pages depending on how deep into a category they were. The first basic template, where we added subcategory navigation tiles at the top of the page, saw **7% more engagement** and a **1% lift in conversion** . The second, which was a more detailed template that showed a sleeker subcategory navigation with filtering options, saw **12% more engagement** and a **5% lift in conversion** . These positive numbers gave us the confidence that our users were enjoying and simultaneously learning and getting used to this new Groupon experience. We were convinced that this was the right path forward and hence rolled out CX90 for desktop much faster than we did for mobile to have a consistent experience across platforms.


Old Touch Browse Page — simple filters shown on top of an infinite deal feed Press enter or click to view image in full size


Left: New Touch Browse Page template 1 — subcategory navigation tiles shown at the top, Right: New Touch Browse Page template 2 — subcategory navigation tiles and filtering options shown at the top


Not only were we gaining more confidence in our new experience, but also we were constantly iterating and shifting gears based on the learnings from our data. For instance, on the CX90 mobile Homepage experience, the first module the users saw was a simple scrollable horizontal navigation of 8 tiles to encourage them to select a category tile to browse deals within. Our analytics team noticed that the users were engaging with the first 2–3 tiles, which were visible in their device viewport and were missing out on the rest of the tiles, which were reachable only if they scrolled horizontally through that section. We quickly adapted to instead present the users with the 8 tiles laid out over 2 rows so they could simultaneously see all the different categories they could navigate to.


Press enter or click to view image in full size


Moved from horizontal scrollable category tiles to icon-based category tiles laid out over two rows


## Conclusion


What started out as a 90-day project turned into one of the core areas of our focus this past year. We wanted our customers to have a modernized and engaging Groupon experience. This project was our way of showing them that we’d always live up to our motto **“begin and end with the customer”** and will keep their happiness at the forefront of our minds. While the initial adrenaline rush of the first 90 days has settled down, we still have some work to do. We continue to iterate, make progress and expand so we can take this experience that’s currently released in NA (North America) to EMEA (Europe, Middle East, Africa) and APAC (Asia-Pacific) as well.


As someone who was new to Groupon when this project kicked off, the countless hours spent with my fellow developers collaborating and pair programming, the various bug bashes with the Quality Engineering team and the brainstorming sessions with the Product team provided a great opportunity to bond. This project has given us all some fond memories and a chance to form connections and friendships across the organization that we’ll deeply cherish in days to come.


*Thanks to the entire Ion team, including developers, QE and product for their continual support and encouragement* ❤️


*Thanks to Aya Salka and Vivek Vancha for walking through the analytics side of things with me* 😄
