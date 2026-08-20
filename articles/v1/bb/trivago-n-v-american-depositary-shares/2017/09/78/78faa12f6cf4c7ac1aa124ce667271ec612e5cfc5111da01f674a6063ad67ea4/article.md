---
schema_version: "1.0.0"
document_id: "78faa12f6cf4c7ac1aa124ce667271ec612e5cfc5111da01f674a6063ad67ea4"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2017-09-26-accessibility-at-trivago/"
published_at: "2017-09-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:25.846890+00:00"
content_hash: "sha256:8af8b075540d37e1eb81ce99faf8c5ab6950359d3767cf10da2b402865b11013"
---

# Accessibility at trivago

Accessibility is an important topic for anyone who builds things for the web, and one that is neglected far too often. We at trivago have also been guilty of this, but we are slowly making changes with the aim of improving the accessibility of our site. Identifying and implementing these changes has not been easy. We have faced a number of challenges along the way, and we continue to do so. But we are committed to improving our site so that anyone can access and use the service we provide, regardless of how they do so.


## Internal Challenges


If you looked at our site a year ago, it was completely inaccessible to users who attempted to access our site with anything other than a mouse or touch screen. This had to change and a small number of our UI developers who care about accessibility decided to see if they could improve things.


From the outset there were a number of issues. Some people in our department either lacked awareness of the topic or they didn’t see the need for changes to be made, as they didn’t want part of a sprint to be taken up on accessibility issues when something “more important” could be implemented instead. Management also showed some indifference towards any improvements as there was no tangible way to measure how many users were impacted. One of our company values is “power of proof”, and while it is fairly easy to measure the number of visitors to our site who use outdated browsers which enables us to make informed decisions on their support, it is virtually impossible to measure visitors who are using tools such as screen readers or who want to use the keyboard as a main method of navigation.


Overall there was a mindset issue, where people displayed apathy towards making accessibility improvements.


## Bottom-up Approach


This sounds like it was a showstopper, but it was not, it just made improving things more difficult, but not impossible. Due to the way trivago works internally, and especially within the software engineering department here in Düsseldorf


, if developers believe in something strongly enough, they can still do it, pushing the topic from the bottom up.


So this is what was done.


### Increasing Awareness


As mentioned earlier, part of the problem was lack of accessibility awareness; what it means, how it can affect people, and what can be done to improve things.


Increasing awareness was tackled in a number of ways. Internally, once a year, we have a two day hackathon which allows people to work on something they think will benefit trivago. One team decided to work on a bare bones implementation of the trivago website with improvements for screen readers. At the end of the hackathon teams presented their work, and the presentation of this short project was used to compare the different experiences between our live website and the cut down, more accessible version.


Additionally, well-known accessibility consultant[Karl Groves](http://www.karlgroves.com/) was invited to give a[trivago Academy talk](https://www.facebook.com/events/857617024340127/) on accessibility. His talk entitled “I never knew a website could hurt somebody” helped increase awareness of why taking care of accessibility is important for users.


With this increased awareness, it was now time to act.


### Small Steps


As they were performing other tasks, UI developers made small changes that constantly improved the site’s accessibility. Existing HTML semantics were improved, and colour contrast changed, where possible. The excellent[pa11y](http://pa11y.org/) tool was used to identify[WCAG conformance level AA](https://webaim.org/standards/wcag/checklist) errors that existed and to keep an eye on any new ones that crept in.


Overall, developers improved things by:


- replacing meaningless` <div>` s with more meaningful elements where appropriate
- ensuring that all buttons use` <button>`
- converting all links to use` <a>`
- adding labels to all form elements
- replacing custom elements with native elements where possible
- introducing WAI-ARIA attributes and roles where required


Many of these things, one could argue, should have been done from the start, but remember that this is a large codebase that has had many developers working on it over time, some of whom were more experienced in other technological areas, so the HTML produced was less than optimal. Any frontend developer who works in a large company with an ever-changing codebase can probably empathise with this. Now that there were a number of developers who specialise in HTML and CSS, things could be improved.


The number of WCAG errors was tracked via the[pa11y dashboard](https://github.com/pa11y/pa11y-dashboard) which gave a visual representation of the work that was being done. This work and improvements were promoted throughout the company, and received general approval. At the start we had over 250 errors, and after all the above work had been carried out, this was reduced to about 70.


### Using the Keyboard


Another topic that the UI Developers were keen on improving, was the ability to navigate the website using the keyboard. Our site has many interaction areas where using native elements alone wasn’t going to make them automatically usable via the keyboard.


Due to the increased awareness mentioned earlier, and the actions that were now being taken – as developers were talking about the accessibility work they were doing in their daily stand-ups – accessibility was beginning to rise in prominence, and support was arriving from the Product and other teams. This meant that getting time to work on keyboard accessibility within sprints was easier, as people were beginning to see the benefit.


After discussions between developers and Product Owners, it was decided to make the main functionality of the website usable via keyboard. Again this is a small step, as it doesn’t make the entire website accessible, but every improvement helps, and this was an excellent way to continue improving the site’s accessibility.


The main functionality that was made keyboard accessible was roughly defined as allowing the keyboard user to:


- enter a search term
- enter start and end dates
- use the room configurator
- perform a search
- navigate the results list and perform a click-out to a required deal


This keyboard functionality was to be bolted on to the existing functionality, and it took a lot of time, and a lot of learning: finding out how a calendar should work with the keyboard, what key combinations to react to etc. A number of other items also had to be blocked from the keyboard (e.g. the filters) as most of them were not keyboard accessible or only partially so, and it was decided to skip these things entirely for now to focus on what can be done.


Focus states also had to be introduced as these didn’t exist on most of the site, and in order for the “focus states look ugly” argument to be quashed, a standard design for this was provided by a member of the design team and implemented.


When this implementation was complete, as with all changes, it went through a thorough code review and QA cycle, and finally went live earlier this year , but was A/B tested of course as all our changes tend to be.


It performed positively enough for it to be accepted, and the coverage was increased to 100%. Again this achievement was posted and promoted internally throughout the company so others could see the improvements that were being made and awareness of accessibility’s importance was increased.


### A Minor – but necessary – Setback


A few weeks after the keyboard accessibility functionality went live, it had to be completely switched off again rather than accepted into the codebase. This was of course unfortunate, but it was a necessary evil at the time. One of the major parts of the website was being completely re-written using our Melody framework, and unfortunately keyboard accessibility wasn’t part of the rewrite at the time due to time constraints.


This meant that when it was ready to be A/B tested, we needed to compare like for like, so all A/B tests on this area of the website were turned off, and this sadly included the keyboard accessibility.


It wasn’t all bad though, as there was a firm commitment to add keyboard accessibility to the new version at a future date, so it was a necessary evil with a happy ending.


## A New Focus, with Results


Within trivago there was also a shift in focus, so that everything that we created should concentrate more on the user, and create user value. Naturally this change was a major boost for accessibility, as every improvement in this area brings user value.


This meant that a lot of the existing accessibility work that was already done was seen in a new light and the user value that it brought increased in appreciation:


-


The keyboard accessibility functionality was re-implemented, and we are pleased to say that it has been live for a number of weeks now.


-


The filters area was rewritten with a lot of care and attention, to ensure that keyboard accessibility was implemented as part of the rewrite. This too is currently live and being A/B tested.


-


Our designers worked on implementing a new colour palette around the green used in the trivago website, as it wasn’t WCAG AA conformant. Thankfully this is[no longer the case](https://medium.com/@lifeattrivago/contrast-negotiations-how-our-digital-product-designers-are-tackling-web-accessibility-2755108e9a01) .


-


pa11y and[Tenon](https://tenon.io/) are becoming part of our continuous integration workflow, with errors and potential issues being flagged. We issued pull requests for pa11y to include functionality that we needed.


-


QA are currently writing tests for keyboard accessibility so it becomes part of their automated testing process.


Many departments have worked together to achieve these results and to push things forward, which is an achievement that we are proud of.


## Work to be Done


We have come a long way. A year ago the site was completely inaccessible, but things have improved greatly, but of course we still have lots to do.


Making things keyboard accessible *IS* time consuming and does require extra effort, but it brings enormous user value and is worth the time dedicated to its implementation. Not only is it the right thing to do as it allows more people to access our site, it also increases our potential audience and therefore revenue.


Up to now we have focused on semantic HTML and keyboard accessibility, and we will continue to do so as there are still areas of the website that need improvement. We do need to spend some time improving the experience for users who use tools such as screen readers, and that will come in due course.


There is still some developer apathy which we need to overcome, but internal promotion and code reviews play a huge part in improving things and educating each other. We also intend to run some internal training and workshops to share the knowledge.


Third party markup causes a number of headaches, as often what they produce isn’t conformant and we have no control over it. We will continue to raise bug requests with them to try and improve this.


We have come a long way. We have a long way to go. But things are improving for the better. Long may it continue. We will keep you updated with our progress.
