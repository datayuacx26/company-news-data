---
schema_version: "1.0.0"
document_id: "740cbd073cbb2f827d41cac6b7197b599f9eb24fddadb6717a5190b548e8fc77"
company_key: "groupon-inc-common-stock"
company: "Groupon Inc."
source_id: "groupon-inc-common-stock-rss-99044b78d036"
canonical_url: "https://medium.com/groupon-eng/felix-the-devops-between-designers-and-developers-c2976572e9f"
published_at: "2021-12-20T16:23:45+00:00"
first_seen_at: "2026-07-20T04:36:53.869647+00:00"
fetched_at: "2026-07-28T21:04:10.558196+00:00"
content_hash: "sha256:233701b47a0f5b3ad3881e4c777d5af316237b65b0e663b2e39135c1b2e2dbc8"
---

# Felix, the “DevOps” between designers and developers

# Felix, the “DevOps” between designers and developers


[Junmin Liu](https://medium.com/@juliu_grpn?source=post_page---byline--c2976572e9f---------------------------------------)


9 min read


·


Dec 20, 2021


--


— Building Felix, the Design System for Groupon


Several new features have been released on Groupon.com recently, such as the QR code in the navigation bar to download the app, and a banner carousel to display multiple banner messages within a single view.


Press enter or click to view image in full size


In the past, similar product features might take 2–3 sprints to complete, but now all of these features are developed and tested in a single sprint. The reason why we can finish the development and testing efficiently is mainly credited to Felix, the design system we’re building for Groupon.


## What is a design system?


Design Systems are not new to the world of product development. There are many successful cases in the industry, such as Google’s Material Design, Microsoft’s Fluent Design System, IBM’s Carbon, Adobe’s Spectrum, and so on. With their design systems, these companies have successfully changed the way they design and develop their software products by establishing a set of reusable components and usage guidelines to inform design and development. The result? Delivering faster and better product experiences to our customers, at scale.


## Felix, the early days


In 2020, Groupon’s consumer experience design and engineering teams began partnering to bring to life a comprehensive design system–now named Felix–to be used throughout Groupon’s many brands, platforms, and products.


Initially, I believed a design system to be simply a set of design specifications and component libraries — an unoriginal concept. After diving into the architectural design and project management of Felix’s development with Lead Product Designer, Michelle Witkowski, I realized Felix’s true potential: a fundamental change in collaboration between designers and developers.


This year, we have built out the initial implementation of Felix, and several of our development teams have begun leveraging it. Through the use of design tokens within Felix, we have developed themes to support Groupon’s multiple brands (Groupon and LivingSocial) and platforms (web and mobile apps). These theme styles can be changed at scale, lending support to even major brand update initiatives. Having a design system in place greatly improves our design and development efficiency, enables product strategy acceleration, and ensures that what we build is consistent with our brand vision.


> “The space to solve the tough problems really opens up when the rest is simply pointing to the right component.” — Michael Forsythe


## Felix is helping to build a culture of collaboration between Designers, Developers, and Product


When we talk about DevOps, one of the most exciting concepts is the way developers and operations work together closely to ensure applications can be efficiently built, tested, and released.


Similarly, when we talk about Felix, we must talk about the collaboration between designers, developers, and product managers. Like DevOps, a design system uncovers a culture of collaboration between product delivery’s most prominent teams and introduces the kind of seamless product development cycle once only enjoyed by DevOps.


## Designers and Developers can share a common language: Design Tokens


Before there was Felix, designers and developers were working in their own languages, in their own ecosystems, and it was difficult to communicate with each other.


The end result, most of the time, was that we ended up spending time doing pixel perfection activities after a particular UI design was implemented by a developer. This usually happened because the developers did not understand the design well, and it required both teams to communicate several times to achieve the same result as the initial design.


So, one of the most important things when building a design system is to define the design tokens. At Groupon, we share a set of design tokens for both development and design, regardless of the platform.


Press enter or click to view image in full size


A Design Token is essentially a set of key value combinations. For example, the following figure is a list of color design tokens, simple and clear. Another way to think of them is they represent codified design decisions.


Press enter or click to view image in full size


Design Tokens, Credit: Michelle Witkowski


With Design Tokens, it is simpler for designers to describe their designs and easier for developers to understand them.


> “I think working together has allowed me to better understand the focus points of designers in a new feature, so the overall process of creating something new has less iterations — a more streamlined process.” — Nida Pervez
>
>
> “Working together on the Design System has helped give designers and engineers an extra common language. This boost in communication helps to increase productivity across teams.” — Daniel Hey


Compare these two pictures below, the first one is before Design Tokens and the other one is after Design Tokens, the difference is very obvious — with the guesswork taken out of the process, developers can simply reference the tokens defined by the design team.


Press enter or click to view image in full size


## Building a Design System Team


Since its inception, Felix hasn’t had a formal team responsible for its development and iteration. Instead, a small team of scrappy designers and developers have been operating as an informal team, defining the components and building them in our spare time.


So how does this work? It’s like an internal open source project where people contribute 20% of their work time to building Felix. In exchange, we are able to reap the rewards of a design system and prove its benefits as we go..


This gave us a huge challenge at the beginning. We couldn’t do this full time and had to use resources outside of our day to day projects to commit. We benefited from extensive prior preparation work, and I had led an internal front-end component library project before. So we started to officially design and develop Felix from scratch during Q4 of 2020. The Design team defined an initial set of Design Tokens and some basic components, and I found a few front-end developers from my own team and other teams who were interested in contributing.


Despite being an unstructured, decentralized team, we operate using the same agile methodologies as other Groupon teams. We conduct bi-weekly sprints and deliver as much work as possible in each. We track all tasks and bugs through Jira, and have bi-weekly team meetings to share progress and plans for the next sprint. We also maintain a running Google Doc to track the minutes of meetings for future reference.


Thanks to the decentralized team model, there were no walls between different teams from the beginning. There was no need to distinguish between Design and Development, and we all worked together with the same goal of building Felix. Cross-team collaboration is possible, and every member of the development team can participate in it.


Another unexpected benefit of the decentralized team model is that some of the contributing teams have found opportunities to adopt Felix into their product work because of their involvement in the project.


Moving forward, we are looking for ways to further optimize our workflow regarding collaboration between Design, Development, and Product.


## Automating the build of Design System Packages


Groupon is a practitioner of DevOps, and the concept of automation is deeply rooted. We automate everything we can, from daily testing to deployment in production environments.


So when building Felix, we also integrated automation into many aspects of it.


## Automatically update and publish Design Tokens


All of our Design Tokens are managed and maintained by the Design team using a Google Spreadsheet. As shown in the image below.


Press enter or click to view image in full size


The Design team will notify the Development team when they have any update to Design Tokens, and the Development team will export the content of the Spreadsheet through automation scripts and parse it into standard JSON format, then generate different formats for different application platforms, and finally submit it to GitHub in the form of a Pull Request (PR). When the PR passes the review and automation test, automatically releases the new version to the package management system.


Press enter or click to view image in full size


## Designers are involved in updating the system with automation


Now, with the help of Felix, we have greatly simplified these operations. Designers can directly submit new SVG files in the form of PRs through GitHub.


Press enter or click to view image in full size


CI/CD will automatically publish the SVG file as an icon component when the PR is merged. The design system even generates png files of corresponding size for mobile platforms.


Press enter or click to view image in full size


Additionally, we have automation for the Design System documentation website. Designers can submit updated pull requests in markdown directly through GitHub web, and when the PR is merged, the documentation website is automatically updated.


## The automation of design test


Automation testing of UI is one of the most difficult forms of automation testing. Now, due to the abstraction of Design Tokens by Felix, all interface components are built on top of Design Tokens, and all interface properties such as color and size must be obtained from Design Tokens.


We have a plugin to check all of Felix’s UI properties when compiling the front-end components. The system will show error alerts at runtime when a component’s properties use a value outside the design token range, so we can avoid design errors the first time around.


In the code review phase, we also plan to use robots to help us find the wrong use of design tokens in the code.


Of course, we still have a long way to go for design automation testing, but Felix makes it possible to automate design testing, and we will continue to explore in this direction in the future.


## Felix makes information more transparent


After completing the first phase of Felix, we published a documentation website containing Design Token definitions, components, and component-related documentation.


With this internal Design System website–or Docsite–Developers, Designers, and Product Partners can find updated information from a single source of truth. This transparency allows developers to have a deeper understanding of the design specifications and designers to know how developers are using the components. Product can find out the latest information on what exists in Felix and if there are inconsistencies with production.


In the past, when designers provided design sketches to developers, they only provided information such as the color and size of the design, but now the design sketches are clearly marked with the design tokens corresponding to the attributes, and the relevant components come with a link to the Felix Docsite, so that developers can clearly know which components can be used directly in the implementation and how to set the attributes.


Felix makes information more transparent, reduces a lot of waste caused by information misunderstanding, and greatly improves efficiency.


## Conclusion


After a year of hard work, we have built a powerful Design System that works for us and successfully landed in the web department. The main web system has integrated Felix, which has greatly improved the efficiency of design and development in new projects.


The success of Felix is not just that we have rebuilt a set of component libraries or that it has a set of Design Tokens, but that it is a more automated way to build the UI of the system, makes the information between designers and developers more transparent, and most importantly, the design system builds a better culture of collaboration between designers, developers and product. These are the same three important principles of DevOps: Automation, Information transparency, and building a culture of collaboration.


The reason why projects can be delivered efficiently based on Felix is that developers and designers already work closely on a daily basis, and developers are already familiar with the Design Token specifications through the Felix Docsite, so they don’t need to repeatedly confirm details with designers when implementing the UI. Felix’s rich library of components can help build the UI semi-automatically, and the included automated detection helps find possible interface errors before submitting the code, avoiding subsequent rework due to UI errors.


Gradually, Felix is changing Groupon’s R&D culture, making the collaboration between Designers, Developers and Product even better.
