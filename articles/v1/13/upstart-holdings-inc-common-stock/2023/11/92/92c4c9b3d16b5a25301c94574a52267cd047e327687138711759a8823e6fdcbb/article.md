---
schema_version: "1.0.0"
document_id: "92c4c9b3d16b5a25301c94574a52267cd047e327687138711759a8823e6fdcbb"
company_key: "upstart-holdings-inc-common-stock"
company: "Upstart Holdings Inc."
source_id: "upstart-holdings-inc-common-stock-rss-a598e0871937"
canonical_url: "https://medium.com/upstart-tech/building-a-mobile-app-zero-to-one-79b862f6d847"
published_at: "2023-11-16T16:03:13+00:00"
first_seen_at: "2026-07-20T23:17:16.152971+00:00"
fetched_at: "2026-08-20T00:26:23.139930+00:00"
content_hash: "sha256:95905155c78807ebbdb5f4c48f94c745efbfb46d193bc90694b9582e755374af"
---

# Building a Mobile App: Zero to One

Hi! In this blog post, I’ll be sharing my experience of working on[Upstart](https://www.upstart.com/) ’s borrower-facing mobile iOS app, now released and available in the[App Store](https://apps.apple.com/app/apple-store/id6450968733?pt=126471806&ct=TechBlogMobileZeroToOne&mt=8) !


Upstart has been primarily a monolithic web stack during our time as a company, with a recent increase in engineering effort to move to a microservice architecture. We have been focused on expanding product verticals and serving borrowers with a significant investment in responsive web. There had been growing interest internally and externally in offering a mobile app, so we put a small team together to build a proof of concept that could evolve into a MVP (minimum viable product). I joined the mobile team to begin working on that proof of concept, with additional goals of shaping early technical direction and quickly adding a second mobile engineer. Here are some highlights from this journey!


Learn more about what Upstart is and how it uses AI[here](https://www.upstart.com/our-story) **.**


### Determining the Architecture


Early on, we were tasked with determining the tech stack of choice for our mobile app. We expected we would remain a small team, therefore building out two separate teams (iOS and Android) was going to be untenable for this launch. We also believed at the time that the MVP wasn’t going to be extremely technically complicated; most likely it would target functional parity with our web experience. In our initial launch, we’d have no need for the power and capabilities of a native implementation.


The general mantra in those early months was “walk before we run,” i.e., get an app out the door, provide value to our users, iterate over time to expand on functionality, and then leverage mobile features such as notifications or new experiences that could drive engagement. We believed at that time the most complicated element would be integration with an existing, constantly evolving monolith with no mobile infrastructure and many years of tech debt.


We felt we had a couple of clear cross-platform options (React Native, Flutter, MAUI, Cordova) to get the most value given our small team. The most viable option that jumped out was React Native with TypeScript, for the following reasons:


- **Upstart Investment in React and TypeScript:** Upstart has invested a large amount of frontend development in React and TypeScript, and its use across Upstart’s ecosystem is mature. New frontend projects follow the adoption of React, TypeScript, and our internal component library (for web).
- **Expert Engineers:** Because of this investment, we have an abundance of React and TypeScript engineers who could contribute in meaningful ways during this proof of concept development and beyond. The interoperability between React and React Native is high.
- **Community support:** Generally speaking, the React Native community and ecosystem are quite active. Other cross-platform communities have a similar ecosystem, but they didn’t intersect with Upstart’s experience and ecosystem.


Because one of the goals would be to propel the proof of concept into a MVP app, we felt that React Native would require the least ramping up and potentially the easiest throwaway work if down the line we wanted to abandon it due to any technical limitations. We never want to throw away work, but it made sense to ramp up and demonstrate potential quickly with a framework that a small team could execute on.


A number of people weighed in on this early tech stack decision and the team moved forward on the proof-of-concept app in those early months.


### Webviews vs. Native vs. Hybrid


Much of our experimental time was spent evaluating between approaches and patterns in webviews, native, and hybrid. For context, a “webview” is a full browser embedded in a mobile app (HTML, JS, CSS, without all the browser packaging), similar to an iFrame in the web world. A native implementation means that content and all interactions live in native code. “Native” here means React Native, which under the hood translates to native app code for Android and iOS. A hybrid approach blends webviews and native as seamlessly as possible — ideally an end user has no idea what is native and what is a webview, while the engineers benefit from the control of deciding which content-serving mechanism is best.


For a tenured mobile engineer at a mobile-first company or even a company that was built with a strong external API architecture, the technical decision to go with webviews versus native might not be interesting. But for a zero-to-one app where there is no existing webview or API infrastructure in a complex, mostly monolithic architecture, the decisioning around where to build native and webviews is perhaps more interesting and challenging than the implementation itself. Framed another way: It is challenging to take a high-velocity monolithic architecture that is already serving customers and build a new client consumer requiring a service-oriented architecture without migrating the entire monolith to the ideal architecture.


In our early experiments, a hybrid approach came out as the winning choice. We learned a number of important lessons in supporting that approach:


**UX Interactivity**


Having a significant amount of UX interactivity (dynamic height, modals, animations) lends itself to native implementation. Generally speaking, the native animation will be snappier and smoother than those inside a webview, i.e., native UX typically “feels” better than webview interactivity on a mobile device.


**API Support**


If there are no APIs to provide backend data, they need to be built. In a post-MVP world, backend APIs should support versioning and multiple consumers like web and mobile. In a monolith web world, API versioning may never have been a consideration because everything ships together!


**Content Complexity**


The complexity of the content may lend itself to webviews if an organization can’t support maintaining logic in two places. For example, a team or organization may not be structured to best support two different codebases if the complexity is high. If shared logic** doesn’t work, webviews are appealing.


**Examples of shared logic:


- Backend / service only, which returns data structures that the different consumers can maneuver into the right form factor at the presentation layer.
- Presentation layer, which returns fully formed API JSON data consumed by the web and mobile app.
- Frontend component based, which is shared in both frontend web and frontend mobile components.


Examples of shared logic options among web and mobile.


**High-Velocity Changes**


A culture of updating content at a high frequency lends itself to webviews as well. Here at Upstart, we now strive for at least three daily releases of our monolith, and many of our microservices release more frequently. Meanwhile, the app release cycle is slower as it is dependent on App Store approval and user update rate. Any content that has a high churn of code can be served in a webview that lives in its own source code release cycle, decoupled from the app release cycle.


**Serving Partial Content**


If there is no ability to serve a section of a webpage for a webview, the code must be modified to support that. This can be done either via additive or subtractive methods, i.e., creating small HTML chunks that contain only the smallest pieces of content, or by rendering a larger piece of content and making only the relevant content visible via style rules. The former is optimal from a content performance perspective. Navigating legacy code in a performant and scalable way may require attention to detail to achieve this.


After a few months and a lot of experimentation in webview-landia, the proof of concept gained support. Our team saw alignment with the product and design teams on the scope of a MVP, shaped by a strong product vision combined with early technical experimentation. The engineering team also grew by one — bringing in a much appreciated tenured mobile engineer!


Our team felt confident about the scope of work except for a pretty big piece: A major part of the mobile app functionality used webviews from a user experience that hadn’t evolved in several years. There was an opportunity to improve the user experience for both web and mobile with the improvement of the webview building blocks. Additionally, the code driving that experience was behind the curve on Upstart’s latest frontend tech architecture. Any time spent updating or improving “legacy” code could become throwaway work if the code was to be migrated to the new frontend solution.


So, the mobile team committed to a large effort on the update of the web experience for users, which would be built in a way to support shared API-level integration and/or webview integration and offer parity in the mobile app. Another intentional decision was made at this time, as well, to align with the direction of our identity platform team, which was set to release and support a centralized identity service in early Q3.


### Execution & Delivery


By this point, we had a growing team and a generous number of contributors from all parts of Upstart (legal, compliance, growth, quality, operational support, and more!) with a few major goals:


- Finish the implementation in the app for those parts that would be native.
- Contribute as major stakeholders in the migration and update of the web experience that would be a large part of the mobile app, shaping the technical direction to allow consumption by web and mobile. Update the mobile app to consume these pieces as they were delivered incrementally.
- Update our mobile authorization to adopt the centralized auth service built by our identity team, working closely with them on specific mobile needs.


We overcame a few bumps and were able to execute on all of these deliverables within a few months. While there were early proof-of-concept decisions made around native vs. webview, a handful of those decisions evolved as the shared UX for web and mobile was finalized.


This project phase also saw the completion of a handful of quality and maintenance concerns:


- We put significant effort into optimizing the user experience in-app by preloading data in the background.
- With the help of a quality engineer, we supplemented unit tests by adding end-to-end tests that focused on integration signals like login and webview form submission processes.
- With support from our React engineers, we implemented stricter linting rules and cleaned up technical debt (yes, we still created tech debt in a POC!).


Earlier last month, we submitted our app to the App Store, and it was approved! Find it[here](https://apps.apple.com/app/apple-store/id6450968733?pt=126471806&ct=TechBlogMobileZeroToOne&mt=8) .


For the remainder of this quarter, we plan on ramping up and monitoring our app through observability and metrics.


Find our app[here](https://apps.apple.com/app/apple-store/id6450968733?pt=126471806&ct=TechBlogMobileZeroToOne&mt=8) .


### Lessons Learned and What’s Next?


While we learned a lot about working with a strong product vision to build a mobile app for zero-to-one, one important lesson is this: Technical decisions aren’t necessarily objective. The people and organizational structure of the team will influence the technical direction of a project. No doubt the journey from idea to launch for this app could have looked different if approached by a different team or organization. But the end result, if done right, should be the same: a strong, ready-to-launch product.


What’s next for us? First and foremost, we want to continue to have stable releases and analytics that help us shape our direction (walk before we run, with data!).


After that, we’re considering a few possible next steps: capitalizing on the cross-platform tech decision of React Native to offer the app in Android, building out notifications, bringing more web functionality over to mobile, or offering new mobile-featured functionality to engage both new Upstart users and existing users. We haven’t decided on the exact direction yet, but exciting opportunities await in our[now-released mobile app](https://apps.apple.com/app/apple-store/id6450968733?pt=126471806&ct=TechBlogMobileZeroToOne&mt=8) !


---


[Building a Mobile App: Zero to One](https://medium.com/upstart-tech/building-a-mobile-app-zero-to-one-79b862f6d847) was originally published in[Upstart Tech](https://medium.com/upstart-tech) on Medium, where people are continuing the conversation by highlighting and responding to this story.
