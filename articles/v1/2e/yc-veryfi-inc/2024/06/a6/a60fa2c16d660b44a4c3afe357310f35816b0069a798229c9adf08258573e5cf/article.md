---
schema_version: "1.0.0"
document_id: "a60fa2c16d660b44a4c3afe357310f35816b0069a798229c9adf08258573e5cf"
company_key: "yc-veryfi-inc"
company: "Veryfi, Inc."
source_id: "yc-veryfi-inc-news-import-dba8fa8c07ed"
canonical_url: "https://www.veryfi.com/engineering/lessons-from-music-and-coding/"
published_at: "2024-06-17T18:24:17+00:00"
first_seen_at: "2026-07-24T06:07:32.032518+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:bfb069bac1627cd496794794174b71db238896f48fa797dc7a9699230d67f092"
---

# Bringing Ideas to Life: 1 Note at a Time, 1 Bit at a Time

Building software and making music go hand in hand. In both instances, the objective is to bring an idea to life, either by writing code or by playing notes. To accomplish this, tools and technologies are needed. In the case of programming, an editor is needed along with 1 or more programming languages. In the case of music production, a digital audio workstation (or DAW) is needed along with 1 or more instruments, plugins, and kits. Both require careful planning and consideration. Each decision that is made along the process has an impact on the final product. Once an initial version is ready, feedback is collected, and then iterations are made. At the end of the process, you are left with, what was once an idea to, now, a product that can be shared with the rest of the world.


Hi, I’m Jared! I’m a Senior Web Engineer at Veryfi. Outside of programming, I also have a huge passion for music production. When I first joined the Veryfi team almost a year ago, I was assigned the task of migrating the existing signup flow for our web API Portal. The objective was to not only change the look and feel of our existing flow, but also to establish a new set of tools and technologies that would serve as the foundation for future products and migrations to come. In this article, I will walk you through the process I went through to bring this new signup flow to life and the lessons I learned along the way.


## Legacy Tech Stack


Before producing a song, careful planning and preparation are required. There are alot of factors to consider, such as what key to play the song in, the tempo, choice of instruments, and the arrangement. In order to build the new signup flow, the same level of in-depth planning was required.


After joining the company, I spent some time researching how the API Portal was initially set up as well as what technologies we could leverage for future products and migrations to come.


The initial version of our web API Portal was built with a legacy tech stack. On the frontend, our pages were built with HTML, CSS, JavaScript, jQuery, and Bootstrap. The frontend would use a modular pattern, in that each JavaScript file was its own separate module. When rendering a page, a global config object would be created and each imported module attached a different set of methods to that global config. These methods would be used to make API requests and cache data. To generate dynamic templates, we used Handlebars.js. Additionally, the web backend server was built with Python and Flask, and the entire application was containerized with Docker.


Eventually, the web team decided to adopt React, which led to a small number of pages on the API Portal being built with React. The initial React application was somewhat limited in that it only used JavaScript (rather than TypeScript), had a small number of unit tests, did not support hot module replacement (HMR), and used Webpack to ultimately generate a single large production bundle that would be served with each Flask template.


## New React Application


Feedback is crucial. In the music production process, it is important to collect feedback consistently from others so that gradual improvements can be made to the song. Eventually, after numerous iterations, you end up with an end product that you are ready to share with the rest of the world.


For the signup flow, once I completed my research, I presented a formal design document to the team to collect feedback. After numerous back and forth discussions, we were ultimately able to come to a decision on the web tech stack we wanted to use for the foreseeable future.


The plan was to create a brand new React application, separate from the one that was initially created, which would leverage our ideal setup. For the frontend, we decided to use React 18 (latest version of React at the time), TypeScript (over JavaScript) for type safety, and SASS modules. For unit testing, we adopted a strategy of writing 1 unit test per component and also leveraging React Testing Library. For bundling, we initially explored using Vite. However, due to Vite’s limitation of only working in browsers that support native ES modules, we decided to continue to use Webpack. Unlike the previous React application, which would create 1 large bundle with Webpack, the new React application was set up to create separate small bundles for each HTML template, which would drastically improve performance. Webpack was also configured to support hot module reload (HMR), so that any change made within the Docker container would automatically get reflected without having to manually rebuild the application and do a hard refresh in the browser. Additionally, we adopted a cache busting strategy to automatically cache bust new bundles that were served to clients after each deployment by appending a dynamically generated query param to the end of each bundle url. To improve the modularity of our web backend, we also decided to adopt Flask blueprints as well.


## Signup Flow Migration


Instruments, plugins, samples, and drum kits serve as the foundational pieces to create a song. Each music producer has their own arsenal of tools, which ultimately enables them to create their own unique sound.


With our new React application set up, I now had the tools necessary to migrate the signup flow.


This migration in particular was challenging because the existing signup flow, which was built with the legacy tech stack, already had its own set of template files, JS bundles, CSS styling, API endpoints, and strategies for state management, error handling, and url redirects.


Here are some of the key lessons I learned from the migration:


### Lesson 1: Understand the needs of the business before deciding on a component library


One of the major decisions we had to make was whether we should use a pre-existing component library, such as Material UI or React Bootstrap, or create our own.


We initially decided on creating a custom component library that was built on top of headless components, such as those from Radix UI. The main benefit of this approach was that we had total control over our design system. Each component could be easily styled and customized to adhere to our brand guidelines. Also, with the use of headless components, accessibility support could easily be baked into each component as well.


Eventually, after spending some time building these custom components for the new signup flow, we realized that it would take too much time to build out a custom component library. As a startup, speed is a priority. The aim is to build products quickly, ship them out to our customers, collect feedback, and iterate. Building a custom component library in parallel to product development work would take too much time, especially since we did not have a dedicated design system team.


As a result, we decided to change course and instead leverage Material UI as our component library. Initially, our concerns with Material UI were the large bundle size, somewhat complex API, and it being not easy to customize. However, we decided we were ok with this tradeoff since Material UI features a large set of accessible components out of the box that are well documented.


Now, by using Material UI, we could focus more on building and shipping products quickly rather than implementing our own set of custom components.


### Lesson 2: Use feature gates for migrations


When performing a large migration, such as a signup flow migration, there should be a way to easily disable the old flow, enable the new flow, and roll back to the old flow in case any issues come up post release.


We were able to accomplish this through the use of a feature toggle, which was essentially a server-side check to determine whether we should serve the old signup flow template files with JS modules or the new ones with the React app bundles based on whether or not the new flow was enabled in a particular environment. This feature flag made it very easy to gradually roll out changes for the new signup flow, while making sure the old flow was unaffected.


### Lesson 3: Keep everyone in the loop


Keep everyone in the loop as much as possible so that they are aware of the changes early on.


For example, while working on the signup flow migration, there were a couple instances where I noticed that our backend team needed to support a new set of APIs. The earlier this is detected, the earlier the respective team can plan to implement and roll out the necessary changes, without impacting the target production release deadline.


## The Road Ahead


As a music producer, it is important to constantly be learning and evolving. New software plugins and kits are getting released constantly. New techniques for mixing and mastering are continuously being discovered and shared. With each generation comes a new group of artists and producers, each trying to create their own unique sound.


Now that our new React application has been set up and our signup flow has been migrated, we will set out to gradually migrate other areas of our web API portal. While we undergo this migration, we will continue to make updates and improvements to the React application itself, including eventually upgrading to React 19, using the React compiler, and also incorporating react-query as well. We also plan to enhance our testing capabilities by using Cypress for integration testing.


While the road ahead may be challenging, I am excited for what is yet to come.
