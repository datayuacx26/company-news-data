---
schema_version: "1.0.0"
document_id: "b32ba8983e83fdba4777c2fed09f26f2896f0f33f24b0d5122e83699b31759e2"
company_key: "thredup-inc-class-a-common-stock"
company: "ThredUp Inc."
source_id: "thredup-inc-class-a-common-stock-rss-cddb21541ec6"
canonical_url: "https://medium.com/connect-the-dots/designing-our-new-experimental-shopping-tool-thrift-the-look-e0bbb29c6e06"
published_at: "2022-03-15T00:47:16+00:00"
first_seen_at: "2026-07-20T23:18:30.437987+00:00"
fetched_at: "2026-07-28T21:04:01.772019+00:00"
content_hash: "sha256:5a59a168152e081f5bc9abb6c81191b79ff772ab2fd6796a765d875a6d3c8afb"
---

# Designing our new experimental shopping tool: Thrift the Look

# Designing our new experimental shopping tool: Thrift the Look


[Sophia Liu](https://medium.com/@hastalasophia?source=post_page---byline--e0bbb29c6e06---------------------------------------)


9 min read


·


Mar 11, 2022


--


The story behind our newest Thrift the Look feature and how it transformed our product development process.


Press enter or click to view image in full size


Our newest Thrift the Look feature incorporates the high potential areas of outfit inspiration and outfit shopping using recommendations generated from a visual similarity algorithm.


## The Problem


ThredUP carries millions of items, and we hear feedback from overwhelmed customers that they don’t know what to look for while shopping. As a designer on the Discovery team, my job is to help the team transform top-of-funnel browsing behavior and help customers discover the items they want to buy, from the moment they hit the site to the moment they land on a product detail page.


Press enter or click to view image in full size


## The Solution


Our newest Thrift the Look feature integrates the high potential areas of outfit inspiration with shopping using item recommendations. Customers can get inspired by a feed of outfit ideas, then recreate those outfits using matching items sold on thredUP.


This feature is positioned as a new way to browse, as part of our larger mission to help people shop more sustainably without feeling like they have to give anything up. You can still love fashion, celebrity and influencer style, and dressing up without falling victim to fast fashion.


Press enter or click to view image in full size


Press enter or click to view image in full size


Press enter or click to view image in full size


The shipped feature: A landing page shows a continuous stream of content that captures a variety of fashion trends and styles, represented by seasonal filters. Customers can tap on the + dots to shop their favorite elements of each outfit and and “build” an outfit based off the photo to purchase.


## Getting Inspired


At the beginning of last year we as a product team read *Inspired* by Marty Cagan, which transformed how we thought about the product development process. We saw the importance of discovering the right product, identifying risks early on, getting engineers involved in brainstorms, and customer validation. We were determined to move away from the traditional feature-oriented waterfall process and try out a more iterative double diamond approach.


For this project, our primary goal was to increase new user acquisition and extend browsing time by introducing a new way to navigate the shopping experience and help customer discover products. In addition, an internal goal was to prove out the importance of product discovery as part of our product development process. For this project we tried out new ways of working that strayed from how things were always done. We pushed to move slower and more deliberately, making time for research. We placed big bets on innovation and got the company excited about a feature that involved new algorithms, styling, merchandising, and lots of user-generated content. In the end, we got to launch one of our most exciting and innovative new features to date.


Press enter or click to view image in full size


The project timeline.


## Lessons Learned


1. **Frame a problem for an effective brainstorm.**


Brainstorms are vital to the creative process but can get messy if unfocused. Emma (Discovery team PM), Karen (Director of Product Design), and Tess (Director of Data & Analytics) planned a brainstorm around discovery experiences. Brainstorm participants spent that time reviewing current problems within the customer experience, learning what’s possible in with data science, and brainstorming feature ideas that make shopping 1) fun, exploratory, shareable and 2) easy, useful, and convenient. The brainstorm was informative and focused, and allowed us to think big.


Press enter or click to view image in full size


A napkin sketch to initiate discussions with the engineering team.


**2. Bring in engineering early.**


After our brainstorm, we selected a few high potential ideas to explore and mock up to evaluate feasibility. Our design team split up the work and wireframed rough concepts to present to engineers and generate product ideas and discussions. How feasible would it be to search an image and generate visually similar results? What about an outfit builder? Does that require ongoing engineering maintenance? With engineers and data science involved from the start, we were able to innovate in directions we’ve never considered. We want diversity of thought when it comes to tackling problems. The more experts and knowledge that gets passed around, the better positioned we are to discover the best solutions.


**3. Build a proof of concept.**


Since this project wasn’t on a specific roadmap yet, Emma got buy-in from a couple of passionate engineers, and together we pulled together a proof of concept for a company hackathon. We ended up winning first place! Even though we had less than a week, the time we were able to dedicate to a collective effort helped us move quickly, finish an incredible amount of work, and get a working prototype out the door. The algorithm was unfinished but enough to help us showcase the idea. Most importantly, this hackathon win brought visibility to the project and excited the execs about moving faster, completing the feature, and launching it the following quarter.


Press enter or click to view image in full size


Testing hover interactions. How do make make it clear that outfit images are clickable?


**4. Beta test with customers.**


It’s a good sign when the internal team gets excited about a feature, but we still needed to validate with customers. We released our working prototype to a small number of customers and emailed them asking for feedback. Quantitative survey results showed that a majority of them liked the feature and said they would definitely use it at least occasionally. With the help of our UX researcher, Anna, I scheduled customer interviews to get more insight as to “why?” I even invited cross-functional partners like copywriters and engineers to sit in on these calls, where we learned that customers preferred to use this tool to shop for occasions, preferred on-figure user-generated content where compared with mannequin collages, and craved for more accurate item matches. These learnings helped us incrementally improve all aspects of the feature, ranging from interaction design details to the item recommendation algorithm.


Press enter or click to view image in full size


Anna and I run a usability test an early prototype. Did people know how to navigate the feature?


**5. Pressure test the algorithms.**


One of the many tests we ran was the algorithm itself. How do we surface good product recommendations? Our data science team had always relied on a certain vendor to generate these recommendations on our product pages, but Claire, our PM intern, decided to put that to the test. A blind survey testing the existing algorithm against one from a new vendor demonstrated just how much better the new option performed, and we ended up switching over. Because customer interviews revealed that the item recommendations were key to the success of this feature, we pushed for this investment.


Press enter or click to view image in full size


Blind survey test of item recommendations. Which selection of skirts seem like a better match for the photo?


**6. Ask engineers for design feedback.**


Engineers were involved from the start, which, lucky for me, means I got to use them as design resources. One-on-one designer-engineer brainstorm sessions were extremely valuable as we brought them into the design and feature development process, bouncing ideas around and getting effort estimates that helped us move faster. When designing the Thrift the Look desktop landing page, I had originally proposed a clean, simple grid of squares to showcase the outfit photos. Taller images would be cropped to fit the square preview. Vlad, a frontend engineer, chimed in and suggested using flexible containers that could accommodate different aspect ratios. The Pinterest-style layout would also allow taller images to fill the width but not cut off any dots that were located towards the bottom of the photos (typically placed over shoes). Brilliant!


A handoff should not be the first time an engineer is seeing “final designs.” In a one-way communication, every handoff loses a percentage of the context and knowledge at each subsequent step. Nobody ever wants the end result to be a set of features that don’t fully serve their intended purposes.


Press enter or click to view image in full size


Press enter or click to view image in full size


My original grid layout proposal (left) gets completely transformed and improved by an engineer’s input (right).


**7. Design for scalability.**


When I build, I want to build for longevity. Products and features come and go, but if you can design a system that can fit varying needs, scale as needed, and accommodate change with minimal development time, they will stand the test of time.


Thrift the Look is a system that allows for all types of imagery, ranging from on-figure photography to mannequin collages. Both can co-exist in this space without having to follow a strict visual formula. Every look is tagged with a keyword, and those keywords populate as filter pills along the top to help people pick an occasion or trend direction to browse. We can choose to show and hide certain keywords to stay seasonally relevant (“wedding guest” looks for summer) or introduce new trends as they become popular (“y2k” looks, anyone?) The feature has been live for almost a year now, and this tagging system has been well-received and well-used to capture a variety of styles for different audiences.


My long-term blue-sky vision for Thrift the Look is to transform thredUP into a destination for outfit inspiration and the go-to source for thrifting matching items. It would evolve into much more than a feature and go on to connect ThredUP to influencers, stylists, Instagram, Tik Tok, and thrift-savvy Gen Z audiences to build a community of shoppers who inspire one another. Even though we’re still very far from making that a reality, the current feature is designed and built to serve as a solid foundation for future updates and increased functionality.


Press enter or click to view image in full size


The self-service admin tool allows any team to upload new looks as needed.


**8. Build for self-service.**


After the initial launch of this feature, ownership moved over to the marketing team. It has become a versatile tool for them to incorporate into influencer campaigns, and a unique offering to pitch in upfunnel advertising. They’re in charge of gathering new user-generated content from our influencer ambassadors to upload to Thrift the Look. However, with the first few batches of uploads, the process lagged because so many people had to be involved, namely, engineers. Backend engineers Artem and Aishwarya built a self-service admin tool that would allow the marketing team to directly upload photos without the help of product designers or developers. Win-win.


Press enter or click to view image in full size


Press enter or click to view image in full size


Recent brand marketing campaigns with Sex and the City costume designers (left) and Youtube star Emma Chamberlain (right) that use Thrift the Look to create shoppable outfits.


Six months after initiating this project, we shipped this feature on the web app in August 2021, and on the native mobile apps shortly after. Early results showed that feature usage is associated with higher conversion. We inferred that shoppers are interested in a contextual shopping experience with influencers where they can see a look and immediately buy it. This feature has pushed our team to think differently about the shopping experience as a whole — it’s not so much linear as it is journey-based.


Press enter or click to view image in full size


Press coverage on[Fox Business](https://video.foxbusiness.com/v/6281200889001#sp=show-clips)


What started out as a hackathon project became a huge ongoing collaboration across product, engineering, brand marketing, influencer marketing, and stylist teams.


I’m thrilled to have been involved in this tremendous effort, and excited for what’s to come. Most importantly, within our product team, we proved the importance of product discovery and research as part of our development process. We strived to innovate on behalf of our customers and ended up creating a fun, new experience that can help millions of thrifters shop easily and sustainably.


See it live:[https://www.thredup.com/looks](https://www.thredup.com/looks)


Press enter or click to view image in full size


A huge shoutout to the team: Angela May Chen, Kelly Lo, Lindsay Martinsen, Anna Brunner, Claire Illmer, Emma Herlihy, Brittany Reano, Aishwarya Umapathy, Oleksandr Shevchuk, Tetiana Torovets, Tess Kornfield, Vlad Snisar, Meredith Capone, Mike Rocco, Artem Shablii, Denys Feshchenko, Ali Ocampo, Catherine Clark, Kyle Blum, and everyone else who helped out, gave feedback, and cheered us on.


*Sophia Liu is a San Francisco-based product designer at thredUP, where she designs products that help extend the life of clothes and fight fashion waste. Read more about her life and work at*[studiosophy.com](http://studiosophy.com/blog)
