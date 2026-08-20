---
schema_version: "1.0.0"
document_id: "428ddab6f426467ebf5f50add9fdda957ff336a8e4f9392e748979170377a08e"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/expand-oss-sponsoring/"
published_at: "2018-07-17T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:5af142dcd8630810cf7f0d8f41d0b04e125c2d84b3e1c2c2b7abff3070ee62ed"
---

# In an Open Source World, Everyone Wins - Here's Why

Imagine a world without open source software. Pretty scary, isn’t it?


There would be no free operating systems that let you take full control of your computer.


There would be no collaboration of passionate software developers scattered across the planet, producing community-made, extensible tools like browsers and editors.


There would be no code sharing platforms to get feedback on one’s work and learn from others. Young programmers starting out would have a hard time learning the craft.


Without open source, we would be reinventing the wheel every time we start a new project, instead of focusing on iterative improvement and innovation.


## Continuing our support


At trivago we are committed to open source principles. Our own culture is built around the concept of ‘test and learn’, and our entire development process has been built around large-scale collaboration.


So we are proud to today announce a significant expansion of our sponsorships for vitally important open source projects. We have committed to[another year](https://medium.com/webpack/trivago-sponsors-webpack-for-second-year-bfe6ca2f0702) of[webpack sponsorship](https://tech.trivago.com/post/sponsoring-webpack) and, starting from July, we are now also[sponsoring the Babel community](https://babeljs.io/blog/2018/07/16/announcing-babels-new-partnership-with-trivago) through[Open Collective](https://opencollective.com/trivago) .


With this new exciting partnership, we hope to help Babel get one step closer to its goal of having two full time maintainers for the project. We also hope to inspire other companies to step up and to follow our lead by investing in the Open Source communities they rely on.


## Why Babel?


We started using Babel early in 2015 and have benefitted from the increase in productivity ever since. The biggest win here is Babel’s ability to compile ES6+ back to JavaScript that even Internet Explorer understands. It is simply great to use the latest version of the JavaScript language without having to worry about browser compatibility.


However, there are other benefits, with some of our custom webpack plugins using Babel under the hood to perform their work.


For example, we currently pre-build most of our packages with[Rollup](https://rollupjs.org/) before combining everything through webpack. Here, Babel is a key tool allowing us to support the` import()` function within our code. We use it to first compile` import()` to a placeholder when running Rollup, then we compile it back to` import()` when running webpack.


Our translation tool also uses Babel to identify the required translations for our application and replaces their usages with array indices for a reduced build size.


There are many more use cases where Babel helps us make our product better and increase our productivity, and we are sure that there are more to come.


## A big thank you to the community


We firmly believe that a sustainable Open Source ecosystem is good for everyone. It is good for the Babel and[webpack](https://webpack.js.org/) communities, because they can build the tools they are passionate about. It is good for us, because these tools make our developers happier and more productive. Finally, it’s good for the people using our products, because we, the developers, can focus on adding user value instead of having to take care of tooling infrastructure.


We could not run our tech teams, our stack, or our entire company without open source software. That’s why we want to contribute to a vibrant open source community, playing our own small part in ensuring it continues to thrive and produce more great products and projects.


Thank you, Babel community, for your drive to innovate and the amazing tool you’ve built!


Thank you, webpack community, for reliably cracking the tough nuts and for allowing us to increase our productivity.


Thank you, open source contributors everywhere, for your passion, dedication, and for doing the right thing.


You truly make a difference!
