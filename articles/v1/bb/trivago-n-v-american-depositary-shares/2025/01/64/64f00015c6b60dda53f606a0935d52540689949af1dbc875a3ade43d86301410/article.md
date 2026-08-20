---
schema_version: "1.0.0"
document_id: "64f00015c6b60dda53f606a0935d52540689949af1dbc875a3ade43d86301410"
company_key: "trivago-n-v-american-depositary-shares"
company: "trivago N.V."
source_id: "trivago-n-v-american-depositary-shares-rss-0be0766927d8"
canonical_url: "https://tech.trivago.com/post/2025-01-27-frontend-at-trivago-behind-the-scenes/"
published_at: "2025-01-27T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:44.896445+00:00"
fetched_at: "2026-07-28T20:58:25.712310+00:00"
content_hash: "sha256:04f6843a756bee3ed2be2acd7329a501e9717aae6b9fb346110952c3863ede7a"
---

# How We Build: Behind the Frontend of trivago’s Website

Modern frontend engineering is a collaborative effort of many people with different talents and expertise. Over the years, a team working together can continuously refine their ways of working, so that the different contributions fit together seamlessly and each step in the production process is in sync with the next one. Read about our team structure, our A/B test setup, and about what makes our QA team so special.


At trivago, our frontend team is a vibrant group of about 80 people working together in squads led by product managers. With close collaboration among product managers, designers, engineers, and QA specialists, the team is dedicated to delivering a seamless user experience. In this article, Frontend Engineering Lead Tom Bartel shares more about the team structure and testing culture.


## Can you describe the structure of the frontend team at trivago?


There are roughly 80 people who drive the development of the frontend applications at trivago. By this we mean the trivago web app accessed through[trivago.com](https://www.trivago.com/) , the Android app, and the iOS app. A product manager (PM) is responsible for a certain aspect of the applications, such as the search, the display of results, the landing experience, etc. They oversee their area across all three frontend applications. They develop a vision of where they want to take the product, stay in touch with user research, and create a roadmap for how to get to that vision. A PM is supported by one or more product designers, a tech team, and a quality assurance engineer.


The product designers define what the user interfaces should look like across different apps and devices. Always in close collaboration with the PM, they often draw multiple versions of a new feature, which might then be tested in parallel, so that we can choose the one that works best.


The tech team consists of a tech lead and several frontend engineers. They implement in code what the PM and designer have envisioned, making sure the code base stays clean and high-performing over time.


Finally, the QA engineers compare readily implemented features against the original description of the feature and inform the frontend engineers of any deviations. They know our applications inside-out, and make sure no errors or unexpected behaviour can sneak in. The PM, designer, engineers, and QA make up what we call a squad. There are currently six such squads, but this can vary.


The squads are supported by two cross-cutting functions; the Product Intelligence (PI) team and the User Experience (UX) research team. Product Intelligence analyses the many metrics our applications collect all the time and advise PMs on how to run tests more efficiently. UX research observes how our users actually use our applications. They conduct interviews with real people, asking them to perform certain tasks at trivago and bring their findings back to the product squads.


## How many tests are typically running at the same time, and what does this mean for the frontend team?


We typically have between 40 and 60 frontend tests running at any given time, at different levels of exposure. For example, we can expose a test to 50% of the traffic globally, or only to 20% of the traffic in Brazil and New Zealand. Some tests will only be active for a few hours to quickly test an assumption we have, whereas other tests might run for multiple weeks, with some tweaks in between to react to new learnings.


Having a large number of tests in the code base can be challenging, because every test comes with additional complexity that can make subsequent changes to the code harder and more risky. The element that is probably tested most frequently is the accommodation card in the search result list. It is not unheard of to have 10 or even 15 tests active there at the same time, which means the designers, PMs, and engineers in different teams have to align carefully in order not to break each other’s tests.


Having such an advanced testing infrastructure in place is a great advantage. It allows us to also quantify the value of purely technical tests like performance improvements, or improved rendering, or switching to a new image format, etc.


## How do we avoid things breaking on the frontend, and what role does the QA team play in this important process?


To ensure we provide our users a great experience with our products, our team carries out extensive testing on all new features that we send live each day, with a strong focus on both manual and automated testing. Our team also collaborates with other tech teams to ensure our applications and services remain stable and functional, using logging tools such as Elasticsearch, monitoring and alerting platforms like Grafana and in-house data analysis software to assist us with debugging and root-cause analysis.


## What technologies/tools are used in the QA team to ensure comprehensive test coverage?


In QA, we utilize automation technologies to ensure wide test coverage and to alleviate our QA Engineers from repetitive regression testing, allowing them to focus more time on testing the latest new features of our products. Our core automation suites are based on the Selenium, Appium & Karate frameworks to test our website, mobile apps, and backend services. They all consist of JAVA-based test scenarios. We also use our own in-house, open-source test reporting tool[Cluecumber](https://github.com/trivago/cluecumber-report-plugin) and have recently adopted Playwright for automated visual testing.


## Can you share one or two examples where A/B testing have brought us learnings and changes to our frontend?


One of our biggest changes in recent months was the “dateless” result list. When the user has not selected dates explicitly, we still list accommodations as before, but we do not show specific prices, and we do not offer to forward the user to booking sites. We only show, e.g., “prices from $79”, but require the user to enter specific arrival and departure dates before they can move on to a booking site.


A lot of iterating and testing was done on this topic to flesh out tons of little and not-so-little details: We had to find a good default sorting of the list, present the information in the most digestible way, find the best wording to communicate what the user is seeing, and finally guide the user towards the selection of dates. The dateless list also caused a lot of underlying technical changes, because searches had so far always been tied to specific dates. The end result is however worth it, because the user can now make a much more informed decision about the booking sites they visit.


Another interesting test was done on the homepage: In order to find out which homepage elements were most important to users, we randomised the order of the elements and measured users’ behaviour accordingly: Which sections did they interact with most? Did the additional information rather support them or distract them? The analysis of this test is still ongoing, but it has the potential of improving the usefulness of our home page.


## Any recently accomplished project in the frontend team and its impact on our business that you would like to highlight?


The trivago result list typically shows 35 accommodations with a lot of information. The markup code behind this list is complex, and means a lot of work for the browser. One of our engineers wanted to try out an optimisation where we render only those accommodation elements that are actually visible - on mobile phones, that means only 2 to 3 items -, while rendering only simple placeholder elements for the off-screen ones. This test was extremely successful!


Users were able to browse through our result lists more smoothly, and our site became more responsive - especially on weaker mobile devices. The result was longer user sessions, more interaction, and eventually more revenue. The test clearly showed how important performance is for modern web applications.


## How do you foster a culture of continuous learning in trivago?


A lot of continuous learning happens in the day-to-day work. People discover a new way of doing something, or read an interesting article and share it in one of our internal communication channels. Team leads are constantly on the lookout for topics or work that should be shared with a wider audience, and encourage their engineers to present something in one of our engineering guild meetings.


An engineering guild typically meets every other week. This is where a lot of knowledge sharing happens. Presenters don’t have to invest a lot of time to create the perfect PowerPoint slide deck. Instead, it is common to just share your screen and show some code while explaining what you did.


Every engineer can get access to online learning resources like LinkedIn learning, Coursera, and Frontend Masters courses if they like. Additionally, we encourage engineers to visit roughly one conference a year to stay up to date with the latest trends and technologies within the industry.
