---
schema_version: "1.0.0"
document_id: "0195fff6efcc4bf4462d4299018e570bac3077866651ddf6855d9d57e7b25c65"
company_key: "chargepoint-holdings-inc-common-stock"
company: "ChargePoint Holdings Inc."
source_id: "chargepoint-holdings-inc-common-stock-news-import-2405547b27fc"
canonical_url: "https://www.chargepoint.com/engineering/keeping-accessibility-a11y-top-mind-chargepoint"
published_at: "2021-11-18T00:00:00+00:00"
first_seen_at: "2026-07-21T13:13:26.870476+00:00"
fetched_at: "2026-07-28T21:33:49.818370+00:00"
content_hash: "sha256:88368aac5878735ee208e6518732e1979a79036b8f6d12dd9ba9d48eee3128e1"
---

# Keeping accessibility [a11y] top of mind at ChargePoint

Share This:


November 18, 2021


# Keeping accessibility \[a11y\] top of mind at ChargePoint


ChargePoint Engineering


Accessibility in software has been a hot topic over the last few years, especially following a major discrimination[lawsuit](https://www.cnbc.com/2019/10/07/dominos-supreme-court.html) against Domino’s, won by a blind man, for the company's lack of accessibility in its web applications. In the U.S. alone, there are 61 million adults (about a quarter of all Americans) living with some sort of disability according to[the CDC](https://www.cdc.gov/ncbddd/disabilityandhealth/infographic-disability-impacts-all.html) . It’s important for all of these people to be able to use web applications to their full potential.


As a company, ChargePoint is taking significant steps towards increasing the accessibility of our user-facing software to accommodate people with disabilities. To do so, we are aiming to follow accessibility best practices throughout our entire software development process. From software engineering to QA and even UI/UX design, it’s important for every team to keep accessibility in mind while designing, creating and testing our solutions. Although the focus of this blog post is web application development, much if not all of what is discussed in this post is relevant to our other software as well.


### Benefits of accessible web application design


There are several key benefits of developing accessible software on the web:


- Developing accessible web applications helps us better serve customers with visual, auditory, physical or other disabilities that may make it difficult to navigate our web applications. As a company, we should be doing our best to enable equitable access of our services to all of our customers. Although it might seem at first that electric vehicle (EV) charging solutions are relevant primarily to the sighted people driving those EVs, anyone can use our software to manage EV charging, so it’s important for it to be accessible to everyone.
- Creating web applications with accessibility in mind also helps us better develop for users with smaller or non-traditional screens, as well as visually impaired users. Many of the specifications we follow while developing an accessible site coincide with the specifications required to make mobile-friendly and elderly-friendly websites as well.
- According to updates to the U.S. Federal Acquisition Regulation, any software developed or used by the U.S. government must be accessible for people with disabilities. By implementing accessible web design practices, we ensure ChargePoint solutions are an option for government contracting. Find more information on this topic at the[U.S. Access Board](https://www.access-board.gov/ict/) .
- A lack of accessibility accommodations can open up the potential for lawsuits. An example of one such lawsuit is the[Domino’s Pizza v. Guillermo Robles](https://www.cnbc.com/2019/10/07/dominos-supreme-court.html) lawsuit mentioned previously. Providing our customers with accessible web applications mitigates the risk of an ADA lawsuit, potentially saving the company millions and, more importantly, improving the driver and customer experience.


### How to make an accessible website or web application


To make our web applications accessible, we aim to adhere to[WCAG guidelines](https://www.w3.org/WAI/standards-guidelines/wcag/) to ensure that our web applications can be used by people with visual, cognitive, neurological, physical and other disabilities. Although these guidelines don’t ensure that our websites are 100% accessible for every user, they provide a reasonable and thorough set of guidelines that, when followed, create an accessible experience for most users.


By striving to follow WCAG guidelines, we ensure the following:


- Relevant elements in our website are perceivable.
- Assistive technologies, such as screen readers, can detect relevant content on our web pages.
- Images, icons and other non-text content have text alternatives.
- Any video or audio content is captioned.
- Users can properly navigate and interact with our web applications.
- Our web applications are keyboard accessible.
- Content is displayed so that users can understand/interact with it.
- Nothing on our web applications induces seizures or any other potentially dangerous response.
- Our web applications support alternative inputformats such as touch screens, modified keyboards and joysticks that may direct a cursor through the use of body parts like chins, hands or feet.
- Content on our web applications is easily understandable.
- Content and elements on our website operate predictably.
- Users are made aware of mistakes in forms or other validated inputs, and clear instructions on how to avoid/prevent their mistakes are provided.
- Text in any form is both readable and understandable.


### Our steps toward accessible web applications


Our web engineering team has had a strong focus on accessibility for the last few months, with our main focus being on improving accessibility in our new React codebase. Since we’re developing our web applications in React from the ground up, we now have a lot more freedom in terms of implementing best accessibility practices and providing the best experience for every user.


#### Pinpointing accessibility issues


Figuring out that your web application lacks accessibility is easy. Figuring out how make your web application accessible can be much harder. In order to pinpoint and prevent holes in our web applications' accessibility, we take advantage of a couple of tools: axe DevTools, React Storybook’s accessibility panel, Jest-axe unit testing and screen readers across multiple operating systems and browsers.


- **axe DevTools** is a Chrome extension by Deque that provides an extensive array of both guided and automatic testing tools. Our team uses axe DevTools to find and document accessibility issues in our solutions.
- **React Storybook Accessibility Panel** : Although the accessibility panel in React Storybook isn’t as extensive as axe DevTools, it’s a great supplement, and it offers a very quick and easy method of estimating the accessibility work that needs to be done on a component.
- **Jest-axe** is a unit testing library built on top of Deque's axe-core automated testing library. It tests our cp-toolkit for accessibility. These tests are automatically run on all of our components every time code is pushed to one of our remote branches hosted on GitHub. Jest-axe automated testing helps us set a minimum requirement for accessibility when designing components. Any code change that doesn't pass our accessibility tests cannot be merged with our main branch and pushed to production until any issues are fixed.
- **Screen readers** : Using a screen reader to test our components and web applications helps us get a better idea of how users with impaired vision navigate our web applications. Using a screenreader while testing for accessibility enables us to ensure that all our components are perceivable and easily understood by users with vision impairments.


### How we’ve improved


Once we use testing and tools to pinpoint and document opportunities for accessibility updates in a component or web page, we can start making improvements in the order they will have the most impact.


So far, our primary focus has been on making[cp-toolkit,](https://www.chargepoint.com/engineering/scaling-chargepoint-web-application-development-cp-toolkit/) a reusable component library used across all of our React web applications, accessible. Focusing first on cp-toolkit enables us to have the greatest impact in terms of reach, since we’re using cp-toolkit components everywhere. Most of our changes to cp-toolkit have been related to improving keyboard navigability, improving the ability for users to perceive elements and improving user understanding of components using labels, roles, descriptions and more. By implementing these updates in cp-toolkit, we’ve reduced the level of work that needs to be done on our web applications to meet WCAG 2.1 AA standards.


As for working on improving accessibility on a page level, we’ve worked to improve accessibility on our driver portal web application. Most of our focus on the page level has been ensuring components are being properly used and managed and supporting alternative page navigation to accommodate users with disabilities.


In addition to this, we’ve been working on developing detailed internal documentation on testing and developing frontend code with a focus on accessibility.


### Next steps


Despite all of the improvements we’ve been able to make with regard to accessibility, we still have lot to do before we can say our websites are truly accessible. Making our web applications accessible is a large task that takes a lot of time, learning and commitment.


We'll focus on these accessibility tasks in the near future:


- Go through new applications using axe DevTools to find and document accessibility issues. Then, resolve issues in order of impact.
- Work towards improving our existing accessibility documentation and add to it as we see fit.
- Continually keep accessibility in mind while developing web applications and components. It’s much easier to develop something accessibly the first time than track down accessibility violations and fix them later on.
- Implement certain accessibility features on the backend as well as the front end. One such example would be forming summaries for graphs and complex tables.
- Educate QA and UI/UX design teams on how to test and design with accessibility in mind. It’s important for developers to code accessible components, but they also need components that are designed and tested for accessibility.


### We’re hiring!


At ChargePoint, we’re always looking to improve our services, and we could use your help! We’re always looking for talented people interested in joining our engineering, product, or design teams. If you think you’d make a great addition to one of our teams, check out our job openings using the link below.


[Join the Charge](https://www.chargepoint.com/about/opportunities/)


Share This:
