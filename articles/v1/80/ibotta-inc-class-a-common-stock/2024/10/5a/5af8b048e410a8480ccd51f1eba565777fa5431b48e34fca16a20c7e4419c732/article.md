---
schema_version: "1.0.0"
document_id: "5af8b048e410a8480ccd51f1eba565777fa5431b48e34fca16a20c7e4419c732"
company_key: "ibotta-inc-class-a-common-stock"
company: "Ibotta Inc."
source_id: "ibotta-inc-class-a-common-stock-rss-dcf741155171"
canonical_url: "https://medium.com/building-ibotta/technical-writing-at-ibotta-10b2b64f3f66"
published_at: "2024-10-20T15:22:49+00:00"
first_seen_at: "2026-07-25T01:07:04.216753+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:2308779c19814a34656ed9cbdedb96f308a442b51ca27d45c3b2f361efd0a026"
---

# Technical Writing at Ibotta

# Guidelines for Technical Articles


[Jaylyn Stoesz](https://medium.com/@jaylynstoesz?source=post_page---byline--10b2b64f3f66---------------------------------------)


7 min read


·


Mar 9, 2023


--


*This article is intended for individuals in engineering leadership roles — or those who want to be — to encourage and help their team to effectively produce technical writing in their organization.*


Technical articles are powerful tools for not only sharing engineering knowledge, but also creating a culture of growth, innovation, and transparency. There is a litany of good reasons to promote technical writing on an engineering team which benefit authors of every skill level and domain, for example:


- Deeper understanding of the tech
- Reducing tacit knowledge
- Improved engagement
- Organizational visibility
- Content for promotion packets
- Company branding and talent acquisition


Encouraging your team to do technical writing regularly helps build good habits and documents their journey. At a minimum, teams should publish content whenever they complete a major project or solve a particularly difficult problem.


As an engineering leader you are uniquely positioned to drive this aspect of your culture forward. The following is a set of guidelines we developed at Ibotta to help do that effectively, with an explanation of how engineers of different experience levels can be expected to contribute.


## Best Practices


A quick Google search will provide many excellent style guides for technical writing, so we won’t cover that here; but below are a few good practices to keep in mind when writing an article or helping someone else write one.


- **Use visual aids:** use graphics as appropriate to help the reader see what the author sees. Just be careful not to expose proprietary information about our systems. Include detailed architecture diagrams and internal documentation on internal publications only, unless otherwise approved. Also be sure to use alt-text for accessibility.
- **Include inline links and cite sources:** sources lend credibility to the writing and help readers explore further if they are inclined to do so. If the reader needs prerequisite knowledge to fully understand the article, there should be links to that information.
- **Avoid internal jargon and tacit knowledge:** this makes knowledge sharing difficult and defeats the purpose of this exercise. If the author has to use jargon, it should be briefly explained in the article.
- **Keep it only as long as necessary:** choose quality over quantity. Be as clear and precise as possible without adding too much fluff, but balance that with making the article enjoyable to read. Most articles are around 1,000 words or so. Longer articles can be broken up into a series for better readability.
- **Give credit where credit is due:** give shout-outs! If someone participated in identifying the problem or building the solution, they should be given credit either individually or as a team. Use good judgment on this to avoid alienating individuals who contributed.


## Content


Technical articles should be specific and easily digestible. They should clearly discuss engineering problems that you have faced as a company, and subsequently position your team as a knowledgeable resource on that topic for the broader software engineering community.


While articles don’t necessarily have to be in this exact format, in general they should include these sections to make sure their topic is discussed with enough depth to show a measure of authority:


1. **Identify the problem and who it is most relevant to:** what is the error, performance issue, or missing engineering best practice? Did you introduce a new feature to fill a gap in the user experience? Think about who the intended audience of the article is, and what context they’re expected to have before they read it.
2. **Explain why this problem matters:** what is the business or user experience impact of this problem? Is there a measurement the author can use to illustrate it?
3. **Provide a solution:** give examples — code snippets, screenshots, scripts, and demos. Unless the article is explicitly discussing tradeoffs between different approaches, it should typically be limited to one solution to keep things concise.
4. **Prove that it works:** should include benchmarks, test results, etc. Ideally the article can demonstrate an improvement on the measurement in section 2.
5. **Explain why it works:** cite sources and provide additional resources for further exploration. This often gets into Computer Science concepts or digging into the source code of the language or framework being used.


## Goals by role


Engineers at different points in their career can set different goals for what they want to accomplish with their article, depending on how prepared they are to address each section at that point. As experienced engineering leaders we can help define what those goals should be and fill in the gaps.


If you are providing guidance to an author, take care to give them what they need to really grasp the explanation so that they can put it into their own words. Avoid telling them exactly what to write. It’s important to help authors find their own style — authors’ voices are beautifully diverse, and you should do your best to bring those voices out in their writing. Try to identify and enhance it, whether the author is academic vs. conversational, or authoritative vs. suggestive.


The figure below summarizes the high-level expectations we would set for an author depending on where they are in their engineering career:


Press enter or click to view image in full size


Technical writing goals by role


### Associate


Technical writing for Associate engineers is more of an exercise in collecting their thoughts and building confidence than anything else. Associates should pair with more senior engineers to make sure all of their bases are covered. They will likely need help identifying unknown unknowns.


- Sections 1 and 3 should be self-guided. They should pair on section 2 to make sure they have appropriate business context, as well as 4 and 5 where applicable.
- Shouldn’t be expected to get too deep into the “explain why it works” section. They can largely focus on sections 1 through 3, and 4 is a bonus. Section 5 could become overwhelming depending on the person, but they should be encouraged to try.


### Mid-level


Most Mid-levels have not published anything at this stage in their career. They should be expected to have more context and self-direction than Associates, but discussions of theory might still become overwhelming depending on where they are in their tenure. Again, at this stage technical writing should be thought of as an exercise in building confidence and communicating clearly more than anything else.


- Sections 1–3 should be self-guided.
- Might need some guidance on sections 4 and 5. Encourage digging deeper into section 5 but a technical deep-dive should not necessarily be a requirement.


### Senior


Senior engineers should be building strong investigative and communication skills at this point in their career, and be more confident solving difficult problems. They should be able to prove that their approach is effective without help, but may not yet have the experience to explain the deeper mechanics of why their solution works without some direction. This depends heavily on the individual and where they are in their tenure as a senior.


More experienced Seniors will be able to execute on sections 1–5 without any guidance. In this case they should be given the space to do so. However, the scope should generally be more narrow than something written by a Staff+ engineer.


- Should be largely self-directed.
- Might need some help with section 5 depending on the person.


### Staff+


At this point in their career,[Staff+ engineers](https://staffeng.com/guides/) should be able to build a strong case for their solution without much guidance. If they don’t know the answer to why something works or how to prove it, they should be able to research effectively and figure it out. The breadth of the problem discussed in the article should typically be broader than one by a Senior engineer.


## Feedback


Feedback is a critical part of any professional writing. Authors should solicit and incorporate feedback for every contribution. Each article should be peer-reviewed to make sure it is correct, thorough, readable, and typo-free. Articles should go through several layers of review:


- **All team members:** the author is speaking for their team. Everyone should be comfortable with what’s said in the article.
- **Staff+ engineer or tech lead:** check for inaccuracies, and make sure the scope of the article fits within the team’s area of expertise.
- **Engineering Manager/Director:** the article should be aligned with the larger organizational direction. These folks can also scan for anything Legal/Executive Leadership would need to sign off on.
- **Legal/Executive Leadership:** in some cases, the Legal team or Executive leadership may need to check for legally dubious or proprietary information that should not be publicized. Engineering managers and Directors should be able to identify if any content in the article falls into this category.
- **Publication admins:** admins for your publication should spot-check all articles before publishing them in a public forum. This is usually not necessary for internal-only publications.


Some tips for providing effective feedback:


- Ask thoughtful questions that the author can answer in their own words. Do your best to guide the author to the best phrasing.
- Try to read the article from the perspective of someone who has little to no business context about what your team does. More specifically, use the perspective of the intended audience of the article, which may be individuals from outside of the company.
- Be constructive! This exercise is meant to build you up as a team. Point out things that the author does well in addition to things that could use improvement.


## Constraints


While internal publications can usually expose whatever information is needed to clearly articulate the message, external publications are more limited. Sometimes you may need multiple versions to avoid conflicting messages internally.


Some hard requirements for external publications are:


- Avoid legally dubious language. If the article talks about anything related to key performance metrics, finances, or security, that’s a clear indication that it probably needs review by the Legal team. It must not contain any sensitive business information including vendor names or revenue figures. Your Legal and PR team should be able to provide guidance on what can and can not be discussed.
- Avoid exposing proprietary information about how your system works. If you are explaining architecture in detail, keep it either very narrowly-focused or very high-level/genericized.
- Do not expose internal data or data structures. Examples must be genericized.


*Interested in working at Ibotta? Check out our*[Careers Page](https://home.ibotta.com/work-with-us/careers/) *!*
