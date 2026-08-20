---
schema_version: "1.0.0"
document_id: "ac7760c530aadab4021cc5f0c13cd18578ab3062fd6c0ba526769b3318b93c79"
company_key: "yc-decentro"
company: "Decentro"
source_id: "yc-decentro-rss-d020e209c049"
canonical_url: "https://decentro.tech/blog/why-next-js/"
published_at: "2022-12-08T14:07:14+00:00"
first_seen_at: "2026-07-25T01:09:32.803029+00:00"
fetched_at: "2026-07-28T21:02:37.132176+00:00"
content_hash: "sha256:dee9d0398465d519851c8943e45bf66af0ccc3c71ec0c6c5a3e548968eb62c08"
---

# THANKYOU, NEXT(Js): Why NextJS is Vastly Superior To React

Trial and error.


The base of any tech-first startup looking to scale sits comfortably behind the iterations of each code deployment.


When you start a product journey, you build up to perfection.


You build to execute. And an error upon execution is only natural.


You learn from it, iterate and then move on to the next deployment.


Sometimes the iteration may be a code change or just a migration of a tool altogether.


It is this migration that we will delve into this time.


We will see how we used React to build our website, discover its shortcomings and then move to NextJs to solve most of those problems.


First of all, Lets us know the most fundamental difference between these two:


React is a component-based open-source library built by Facebook
Next is a framework built on top of React for React


Now for comparison points.


Comparison Points


**Rendering:**
Let us see how the rendering process works on both:


**React:**
React supports Client Side rendering only.


Next on the other hand is known for its server-side rendering & static generation capabilities.


Now, what is client-side rendering & what is server-side rendering?


In ***Client Side Rendering*** , the request is redirected to a single HTML file. The server delivers it without any content until all the JavaScript is fetched and the browser compiles everything before rendering the content.
In React, It takes extra effort to integrate SSR with your preferred server and setup.


Whereas ***Server-side rendering*** means using a server to generate HTML from JavaScript modules in response to a URL request.
The server receives a request, parses the Javascript modules & data required to generate a response, and returns a rendered HTML page to the browser.


Say, we have a React application on production, and the client requests a URL, and the request will be passed on to the server. On a handshake, the server will deliver a single index.html file to the Client. The index.html file contains bundle.js (compiled js). This bundle file contains all the React components, routing rules & the page layout. The Client executes this JS first and understands what the page should look like.


Here the server has no significant role where it can help in improving the speed & performance of the app.
What if the bundle.js file is executed on the server side itself instead of on the client side, and then a single HTML document is sent to the client to render directly?
Since browsers are very optimized in displaying the HTML part quickly, as compared to a JS document which constructs HTML from scratch, this makes the app faster.


Next is super fast as compared to React.


We found out about this issue on our Indian website, updated our stack to Nextjs for the Singapore website version, and boom! On load itself, you can feel the fast loading of the application.


This also affects **SEO** .


Next is fantastic from an SEO point of view as well.
Various search tools, such as Google, Yahoo, and others, use optimization principles. All these engines analyze each website they know and can reach and categorize them. As a result, each webpage has its characteristics and keywords.


React Case:
Consider a single-page application (SPA) developed using React. Whenever a user opens a website, it generates a single http file. When the user requests a new page, a new HTML file is generated.
So all other pages do not exist until the user renders them by opening these pages (classic Client Side Rendering case)


Got the problem? It results in poor SEO, since these pages do not exist without the user, so search engines cannot analyze/identify these other pages. These pages will struggle to be promoted by Search Engines.


Is **Next** the solution?
Yes, particularly the Server Side Rendering concept in Next resolves this issue.


Each page on the website has its own HTTP file, which is already rendered and does not depend on other actions like the visitor’s opening.


We all know about website Metadata.


Metadata is probably the most important aspect of SEO. Remember to write a <head> on top of every html file you wrote.


Metadata provides search engine crawlers and specified bots with important information about the content and purpose of each individual page on our website and helps them gather and categorize the metadata and general information & determine whether our website is relevant enough to display in search results.


*Code Snippet:*


In Client side rendering these meta tags were not read by crawlers and were highly impacting our SEO. Once migrated to React this issue was solved.


One more example we remember is while making social media posts with the webpage link embedded in them. Most social media platforms display basic previews of the contents in the link Eg: Banner Image, title, and description, so If interested the user can click the link and look at the whole article. The meta tags are also responsible for this preview.


The <meta> tag with properties ***og:title*** , **og:description,** and ***og:image*** is responsible for this preview. Since in React these meta tags were not properly recognized by social media crawlers & previews were not proper, simply migrating to Next solved this issue.


**Optimized Images:**
Remember, setting up a ***<filename>.lazy.js*** file and configuring it in **React** to implement Lazy Loading for Images on the webpage or spend time finding different third-party image components or packages for faster image load, serving images through CDN & other magic tricks for minimizing layout shifts, and more.


Say No More!
Welcome **Next** ! Next has a built-in Image Component next/image to do the same and without any other extra line of code and time going into configurations.


Next/image, is an extension of the HTML <img> element. It includes a variety of built-in performance optimizations to help you achieve good Core Web Vitals. These scores are an important measurement of user experience on your website and are accounted for by Google’s Search Rankings.


Below are some of the optimizations the built-in Image component includes:


Types of Optimisations


1. **Faster Page Loads:** Solves the lazy loading issue. Images are only loaded when they enter the viewport, thus the page doesn’t have to wait for all those bulky images on your page to load. Images are only loaded when required.
2. Prevents Cummulative Layout Shifts, thus improving the User Experience through **Visual Stability**
3. Correctly Size Images are served for every device every time, using modern image formats thus **Improving performance**
4. Provides **On-Demand Image Resizing**


Some other features include adding a **priority** prop to the image that will be the Largest Contentful paint element for each page. Doing so allows Next.js to prioritize the image for loading especially.


*Code Snippet*


Or


Cool, Right?


Let’s dig out one more!


**File-Based Routing Mechanism:**
Remember setting up a routes file & using react-router in **React** Apps to define rules stating what page to serve on what URL request.
That doesn’t work for **Next** . Next is its boss, the way a developer sets up the page structures (individual or nested), and routes are automatically created based on them.


This may not seem a big issue here. Still, it saves a lot of development time since the file names and their structure define the route without using any third-party libraries and setting up a well-defined file structure for large applications.


Eg: If the pages folder contains an about folder with an index.js file in it then the about us page can be accessed using /about as this route is automatically set up in the Next application.


The advantages highlighted above paint a picture of our product journey. Not brainwashing anyone to use Next over React; however, the plethora of offerings in the form of SEO assists and UI flexibility does present a compelling case to build a bias in your mind. Now it is for you to decide on your preferred tool.


No pressure. Just facts.


With that, we have come to the end of an immersive read elucidating the world of React and Next. The developers at[Decentro](https://decentro.tech/) , incessantly work on getting their subject matter expertise to you, via these technical write-ups. Feel free to check out our workaround,[Data Tables,](https://decentro.tech/blog/guide-to-datatables-the-superman-of-data-management/) and[Metabase](https://decentro.tech/blog/guide-to-datatables-the-superman-of-data-management/) , and do leave suggestions of what you wish to read next.


In case you wish to connect with us, feel free to drop us a line athello@decentro.tech


[Let’s Connect](https://decentro.tech/signup?)
