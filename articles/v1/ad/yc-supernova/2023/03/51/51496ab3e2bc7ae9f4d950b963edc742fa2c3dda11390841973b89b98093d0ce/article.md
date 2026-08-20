---
schema_version: "1.0.0"
document_id: "51496ab3e2bc7ae9f4d950b963edc742fa2c3dda11390841973b89b98093d0ce"
company_key: "yc-supernova"
company: "Supernova"
source_id: "yc-supernova-rss-864f3bee1480"
canonical_url: "https://www.supernova.io/blog/accessibility-and-design-systems-the-deep-dive-q-and-a"
published_at: "2023-03-16T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:40.084347+00:00"
fetched_at: "2026-07-28T21:02:21.428828+00:00"
content_hash: "sha256:95e15a87be201f7207623e7947d266c679c4226997284e943f7a9c094b7fedc9"
---

# Accessibility and Design Systems: The Deep-Dive Q&A

Last month we hosted the[Scaling Accessibility Through Design Systems](https://www.supernova.io/scaling-accessibility-through-design-systems) panel — with our fantastic panel consisting of accessibility advocate and design leader[Geri Reid](https://twitter.com/gerireid) , renowned engineer[Lauren Beatty](https://www.linkedin.com/in/lmbeatty/) , and UX and design researcher[Stéphanie Walter](https://stephaniewalter.design/) **.** You can also find the recording of the panel if you missed it.


However, the topic of accessibility within design systems is enormous and couldn't possibly be covered in just an hour, and many of you had some burning questions that you followed up with. So we brought your questions to our amazing panelists for a follow-up Q&A to give you all that detailed insight. We split the questions into three main sections, education and adoption, testing and tracking, and broader technical questions.


## Education and Adoption


#### What are some of the biggest challenges in ensuring that websites and products are accessible to all users, and how are those challenges addressed?


**Lauren** : At[Zapier](https://zapier.com/) , we face two main challenges in ensuring accessibility:


- Development moves quickly, and testing beyond static analysis can be overlooked.
- Business buy-in to address accessibility debt.


To address these challenges, we take the following steps:


- Educate our development teams to make manual testing more straightforward and accessible.
- Allocate budget to purchase outside testing from qualified accessibility experts. When our leadership sees that users cannot use our product due to our poor choices, it motivates us to fix the issues.


#### We have a strong brand, but our app's colors are not accessible. How can we convince the brand team to change this, considering their concern about losing customers who love our brand?


**Geri** : Focus on the target demographic who love your brand. How many of these customers aren’t able to use your product because they have visual impairments, are color blind or use your app outside in bright sunlight? Use an impairment simulator to show the brand team how these customers perceive your product. Calculating this exclusion might give your argument more weight.


**Stéphanie** : I’m not sure anyone lost customers because they changed the branding, and I would ask them for some data to back up that claim. But, this might be deeply rooted in the “accessibility makes products ugly” old myth. If anything, having a more accessible website could actually bring more customers. And if that doesn’t work, if it's a website (or native app), there might be a technical solution for that. You could offer a theme switcher and have a “high contrast” mode for people who need it. It’s not a perfect solution because this will only cover the app and site, so you might still have issues with PDFs, documents, and print communication, but it’s a start.


#### How do you convince the Product Managers that a11y work is worth prioritizing?


**Geri** : Aligning accessibility to the company’s strategic goals can help get it onto roadmaps. This could support objectives around driving forward innovation, extending market reach, or enhancing brand values toward diversity and inclusion. Minimizing legal risk is also a consideration that might help prioritize accessibility.


#### How do you communicate that product teams need to ensure their product is accessible? It seems like I always have to re-explain that DS gives you the building blocks, but teams assembling those blocks need to make sure they assembled them correctly.


**Gerri** : Assist consuming teams by documenting component accessibility. Suggest aspects such as focus order, keyboard interaction,[ARIA tags](https://www.w3.org/TR/html-aria/) , and expected user experience to help teams configure and compose components thoughtfully. If you possess accessibility knowledge in the systems team, you could offer an accessibility review of their work-in-progress. Catching potential issues early in the design phase can prevent complications later. It's important to note that although a design system supports accessibility, it is not solely responsible for ensuring the end product is accessible. Product teams must also be accountable for their own quality assurance and testing.


#### What is the minimum set of artifacts that should be ready when you start to introduce an internally designed and developed design system in the wider corporate?


**Lauren** : I would start with an audit of what's currently being used in the product. What are the common patterns? A design system that folks love to use is not something that’s forced upon product teams but is directly solving those teams’ challenges. I don’t know that there is a bare minimum of what you need to get started; the smaller the initial setup, the faster you can get the design system out to your consumers and get feedback. Rinse and repeat!


#### Do you have any "curb-cut effects" examples within Design System assets? To promote a11y within stakeholders.


**Geri** : The "curb-cut effect" is making accessibility improvements that go on to be appreciated by a larger group than the people they were designed to help. A digital example of this is[Search Engine Optimisation](https://searchengineland.com/guide/what-is-seo) (SEO). Producing design system components with structured, semantic code and content in plain language can make your products more accessible to users of assistive tools like screen readers. But it also improves how search engines like Google crawl and rank your site. SEO can be a nice upsell to business-focused stakeholders.


## Testing and measurement


#### By creating accessibility as a foundation of the design system, what are the best practices in validating your choices in accessibility design?


**Stéphanie** : You can do a couple of things:


- Small accessibility audit on the components or design to ensure they comply with the level you need to comply to, like[AA](https://www.w3.org/WAI/WCAG2AA-Conformance) or[AAA](https://www.w3.org/WAI/WCAG2AAA-Conformance) .
- Conduct evaluative user research by testing new components and designs with a diverse audience that represents various disabilities. This will ensure that your design choices are heading in the right direction.


#### What’s the best way to build a business case when you know that specific redesigns will require a big lift and lots of effort (and will probably meet resistance)? Or, to put it another way, what success indicators can we measure as a design team?


**Lauren** : Tying the work back to the company's strategy and values is crucial. For example, you could say, "Our company's number one key bet is X. This work aligns with X because Y. It is also in direct alignment with our core value of Z." Stéphanie previously made a great point about survivor bias — it's possible that there are many potential customers who are unable to use your product due to its inaccessibility.


#### We’re considering rewriting WCAG guidelines to focus on how we implement and use them within our company and how to test and implement them within our systems. Has anyone else done this and found it good/bad?


**Geri** : The WCAG guidelines can be complex, so bringing them back to specific applications in your company is a great idea. When I worked at a fintech company, the design system ran an accessibility workshop with the product design team. We created cards that showed the criteria we needed to comply with to meet WCAG AA, and had the designers measure their design layouts against them to decide what needed to be changed. Reducing the WCAG guidelines to relevant and actionable points makes the criteria more relevant. This was a useful exercise to work through as a team, and we saw some positive changes going forward.


#### What about accessibility tools and tests for mobiles or small devices? What is the current state of those? I see lots of tools for web only.


**Stéphanie** : There is a pretty decent list of tools and techniques[here](https://www.digitala11y.com/free-mobile-accessibility-testing-tools/) . These will help you automate some tests, but at the end of the day, you still need manual testing.


#### Do you tend to handle accessibility testing more on the design side of things (e.g. tools in Figma) or more on the engineering side of things (e.g. CI/CD automation)?


**Lauren** : Both! Having both design and engineering on board with testing for accessibility is a game-changer! If design and engineering are really in sync, then the product and users benefit.


## Technical Questions


#### What guidelines do you give your dev team to implement components in an accessible way?


**Stéphanie** : When it comes to “where does design stop and where does dev start,” I’m not sure there is a “one size fits all” guideline here. I would discuss this with the team directly and see who feels comfortable documenting what. You could create sort of a roles and responsibilities matrix for the whole team (and rework it with new team members) as to where each role finishes / starts.


I have an example of such a[mapping](https://stephaniewalter.design/blog/a-designers-guide-to-documenting-accessibility-user-interactions/#team-alignment) . In the dev documentation, you could also add some[generic accessible guidelines](https://dna.babylonhealth.com/accessibility/) . And add some examples of “best practices on how to use the component.”


#### What is the minimum font size required for text to be considered accessible? Footnotes and disclosures, for example.


**Geri** :[WCAG](https://www.w3.org/WAI/standards-guidelines/wcag/) guidelines don’t specify a minimum font size for the web. It’s generally agreed that 16px is a thoughtful minimum for body copy and I wouldn’t suggest going smaller than 14px for footnotes. This can vary though, depending on the font. More importantly, text must allow resize to 200% of its original size. Line length is also a consideration for comfortable reading.


**Stéphanie** : I would also add that you test this with users as well. Especially if you work on enterprise products and dashboards with a lot of data. I work on a project where we switched to 16px to make it easier to read, and we thought it would help everyone. But, we then had a LOT of feedback that the new site is too big and users see fewer data, so they wanted the font smaller. Some users wanted a 10px font size. We ended up with 14px as the base and taught people to use the zoom feature if they wanted smaller or bigger. So, this goes back to Geri’s answer, make sure you don’t block zoom, and people can resize without breaking the layout.


#### Zoom or font size, which one is more important? And how do you make sure the design doesn't break?


**Geri** : Good accessibility is allowing multiple ways to perceive a product, so font size and zoom are both important. If I had to pick one, I’d go with zoom — it’s crucial to be able to resize text up to 200% and allow for reflow when zoomed. The web is fluid and responsive, and it’s us designers putting up barriers by constraining content! This should be a consideration early on in design.


#### Any thoughts about starting to work with APCA guidelines for colors?


**Lauren** : I think Geri and Stéphanie touched on this during the live stream.[APCA](https://www.accessibility.com/glossary/advanced-perceptual-contrast-algorithm) is promising, but if your site is required to conform to a certain WCAG standard, you should stick with WCAG. We’ve experimented with this at Zapier with some elements that need dynamic color variation, and it’s challenging to maintain WCAG compliance.


#### At my work, we often find it the most difficult to make flows or entire pages make sense for screen reader users. Individual components are working great, but the composition with them is not. How should teams go about constructing the parts outside of the design system?


**Geri** : An understanding of semantic structure is fundamental to an accessible screen. Encourage teams to start with unstyled copy and group content into logical regions. Is the order still logical with the styling removed? WebAim has a great[basic overview](https://webaim.org/techniques/semanticstructure/) .


#### Is there a WCAG for mobile apps?


**Stéphanie** : There isn’t. The W3C documentation explains that the existing WAI accessibility standards and guidelines cover mobile. So, for now, there’s nothing specific for native apps. There are still guidelines that apply to both[web and native](https://www.w3.org/TR/mobile-accessibility-mapping/) .


Some people collected resources on how to make mobile apps more accessible though, you could check:


- [https://brucelawson.co.uk/2022/react-accessibility-resources/](https://brucelawson.co.uk/2022/react-accessibility-resources/)
- [https://mobilea11y.com/](https://mobilea11y.com/)


#### Is anyone using tokens to create themes in their system that can help enable AA or AAA contrast ratios?


**Geri** : I’m a fan of the[Radix UI](https://www.radix-ui.com/colors) color system — its use of color allows designers to make sensible choices and, if applied in accordance with the scale, ensures components maintain contrast.


At[NewsKit](https://www.newskit.co.uk/) , we use tokens for multi-brand theming. Using documented tokens helps remove the guesswork for designers on which color and font sizes to use.


---


That's all for this time, we hope these insights can help you better implement and scale accessibility within your organizations. Share with us on[Twitter](https://twitter.com/supernova_io) what your top tips are and join the discussion.
