---
schema_version: "1.0.0"
document_id: "f74f013ce7bf3984a97e11e8b90458cf342af70bd6a1b97fd782b1e0a5ee80ea"
company_key: "doximity-inc-class-a-common-stock"
company: "Doximity Inc."
source_id: "doximity-inc-class-a-common-stock-rss-4199c1c9997e"
canonical_url: "https://technology.doximity.com/articles/the-cyclic-nature-of-in-house-tooling"
published_at: "2022-09-28T17:00:00+00:00"
first_seen_at: "2026-07-25T01:15:21.586776+00:00"
fetched_at: "2026-07-28T21:03:09.867162+00:00"
content_hash: "sha256:b9102834dfc6e1c868c1b8d4c100928409573a330dc2a6e2a3cfcf5a402a27f8"
---

# The Cyclic Nature Of In-House Tooling

Joseph M. Juran popularized the concept of considering members of an organization who are stakeholders concerning another individual or group’s products or services to be *internal customers* . In contrast, and as suggested by the name, external customers are those whose relationship to the organization is not through employment but by purchasing the organization's products or services. While a company’s focus primarily lies on the external, teams *within* the organization must often focus on both.


In this article, we’ll discuss the marketing initiatives surrounding the release of an internal product. At Doximity, we have various developers focused on many products and services. On the Infrastructure Automation team, we aim to design tooling and workflows that directly assist our internal customers, with the downstream effect benefiting those who are external. Our team's core mission is to build, maintain, and support test tools and frameworks, CI infrastructure and pipelines, and all test automation processes. Our overall goal is to make developers more productive while simplifying the process of deploying to production. While the utilities and services managed by our team are not directly customer-facing, their impact helps reduce friction in the creation of products that are.


Being an infrastructure team spanning support across R&D, we’re divided between our web, mobile, and data business units, myself aligned with the latter. Therefore, data engineers and scientists are my primary internal customers, with CI/CD users as a whole being my secondary. How do I determine where my developer time is best spent to support them? How can I generate new ideas that benefit my customers? How should I prioritize the myriad of requests sent my way?


Suppose I do consider the employees I support to be internal customers. In that case, I should also treat the utilities and services I create and manage as products that adhere to a product life cycle of sorts. This cycle emulates procedures which occur for products reaching a consumer market, and with the proper processes in place, can be used in a recursive manner throughout the entire journey.


Stable Diffusion Prompt: Online meeting in a grid, the face in the middle with a thought bubble cloud containing a light bulb signifying an idea above their head, corporate cartoon-style art


## Market Discovery: Finding The Need


As a whole, Doximity has adopted various processes to foster communication across our development teams, which is fundamental in generating new ideas. Many avenues can be used to assist in identifying areas within the organization that have room for improvement.


#### Cross-team Syncs


Groups with common goals tend to implement *cross-team syncs* , where we discuss new initiatives or changes to existing items which may impact shared internal or external customers. For instance, in situations where data engineers might be experiencing a point of friction that has not yet been raised to my attention, these sync sessions provide an opportunity for these pain points to be aired. Additionally, given that we tend to map our projects to a quarterly timetable, these meetings are extremely valuable as they allow us to coordinate joint implementation details before the start or release of a project. Furthermore, they enable us to keep a finger on the pulse of the latest external developments relevant to our team.


#### Working Groups


In a similar vein, Doximity makes use of *working groups* . Inspired by the Spotify "tribe" model, working groups consist of members from various teams with a shared interest in specific tools or products. These groups are responsible for cross-team collaboration and coordination of the subject they manage, and directly own the tool's or product’s success.


Joining a working group does not necessarily mean direct involvement in the development of the subject. While these groups discuss concerns, issues, and solutions to open problems, their overall purpose might be to simply determine guidance for the team which owns the subject matter. Regardless, these periodic meetings allow for valuable insight into the usage and needs of a tool or product from a diverse perspective and can help generate improvements and new utilities that otherwise may not have been considered.


#### Offsites


Finally, *offsites* tend to provide a more organic form of product discovery. Given that Doximity has historically been (and continues to be) a distributed company, offsites are an opportunity to meet outside of a typical workspace in a relaxed environment. Once per quarter, Doximity employees travel to a shared destination to socialize in person, set goals, and have retrospectives on what is working well or needs improvement. These discussions span multiple days and provide an invaluable opportunity for conversations that wouldn't take place over Slack, Email, Zoom, or other tools.


#### General Communication


There are of course more apparent methods of communication, such as public Slack channels, or the distribution of surveys and spreadsheets that allow our internal customers to log areas of our domain they find are slowing them down. There are many more opportunities to share ideas and discover improvements, but the bottom line is that providing open, monitored, and easily accessible channels of communication can help kickstart the process of identifying optimizations.


## Product Discovery: Defining The Solution


If market discovery resulted in a “ *what if?* ”, it’s through vetting the need that we determine “ *how to?* ”. This process typically begins with stakeholder feedback to determine if your proposed product will fulfill their needs. Should feedback be positive or not needed for this situation (such as refactoring an internal product to reduce spending with no change in behavior), we categorize our ideas in a shared team workspace and discuss them during our weekly team tag-up meeting.


Should team consensus signify the idea has merit, the next step is to formally discuss the need. This is done through a *Needs Assessment* , a short document communicating why the idea should be implemented, potential solutions, and beneficiaries. When complete, this document is shared within the company to ensure stakeholder validation and that all considerations have been addressed. If the sum of stakeholder reviews indicates approval, a *Technical Proposal* is often the next step for larger projects.


Technical Proposals define what needs to be done to achieve a successful release. These documents contain the architectural and design decisions planned for development and allow fellow engineers to provide input and suggestions regarding implementation details. Considering that they include much more in-depth technical information than the needs assessment, the planned approach is laid out clearly and serves as a developmental roadmap should another engineer adopt the project.


At this point, our product has been fully conceptualized and internal “ *market research* ” has indicated a gap in the market. The needs assessment (serving as a focus group) confirms consumer demand, and the technical proposal has paved the foundation via an approved and sound plan for creation.


Stable Diffusion Prompt: Focused on a 1920s newsie holding a laptop on a crowded street corner, yelling out to a crowd of surrounding pedestrians who are eagerly listening, corporate cartoon-style art


## Marketing Methodology & Feedback Utilization


While the next step in the product life cycle is product development, I’m going to skip that topic here and assume that readers who have reached this point have a firm understanding of what it takes to prioritize and implement concurrent projects. Instead, I’ll discuss marketing, a process that starts in parallel with development and continues throughout the product’s shelf life.


First and foremost, easily accessible and definitive documentation is critical. Consolidating information pertinent to what the product is, why it benefits stakeholders, and how it should be utilized helps reduce any possible hesitancy internal customers may have. Much of this information can be pulled from the initial assessment and proposal, and additional clarification can be appended throughout the marketing process as internal customers raise concerns or have questions regarding specific details. Fragmenting information into digestible portions will be beneficial down the road, as relevant reference material can be provided when needed.


Given that this living documentation exists throughout the product life cycle, I often find that organizing it as a guide provides the most flexibility. Partitioning configuration details, general usage information, and internal processes related to contributions and product extension improves accessibility regardless of purview. Prior to and following launch, access to this documentation helps set expectations with developers on the product’s capabilities and provides them time to prepare for any changes they may need to implement into their own projects. In cases where this new product introduces breaking changes, it’s crucial to relay this information to internal customers, even if migrations will occur without their involvement.


We have many approaches to internal marketing at Doximity, but some of the most useful include:


- Announcements made via Slack or email before and after launch provide a simple heads up that something new is heading downstream.
- *Lunch and Learns* are presentations with optional attendance that we frequently make use of. These sessions are recorded for those who could not attend and provide an opportunity for a quick deep dive into the product at hand, as well as an opportunity to field questions.
- *Show and Tells* are quarterly domain-focused collaborative presentations in which teams highlight new products and services released in the previous quarter.
- Sync meetings discussed earlier can provide simple reminders of new or soon-to-be-released products, and meeting with team leads ensures we can disseminate information to individual team members.


Using these avenues, we can broadcast our intentions to a wider audience. The feedback we receive may allow us an opportunity to pivot development, modify plans for release, or further clarify documentation after contemplating perspectives that we may not have considered so far.


## Product Release


Development has finished, and integration testing confirms that product functionality should fulfill the needs of internal customers. The rollout of this new product or service will not interfere with external team initiatives nor impact their ability to achieve goals. How do we ensure adoption is truly happening? How can we monitor success?


My infra-automation counterpart for data, Sam Hart, wrote a fantastic[article](https://technology.doximity.com/articles/building-a-test-automation-pipeline-for-data-science) covering many of the features we’ve implemented within our pytest-based internal test framework, which is currently used across nearly all data projects. While not discussed within his article, one plugin we frequently utilize is our analytics collector.


Upon completion of a test session, the analytics plugin captures information regarding the state of the project and uploads it to Segment. Through Looker dashboards, we can visualize and interpret this data at a glance in various ways. Examples include:


- A better understanding of testing growth within Doximity during specific periods by filtering total tests over time.
- Ranking project staleness by summing the number of test runs conducted in CI within their default branch.
- Determining which projects could make use of parallelism to reduce testing runtime.


We also collect project configuration and feature usage details. In cases where we deprecate features within our test framework, we can instantly be made aware of which teams will be impacted by these changes. In contrast, for cases where we add features, we can track the adoption rate and usage of whatever we may be implementing. Should the new feature stop working as intended, we can immediately notice a variation in our charts and begin an investigation into the projects which encountered this error. This is all to say that, if applicable, internal analytics might provide valuable insight into the usage of your product over time.


A more straightforward method of tracking adoption is manually verifying success within the projects that may utilize it. If rollout is planned via spreadsheet, additional columns to indicate successful results (links to pull requests, artifacts your product may generate, etc.) will simplify calculating rollout completion.


Regardless of the methodology used, ensuring your product has usage is a fundamental step. The effort and money poured into your development time should always have a productive outcome - otherwise, what was the purpose?


With the product now in the hands of internal customers, feedback regarding improvements or new functionality may be requested over time. While the customer may always be right in matters of taste, the needs of the greater stakeholder community must always be considered.


The solution to this is where the recursive nature of the product life cycle becomes apparent. For projects on a larger scale, heavy adoption might eventually lead to the creation of a specific working group to guide the path for further development. Just as we once used needs assessments for conceptualizing the product, your users may now use them to validate their requests. Alternatively, the product’s documentation, should it contain contribution guidelines, may be the subtle push needed to provide the user the confidence to implement the idea on their own. Requests aside, bug fixes, refactoring, and general improvements will always be required in the ever-changing world of technology. As the product grows, these situations may require a repeat of the stages discussed previously.


Eventually, there will come the point where this internal product no longer serves a purpose. For example, an underlying service may no longer be used within the organization, or a new product may be a more suitable replacement. Regardless, the product will eventually be considered outdated and to have reached the end of its life. However, the deprecation, migration, or investigation into an alternative solution begins this process anew. Much like a phoenix rising from the ashes, the cycle will repeat until the needs it once satisfied are no longer a concern.


## Conclusion


Every organization defines its development process, but key steps within this cycle enable us to create superior tools and services while developing more efficiently and effectively. Documentation and open communication are essential for every stage of this journey and help reduce the friction in releasing your project into the hands of the teams you support. Ensuring your fellow employees understand why this implementation is best increases trust in your vision and builds support for the initiative. Fortunately, Doximity offers a collaborative environment full of intelligent peers and a well-thought-out methodology to accomplish this in full.


---


Be sure to follow[@doximity_tech](https://twitter.com/doximity_tech) if you'd like to be notified about new blog posts.
