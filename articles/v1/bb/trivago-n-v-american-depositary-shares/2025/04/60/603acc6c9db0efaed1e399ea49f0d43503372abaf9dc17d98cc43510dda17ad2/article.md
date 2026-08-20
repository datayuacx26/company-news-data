---
schema_version: "1.0.0"
document_id: "603acc6c9db0efaed1e399ea49f0d43503372abaf9dc17d98cc43510dda17ad2"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2025-04-23-tailor-made-browser-extensions-for-increased-testing-efficiency/"
published_at: "2025-04-23T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T20:57:45.279486+00:00"
content_hash: "sha256:059ffbce2756034a590da7f54d30dae4164a6aafd42bec01b9349965fa26e399"
---

# Tailor-made browser extensions for increased testing efficiency

## Our first browser extensions


When doing exploratory testing on a web application, the nature of activities involves discovering things and not repeating the same patterns. However, there might be aspects that could be improved by specific tools without trespassing into the realm of test automation. Can coding skills help testing activities, beyond the somewhat obvious case of automated tests? In the trivago QA team we asked ourselves this question, trying to find solutions to make some testing processes less cumbersome and save time while maximizing focus.


Almost 10 years ago, we started exploring the idea of using browser extensions and started documenting the most useful ones in our Confluence pages to serve as a valuable resource for newcomers to the team. For a glimpse of some of these extensions, feel free to check out the[Chrome web store](https://chromewebstore.google.com/search/testing) .


But we wanted something more bespoke. Interestingly, the very first inspiration for a browser extension we developed in-house was actually a reverse engineering process. The need to efficiently test the Google Tag Manager (GTM) data layer values across various pages of our web application, as well as in diverse scenarios such as redirects and other complex situations, prompted this exploration. It was just a simple confirmation of what we could do, but it sparked interest in a topic that hadn’t been explored before.


The first custom Chrome browser extension we built helped identify the version of the trivago website being displayed. Although small, it was widely adopted by the Quality Assurance team as well as many other employees across the company. It was developed during an extensive rewriting of our web-app. As the goal of the rewriting project was having the same UI, but with better performance, there was no instant way to distinguish the old platform from the new. A tool to instantly understand what version we were looking at was a quite helpful addition and at the same time triggered a deeper involvement into the technical aspects of developing a browser extension. It involved studying the best approach for the requirements, for example dynamically updating the extension icon was one of the best features for such a use case, while a click would show a popup and provide more detailed information about the version of the website that the user was visiting.


Following the development of the first extension, we experienced a rapid influx of ideas and feature requests and in a short time created extensions tailored to the diverse needs of the QA team.


## More requirements, more extensions


It started with a simple need: our QA team often had to report incorrect data or unexpected behaviour related to specific accommodations on our website. After searching for a hotel, they needed a way to quickly identify and share the internal ID of that accommodation when filing a report. To streamline this, we built a small Chrome extension that displayed the accommodation ID directly in the UI.


This initial tool quickly proved useful not just for QA, but for other teams as well. As more internal use cases emerged, we started building additional extensions:


- **QA-Buddy** for quickly accessing error logs, QA tracking tools, and setting debug cookies
- **TLD-Switcher** for switching between different country and language versions of our site


These tools helped automate repetitive tasks, reduce manual errors, and improve team efficiency. But managing them as separate extensions became cumbersome. Each required its own updates, permissions, and maintenance.


As usage grew, it became clear that we needed a more streamlined approach. We decided to merge everything into a single, universal extension. This reduced overhead and made it easier for teams to access the tools they needed all in one place.


## A Universal Extension


Instead of managing multiple standalone extensions, we saw an opportunity to simplify and improve our workflow by merging them into one. A single, unified extension would reduce complexity, improve performance, and make it easier for our team to access all necessary tools in one place. Here’s why this approach made sense for us:


**1- Seamless User Experience**


Having separate Chrome extensions meant our team had to switch between multiple tools to complete simple tasks. This disrupted workflows added unnecessary friction. A single, well-designed extension brought everything under one roof, making it easier to access and use all essential functions without switching back and forth.


**2 - Easy To Scale**


As our internal needs grew, so did the number of Chrome extensions we relied on. Instead of continuously adding new ones, we took a step back and designed a modular, scalable solution. Our universal extension allows us to add new features as needed without cluttering the browser with separate tools.


**3 - Easy To Maintain**


Every extension had its own codebase, making updates and bug fixes time-consuming. Unifying them into a single extension simplified maintenance, deployment, update process, and created a single destination for all future improvements.


## Manifest v3


As we worked on combining our internal tools into one extension, Google rolled out the requirement to migrate to[Manifest V3](https://developer.chrome.com/docs/extensions/develop/migrate/what-is-mv3) . The timing aligned with our application rewrite, so we used the opportunity to modernise the extension as well.


The Chrome Extension Manifest V3 is a mandatory technical specification that defines how Chrome extensions must be built. It’s essentially a JSON configuration file that declares your extension’s capabilities, permissions, and structure.


By June 2024, all extensions must use V3 to continue working in Chrome, as Google is phasing out the older V2 version. Any extensions not updated will stop functioning after this deadline.


Manifest V3 introduced several changes most notably the move from background pages to service workers, a more restrictive permissions model, and improved performance and security standards. These changes required a full refactor of the extension’s architecture.


We took this chance to rebuild the extension using TypeScript and React, moving away from vanilla JavaScript. This brought multiple benefits:


- Type safety reduced runtime bugs and made the codebase easier to refactor
- Better tooling and editor support sped up development
- React allowed us to structure the UI more cleanly and build more interactive components
- Overall, it aligned the extension with how we build and maintain the rest of our frontend code


This tech stack upgrade, combined with the shift to Manifest V3, helped us create a more stable, maintainable, and future-proof extension.


## Improving distribution


Just like the extensions themselves evolved with time, also the release process of the extensions went through different stages, with constant improvements. The first rudimental QA-Buddy was stored in a GitHub repository and could just be pulled to the same folder from where it was loaded, then mounted from the “load unpacked” developer mode of Chrome. That was an initial simple and straightforward solution, but the Universal extension required a few more steps after the rewrite for Manifest v3 and due to its build process. Pulling the repo and then building the extension, then loading in Chrome the content of the /dist folder was the initial solution. We then managed to simplify that too with Github Actions. Upon every merge commit to main, the workflow builds, zips and pushes the zip file to a GCS bucket that thanks to a proxy serves it as a regular web URL. Next to it, we also publish the changelog as a HTML page, allowing anyone in the company, even without GitHub access, to read the changelog and just click “Download” on that same page to get the zip file with the latest version. Each update also triggers a Slack message to a dedicated channel, providing an effortless process of deployment and notification.


An additional aim that we had was also to set up auto-updates of the extension, i.e. let the users install it one time and then ensure that it automatically updates based on the version in the manifest.json. We couldn’t achieve that though, due to the strict policies of Chrome that doesn’t allow updates from a URL different than the webstore. As we can’t publish it on the Chrome webstore either, due to a couple of sensitive features, we are sticking to our deployment process and the “developer mode” installation for the time being.


## Conclusion


The original motivation to develop browser extensions for testing purposes was simply to solve problems that individual team members faced. The benefits that we discovered along the way were more than that though, as for example:


- Browser extensions that enrich information on a web page with additional data on a web page can help colleagues from other teams with less technical knowledge
- Such solutions can help speed up tedious parts of testing and debugging processes
- The development of extensions can be a concretely useful, but low stake, coding learning opportunity for QA engineers


Also, eventually reaching the state of having a modular design in the architecture of our Universal extension made it easier to add new features and achieve better results with a more accessible development experience, as testified by the increase of contributors to the project.


What we figured out over time is that a creative approach to enhancing testing activities can be very powerful and that as QA engineers, we should not feel restricted by the existing set of tools, but instead explore every new opportunity. This is even more relevant now that AI-powered assistants allow to quickly transform ideas into code and working tools.
