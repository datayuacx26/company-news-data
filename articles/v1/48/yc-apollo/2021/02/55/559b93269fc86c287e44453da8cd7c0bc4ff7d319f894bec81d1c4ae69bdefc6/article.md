---
schema_version: "1.0.0"
document_id: "559b93269fc86c287e44453da8cd7c0bc4ff7d319f894bec81d1c4ae69bdefc6"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/how-we-built-odyssey"
published_at: "2021-02-24T13:59:50+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:05:13.337196+00:00"
content_hash: "sha256:da03eba5d55f6176d5bb5afb5d26f2dc4dd054d37bf5d3cdfb7085eed5d28ff5"
---

# How we built Odyssey

A few weeks ago, we launched[Odyssey](https://odyssey.apollographql.com/) , Apollo’s new interactive learning platform. Our aim with Odyssey is to provide a fun, hands-on experience for developers who are eager to begin or advance their journey with Apollo and GraphQL. To provide the interactive experience we imagined, we decided to build our own learning management system (LMS) instead of using something off the shelf. With our initial launch behind us, we’d like to share how we went about building and launching Odyssey in just three months! (And no, the answer definitely isn’t “overtime” 😉)


Odyssey homepage


## Architecture


Odyssey is a React application built with Gatsby and MDX and deployed with Netlify. These tools were an easy first choice for our team, because several other Apollo web properties (the documentation, blog, and developer hub) already use them. We know how they work, and we know they work well!


[Netlify](https://www.netlify.com/) gives us a user-friendly CI/CD with GitHub integration and quick and easy deploy previews.[Gatsby](https://www.gatsbyjs.com/) enables us to generate all Odyssey course content at build time, which is perfect for us because it’s almost entirely static. Meanwhile,[MDX](https://mdxjs.com/) enables us to author courses in familiar Markdown syntax, while also enriching the learning experience with custom JSX components like our coding challenges and multiple-choice quizzes. Instead of using an external CMS, we keep our MDX files in the same repo as the Odyssey app itself. This makes it easier for us to look back at previous iterations of a course via Git history, and to use that history to inform future improvements to our content.


Our particular approach with MDX was inspired by[MDX Deck](https://github.com/jxnblk/mdx-deck) , a library that allows you to create an entire presentation slide deck with a single MDX file. Similarly in Odyssey, we author a course in a single MDX file, and it’s automatically parsed into separate lessons when it’s rendered. Reducing the total number of MDX files like this helps us maintain a more organized file structure and provides a smoother authoring flow. When writing a course, we don’t have to interrupt our stream of thought to hop around between various lesson files — we can write everything in one flow and worry about splitting it up into smaller lessons later.


Just like in MDX Deck, Odyssey uses an` hr` horizontal rule tag to separate a course into individual lessons. Then within each lesson, we use a YAML code block with a` meta` flag to include each lesson’s metadata, similar to how we can use the file’s frontmatter to write metadata for the entire course.


Excerpt of MDX from an Odyssey course


## Styling


For styling, we used[Chakra UI](https://chakra-ui.com/) . Its built-in components helped us get off the ground quickly while providing out-of-the-box accessibility and intuitive customization. It continues to help us iterate quickly, because it’s so easy to customize the CSS. We also took advantage of[Framer Motion](https://www.framer.com/motion/) , one of Chakra UI’s dependencies, to add animations to page transitions between lessons. This gives learners a sense of progression as they navigate backwards or forwards in a course.


Sliding transitions when navigating between lessons


## Evaluating code challenges


The interactive coding challenges in Odyssey allow a learner to submit their code and get feedback about any inaccuracies:


A partially correct answer for an Odyssey code challenge


To power this, we use[Netlify Functions](https://docs.netlify.com/functions/overview/) : the reader’s code is passed to a serverless function that executes tests against the code in a Node.js environment, then returns the results to the browser. We’ll cover more about this flow in a future blog post, so stay tuned!


## Our Odyssey is just beginning


Our launch iteration of Odyssey is powered by a combination of proven, familiar tools and exciting new ones that both helped us to build quickly and confidently. And while Odyssey is currently a private repo, we may open it to the public as an open source project in the future! Interested in the nitty gritty details on any of the above? Let us know!


Our continuing mission is to help app developers like you change the world, and we hope Odyssey will help you do just that.[Begin your Odyssey today!](https://odyssey.apollographql.com/)


Written by


Janessa Garrow


[Read more by Janessa Garrow](https://www.apollographql.com/blog/author/janessagarrow)
