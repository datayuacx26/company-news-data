---
schema_version: "1.0.0"
document_id: "01b1a43d02e5713e28d2ac48d6d69e7652638699790fe50c734bdd4675cc6e77"
company_key: "yc-nova-credit"
company: "Nova Credit"
source_id: "yc-nova-credit-rss-8f3179f5a680"
canonical_url: "https://medium.com/@nova-credit/building-a-design-system-backed-shared-component-library-b05fc8f2ba2f"
published_at: "2021-10-26T19:36:05+00:00"
first_seen_at: "2026-07-25T16:35:22.821563+00:00"
fetched_at: "2026-07-28T22:26:35.155530+00:00"
content_hash: "sha256:ffdbc7a6dcddafdff0e03a3382d2edcce72d6c5cb8447e6efcba06aeea34ff49"
---

# Building a Design System-Backed Shared Component Library

# Building a Design System-Backed Shared Component Library


[Nova Credit Engineering Blog](https://medium.com/@nova-credit?source=post_page---byline--b05fc8f2ba2f---------------------------------------)


4 min read


·


Oct 26, 2021


--


by[Marissa Sileo](https://www.linkedin.com/in/msileo/)


Press enter or click to view image in full size


Photo by[Sigmund](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral) on[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)


Here at[Nova Credit](https://www.novacredit.com/) , our path to a shared UI component library has been long and winding, often going down one route only to discover we had taken a wrong turn. A recent inventory of our frontend apps revealed that we had four apps implementing their own version of the basic UI components, two utilizing Semantic UI and three making use of what we’ll call V1 of our shared component library.


On a positive note, we had the foresight to create a shared component library early on, which, though slightly out of date, has the structural roots required by any good component library. However, its infrequent usage and lack of broader understanding across the engineering organization led new team members to build out their own versions of these components or utilize libraries more familiar to them.


In the early stages of any startup, it’s not been uncommon to embrace Mark Zuckerberg’s “move fast and break things” attitude, which definitely factors into my diagnosis of what happened with our frontend apps, their unique designs and ever-so-slight differences in component implementations.


As the company has grown and matured, we’ve begun to understand how these individual components are a reflection of the company itself, in their design, usability, and consistency. For our customers and consumers, trust is built with each interaction they have with us, and that includes their interactions with the products we’ve built. In light of that, we’ve made it our mission to provide the best experience with our products and that starts with our design system.


## Design System


A design system goes beyond a simple pattern library. It needs to include a set of rules and guidelines to give the included components structure and meaning.


Ours is built by our design team here at Nova Credit and is based on[atomic design principles](https://bradfrost.com/blog/post/atomic-web-design/) . It will always be a work in progress, but the initial atomic level components have helped shape our approach to building out V2 of our shared component library and will be the backbone to all of our product designs in the future. Utilizing[Figma](https://figma.com/) as our chosen tool to share design files between the design and engineering teams, we were able to create a feedback loop for the initial elements and transition smoothly to starting the development process.


## Design Tokens


With an arsenal of elements in need of implementation, it was important for us to not be too eager to immediately start building but rather to focus on ensuring we have a clear and easy way to keep a source of truth for the foundational elements of the design system. Things like spacing and typography values, colors, and so on.


Most of us have utilized[CSS variables](https://www.w3schools.com/css/css3_variables.asp) or CSS preprocessors which support variables such as[Sass](https://sass-lang.com/documentation/variables) during our time working on frontend code, which worked fine when these were still the most commonly used formats for styling HTML. With the rise of things like CSS-in-JS and CSS Modules, it became more of a challenge to limit codebases to just one Sass/CSS variables file. We began to see corresponding files created in different languages, such as Javascript, and all the time these files were growing larger and larger and becoming more difficult to maintain.


Ultimately, we discovered the concept of design tokens (read more[here](https://amzn.github.io/style-dictionary/#/tokens) ) and a useful library for generating them:[Amazon’s Style Dictionary](https://amzn.github.io/style-dictionary/) . This allows us to define a token to store any value (a color’s hex code, a pixel value for spacing, a rem value for a font size, etc.) in one place and transform it into a variable in a vast number of languages/file formats. For our specific purposes, we’re able to generate JSON, Javascript and Sass files. We’re even able to split the tokens into separate files based on a category, allowing us to import partial files like \`_colors.scss\`.


Design tokens have provided a way to guarantee that there is a sole blueprint for the most basic of values used throughout the components in our shared library and have truly been a game changer thus far.


Press enter or click to view image in full size


Demo of Nova Credit components on Storybook


## Storybook


The last piece of the puzzle is the tool that so many know and love:[Storybook](https://storybook.js.org/) . With Storybook, engineers have a way to build out and locally test components without having to first import and use them in existing frontend applications. Testing changes is isolated to Storybook’s UI, where mocking out different states and edge cases is made simple with Storybook’s[Addons](https://storybook.js.org/addons) .


At Nova Credit, we host the latest version of our Storybook site to provide the design team and other relevant stakeholders the ability to validate and provide feedback on newly built or updated components and of course, to inspire more discussions around existing and future designs.


## Pandora’s Box


Together, these different tools and libraries make up our shared UI component library, which was dubbed in the early days and has been known ever since as *Pandora’s Box* . We’ve got a thing for Greek mythology here… it’s a long story. The point is, while we hope that these recent developments have sparked curiosity and strongly encourage everyone to open up Pandora’s Box and take a look inside, we hope that what they’ll find is a blessing rather than a curse, as the myth goes.


*Nova Credit is hiring! Check out our*[careers page](https://www.novacredit.com/careers) ,[current job openings](https://jobs.lever.co/neednova) *, and*[engineering values](https://www.keyvalues.com/nova-credit) *.*
