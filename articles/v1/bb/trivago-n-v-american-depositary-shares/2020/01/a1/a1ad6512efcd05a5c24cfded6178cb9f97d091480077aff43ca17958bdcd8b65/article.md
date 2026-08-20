---
schema_version: "1.0.0"
document_id: "a1ad6512efcd05a5c24cfded6178cb9f97d091480077aff43ca17958bdcd8b65"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2020-01-29-trivagomagazinesjourneytoserversiderende/"
published_at: "2020-01-29T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:05:25.747778+00:00"
content_hash: "sha256:9ffee24729655fb8b42e7c0fea802c6152a2d9f83e4b975b3c24bfb769a3cbcc"
---

# trivago Magazine's Journey to Server-Side Rendering

In the beginning of 2019, the[trivago Magazine](https://magazine.trivago.com/) team decided to switch over from a JavaScript Single Page Application to Server-Side Rendering. This article describes the why, the how, and further challenges of this journey.


## Single Page Applications and their Issues


The trivago Magazine started as a simple WordPress (WP) blog about interesting hotels and travel destinations. We used a customized WP theme that provided proper user navigation as well as SEO and further functionality. Even though WP offers a content management system that is set up easily and can be used by most editors comfortably, it also causes a lot of issues such as performance problems, scaling issues, tough maintainability, and many more.


In 2018, the development team changed over from WP themes to a trivago Magazine provided as a JavaScript Single Page Application (SPA) based on a WP REST API in order to isolate the frontend application from the WP backend. With this step, we gained a lot of design- and implementation sovereignty. The developer team was very happy about that step and with the chosen JavaScript framework,[Vue.js](https://vuejs.org/) . We were able to implement a lot of new functionality in a much shorter time without having (backend) performance and scaling issues.


As SEO traffic is very important for the trivago Magazine, we produced HTML landing pages containing SEO tags and further important HTML tags and stored these HTML pages as entry points for the SPA. However, the situation with the SPA and the custom HTML entry points was still very unsatisfactory.


Due to the SPA and all the JavaScript, search engines such as Google, Bing, and DuckDuckGo as well as social media sites did not always properly index our pages. To be indexed properly by Google and the other search engines, the SPA has to be rendered somehow. However, not all search engines are able to fully render an SPA. And even for Google, which is one of the few search engines being able to render JavaScript websites, the more it has to render the higher the probability that some pages are rendered later and, therefore, do not show content of pages instantly. This can have a negative impact on the ranking (see also[this blog post](https://www.onely.com/blog/ultimate-guide-javascript-seo/) to get an overview of the impact of JavaScript on SEO). This situation was even worse when our trivago Magazine content managers were performing many changes to our articles and, thus, a lot of pages had to be re-rendered. As a result, our SEO traffic broke down.


In addition to the SEO issues, the high amount of JavaScript lead our[PageSpeed Insights performance score](https://developers.google.com/speed/docs/insights/v5/about) based on the[Lighthouse tool](https://developers.google.com/web/tools/lighthouse) to drift massively to the red zone. Our Time-To-Interactive score had decreased massively. Especially, mobile clients suffered from the high amount of JavaScript, because they often do not have the computing power that desktop clients have - and the PageSpeed Insights performance score is specifically based on a mobile-first approach.


## Server-Side Rendering - Nuxt.js to our Rescue


Having these issues in mind, we started to come up with new ideas on how to improve our situation. Besides several small improvements, our main idea was to switch over from an SPA - and, thus, Client-side Rendering (CSR) - to Server-Side Rendering (SSR) keeping our freedom gained by separating the frontend application from WP using Vue.js.


In general, SSR can improve SEO traffic and performance, as all the content is already rendered as HTML source code and, thus, can be rendered faster by the customer’s browser (see also[this page for an overview of Client-Side vs. Server-Side Rendering](https://ssr.vuejs.org/) ). For implementing SSR, there are different approaches. One solution is to pre-render everything with a custom solution or with[Puppeteer](https://developers.google.com/web/tools/puppeteer) ,[Rendertron](https://github.com/GoogleChrome/rendertron) , or[prerender.io](https://prerender.io/) . The Vue.js website suggests using either[vue-server-renderer and create an SSR configuration on your own](https://ssr.vuejs.org/) or using specific frameworks such as[Nuxt.js](https://nuxtjs.org/) .


Nuxt.js is a higher-level framework on top of Vue.js providing an excellent development experience for writing “universal Vue.js” applications. Thus, Nuxt.js allows to run in[different modes](https://nuxtjs.org/guide/) : Beside a typical SPA mode for your application, you can decide between universal SSR and pre-rendering mode for your application. In universal SSR mode, your content is rendered as HTML on-the-fly based on a Node.js HTTP server. In pre-rendering mode, your content is compiled into static HTML files containing the necessary JavaScript which you can then store on a classical HTTP server.


For the trivago Magazine, we decided to go for Nuxt.js. We simply wanted to focus on application development instead of doing errorprone SSR configurations based on a basic Vue.js or even changing to a completely different framework. For a summary of the benefits of using Nuxt.js, we refer to[this blog post](https://medium.com/vue-mastery/10-reasons-to-use-nuxt-js-for-your-next-web-application-522397c9366b) . As we were already using Vue.js for the trivago Magazine before, the migration from Vue.js to Nuxt.js was not a big deal from the technical perspective.


## Pre-Rendering - the Ultimate Solution for the trivago Magazine


When we started to work with Nuxt.js, we eventually decided to go for pre-rendering and not for the universal SSR mode for the trivago Magazine. Most content that is being created by our content managers in the trivago Magazine is static. Although, we also have some CSR content in the trivago Magazine, we usually only have to pre-render our content when we add, modify, or delete articles or when we add, change, or remove application features. Essentially, we have mostly static content that allows us to use pre-rendering provided by Nuxt.js.


Finally, we deploy the pre-rendered HTML and JavaScript of the trivago Magazine as static HTML and JavaScript files on[AWS Simple Storage Service (S3)](https://aws.amazon.com/s3/) and attached[AWS CloudFront](https://aws.amazon.com/cloudfront/) - a Content Delivery Network (CDN) provided by AWS - to the AWS S3 buckets to host the trivago Magazine (see, e.g[this guide](https://nuxtjs.org/faq/deployment-aws-s3-cloudfront/) for more information about hosting a statically generated Nuxt.js site using AWS S3 and CloudFront). AWS CloudFront provides HTTPS to our customers and routes the customer traffic properly to AWS S3 for the HTML and JavaScript files or to our trivago Magazine API for Nuxt.js CSR components.


For pre-rendering the Nuxt.js SSR components of the trivago Magazine, we use[Jenkins](https://jenkins.io/) pipelines which run the pre-rendering in custom[Docker](https://www.docker.com/) containers. When pre-rendering our content, we access our trivago Magazine API to retrieve the content that is pre-rendered as static HTML and JavaScript which is then copied to AWS S3. We configured specific triggers to pre-render the trivago Magazine content when there are changes. The overall architecture and the steps in our pre-rendering approach is depicted in the figure below.


Overall, our pre-rendering approach powered by Nuxt.js and hosting the trivago Magazine via AWS S3 and AWS CloudFront provides us a highly cost-efficient and highly available architecture as well as a fast website delivery. The costs for storing and serving HTML and JavaScript via AWS S3 and AWS CloudFront is nearly negligible. The downside, however, is that we do not have an own configurable web server. The HTTP-server capabilities of AWS S3 are quite limited. But this is outweighed by far by the ease of the overall architecture.


## Conclusion


In the trivago Magazine, we use Nuxt.js to build our SSR and CSR components, to deploy pre-rendered HTML and JavaScript files to AWS S3 and AWS CloudFront. In doing so, we can solve our SEO and JavaScript performance issues. At the same time, we also get a very cost-efficient and highly available architecture. In the figure below, the development of Render-Start, Time-To-First-Byte, and Availability of the Magazine over time is depicted. You can see a drop in Render-Start when we deployed the SSR version of the Magazine in May.


Although, you can also do SSR or pre-rendering with other frameworks and tools or with an own configuration based on plain Vue.js, we decided to go for Nuxt.js, because it allows you to concentrate on implementing your business logic rather than building complex and, maybe, error-prone configurations. Furthermore, Nuxt.js provides you the freedom to choose between CSR and SSR per JavaScript component.


So, if you want to benefit from CSR and SSR on a per-component basis and want to have a comfortable framework for this based on Vue.js, the trivago Magazine developer team highly recommends Nuxt.js for your application.
