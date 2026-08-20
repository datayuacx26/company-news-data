---
schema_version: "1.0.0"
document_id: "7e77d4e17056ecfb930556c5a31d0616d07a508231747dc19ba8ca1b086e7971"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2016-02-02-large-scale-css-refactoring-at-trivago/"
published_at: "2016-02-02T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T22:27:32.834035+00:00"
content_hash: "sha256:c4cdd97de82ceedc5751a089051d879e3d25154d701d34bf8c390988660db3a5"
---

# Large Scale CSS Refactoring at trivago

### Project Ironman: the technical migration of the trivago hotel search's existing CSS core


While our company and our application were constantly growing, we often ran into some consistency issues between code and design. Because we didn’t have a design/frontend system and development guidelines to follow, our UI became cluttered and unsustainable. This caused maintenance issues, slowed down our UI Development and caused us to produce technical debt with an overcomplicated CSS/DOM structure. In the end, we were afraid of spending our time on figuring out crazy rendering bugs rather than rapidly developing new UI features. I gave a[lightning talk](https://speakerdeck.com/creinartz/about-consistency-lesson-learned-the-hard-way) at the beginning of 2015 to present what we had learned at the[LeanUX Meetup](https://www.leanuxdus.de/) in Düsseldorf.


A tool like[CSS Stats](http://www.cssstats.com/) can easily help you figure out consistency issues within your codebase. Indicating what can happen when everybody has different opinions on how a grey tone should look like, you will end up with 50 shades of grey.


Moreover,[Specificity Graph](http://csswizardry.com/2014/10/the-specificity-graph/) gives you a good overall indication of your CSS base’s health. Peaks are generally bad, especially at the beginning of the graph. What we want to have is an upward trending graph. The x-axis defines the position of a selector in the stylesheets, while the y-axis defines the specificity of the selector.


An extreme example is our graph from 2012. I have no words for that.


Of course we tried our best even without having a defined design system in place and we tried to constantly fight against any code that seemed to be rotting. We carried out face-lifts and maintenance cycles and anything else that could improve our graph. But we still had consistency issues and we had peaks at the beginning of the stylesheet which led to maintainability issues.


### We needed a design system


After this long journey, at the beginning of 2015 we began establishing a design/frontend system and improving our design-development workflow. We introduced a[pattern library](http://www.patternlab.io/) , started utilizing[atomic design](http://bradfrost.com/blog/post/atomic-web-design/) in our workflow, created new coding guidelines, and adapted several methodologies like[BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/) and[ITCSS](http://itcss.io/) in order to support us in maintaining and developing our CSS/UI on a large scale.


Some of our top-level goals in creating a design system were:


- Ensure consistency in code and design in all our applications.
- Reduce technical debt.
- Keep our application scalable.


In less than a year, we were able to develop the base of a new frontend/design system, created our own CSS framework and started to redesign and re-implement some of our applications such as the trivago company pages.


The[trivago company](http://company.trivago.com/) pages before using a frontend/design system:


The first version of the company pages built up upon a new design system:


Nevertheless, implementing a new CSS/frontend foundation in our core product - the[trivago hotel search](https://www.trivago.com/) - was a completely different challenge. Around 100 people work on this application, including developers, product owners, and designers, and many more have a stake in it, so there is no easy way to create smooth (agile) transitions and keeping (product) development freezes - which are normally caused by refactoring - as short as possible.


We as developers would have liked to rewrite the whole core product’s user interface, but of course this would have taken too much time. We had to somehow integrate the foundation of the new framework without interrupting the existing feature development process in addition to rebuilding old UI components and to start implementing new UI features. As we were using the ITCSS methodology, we decided to squeeze in all layers up to and including the components layer and remove the old CSS foundation. We had to change the core foundation of our CSS base but keep all layout, design, and functionality as it was before.


### The three Disciplines of Project Ironman


This had three major implications on our user interface. The new CSS foundation was built with a mobile first approach and prefers min-width queries whereas the exisiting website was built with a desktop first approach and therefore required max-width queries. Furthermore for better maintainability we decided to move the media queries inline, after we had initially moved them to separate files, because of asset file sizes. So the first step was to rewrite the media queries from a min-width perspective and re-inlining them into the CSS core.


Our existing CSS core was founded on a reset.css while the new framework uses a custom build of normalize.css.


Additionally, due to the fact we supported IE7 until the beginning of the year we still used content-box as our default box-sizing setting. To achieve this we would have to rewrite a lot of widths and heights and adjust a lot of default styling.


This was the second step, replacing the reset.css with normalize.css and switching default box-sizing to border-box.


One of our major pain points was the usage of image sprites for our icons. They were completely cluttered and became unmaintainable as we had several versions of them, with variations for mobile, high-resolution, and some which were only used when testing new features and were no longer needed. We decided to use embedded SVG icons with an embedded PNG fallback which we built with[grunticon](http://www.grunticon.com/) . In the end, this step allowed us to remove around 800 icons and more than 2000 background-positions from the stylesheet and adapt all occurrences to use the new SVG icons.


So as a last step we had to get rid of our image sprites.


In short we had to refactor > 50.000 Lines of SCSS Code, adapt dozens of templates and test the whole application on all possible device and browser combinations once again.


Wuu. What a technical challenge. You can imagine that there was time pressure to get this done quickly as we cannot afford to stop (UI) feature development for a long period of time due to business reasons.


### Getting things done


We were able to convince the management - and ourselves - that we could do this in a timeframe of four weeks and to do so we built up a dedicated project team of incredibly committed and driven people from across Software Engineering, Quality Assurance, and Design.


Since estimating this whole project would probably have taken longer than it would to actually do, we decided against bigger estimation rounds and trusted our own commitment, our passion, and our gut feelings. This was the project we always wanted to be able to do and now this was our chance. We were so committed that it was clear that we will get this thing done as soon as possible. Communication and clearly making the progress and any upcoming issues visible to the whole company were our only weapon. We decided to build up a very simple Kanban board, established a project stand-up and a project Slack channel and kept management and the company up-to-date via our internal socialcast network.


We delivered. After three weeks, 800 commits, and more than 50,000 lines of refactored code, we were able to release the technical migration as an A/B Test. We tested the migration for one week, with positive results on mobile devices where mobile first paid out and accepted the migration after only four weeks.


The following infochart visualizes some of the important numbers of this project e.g. the numbers of coffee needed.


### The outcome


From the technical point of view our main achievements / improvements were the following:


- we reduced > 10% of CSS asset file size
- we removed 47 image sprites and put in our new sharp SVGs
- we reduced the LOC of SCSS by more than 20%
- we saved in average more than 6 HTTP requests for image sprites
- we improved the render performance on smaller viewports due to the mobile first approach
- we improved our overall quality of the CSS base
- we enabled ourselves due to better encapsulation of the new CSS framework an easy and smooth update process of the Core CSS


The Specificity Graph after Ironman shows exactly what we wanted to achieve with Project Ironman. A maintainable, healthy and sustainable UI-Layer base for future developments and frontend/design iterations.


I did a talk about the whole project at the[OpenTech School Meetup in Dortmund](http://www.opentechschool.org/dortmund/) . If you want to have a look into the slides you will find them[here](https://speakerdeck.com/creinartz/project-ironman-large-scale-css-refactoring-at-trivago) .


Last but not least it is worth to mention that the not the technical challenges nor the implementation made this project so incredible successful. No it was the team, the spirit, the commitment and the passion of the whole involved project team and the dedication they put into the implementation. I have never experienced such an awesome engineering culture. Are you curious? Maybe we are looking for you right now? Have a look at our second iteration of our[job pages](http://careers.trivago.com/) built upon a Design System :)
