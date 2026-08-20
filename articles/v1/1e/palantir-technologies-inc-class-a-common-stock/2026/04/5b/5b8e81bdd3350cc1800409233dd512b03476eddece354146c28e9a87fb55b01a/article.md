---
schema_version: "1.0.0"
document_id: "5b8e81bdd3350cc1800409233dd512b03476eddece354146c28e9a87fb55b01a"
company_key: "palantir-technologies-inc-class-a-common-stock"
company: "Palantir Technologies Inc."
source_id: "palantir-technologies-inc-class-a-common-stock-rss-f87a89a6619a"
canonical_url: "https://blog.palantir.com/frontend-engineering-at-palantir-building-a-backend-less-cross-application-api-a40be7874ee5"
published_at: "2026-04-06T17:16:17+00:00"
first_seen_at: "2026-07-20T03:31:10.454667+00:00"
fetched_at: "2026-07-28T22:00:15.315546+00:00"
content_hash: "sha256:a0ad039c5a5a410438138c9c8e9bc7f52df262bb6120fbbb1f3d797a01baeab4"
---

# Frontend Engineering at Palantir: Building a Backend-less Cross-Application API

# Frontend Engineering at Palantir: **Building a Backend-less Cross-Application API**


[Palantir](https://palantir.medium.com/?source=post_page---byline--a40be7874ee5---------------------------------------)


7 min read


·


Apr 2, 2026


--


Press enter or click to view image in full size


[About this Series](https://blog.palantir.com/all?topic=palantir-frontend)
*Frontend engineering at Palantir goes far beyond building standard web apps. Our engineers design interfaces for mission-critical decision-making, build operational applications that translate insight to action, and create systems that handle massive datasets — thinking not just about what the user needs, but what they need when the network is unreliable, the stakes are high, and the margin for error is zero.*


*This series pulls back the curtain on what that work really looks like: the technical problems we solve, the impact we have, and the approaches we take. Whether you’re just curious or exploring opportunities to join us, these posts offer an authentic look at life on our Frontend teams. Find all Frontend Engineering blog posts*[here](https://blog.palantir.com/all?topic=palantir-frontend) *.*


*In this blog post, a frontend engineer based in CA shares an overview of several frameworks that Palantir apps use to communicate with each other in real-time. Stay tuned for Part 4.*


When I graduated college and joined Palantir as a frontend developer, I expected a familiar form of app-building work: design an interface, build it out, iterate with users. When I joined a team called “Application Frameworks,” the work turned out to be slightly different. Instead of developing an individual app, the team focuses on how different apps talk to each other — what my teammates describe as, “the backend of the frontend.” This brings into question whether we were a product team focused on user experience or an infrastructure team maintaining services across applications. The answer to this question required learning how Palantir apps are actually used in the field.


It’s true that for most app teams, UX challenges are limited to the scope of a single application and focused primarily on how its interface and information flow can be optimally designed. But, in practice, this understanding of UX is not a complete picture of how users interact with Palantir software. Many of our users’ most impactful workflows actually involve multiple applications working together simultaneously. My team’s job was building the frameworks that made that communication possible.


Two such applications frequently used together are Gaia and[Workshop](https://www.palantir.com/docs/foundry/workshop/overview) . Gaia is a tool that specializes in geospatial data, allowing users to visualize and interact with data on maps. Workshop is quite different — it’s a WYSIWYG editor for building specialized, interactive data interfaces. Workshop allows users to explore data, understand their decisions, and trigger actions with custom-built buttons.


These applications are useful on their own, but they become even more effective when paired together. Now, users can interact with map-based data in Gaia while simultaneously accessing tailored interfaces in Workshop built for their exact workflow. When used this way, although each app has its own UX, the true frontend experience that users care about is one that connects both interfaces seamlessly across both apps.


The following is a notional example of a user workflow that involves selecting a US state from Workshop (on the left) and exploring it inside of Gaia (on the right). Our users frequently work in side-by-side layouts that allow them to work with two apps simultaneously.


Press enter or click to view image in full size


Typically, this multi-application workflow would be handled by relying heavily on backend services. However, because Palantir’s products are often deployed in denied, disrupted, intermittent, or limited (DDIL) connectivity environments, there are no guarantees about the latency of reaching backend services. To solve for this, we’ve had to build these cross-application frameworks using frontend-only browser APIs, thereby eliminating the risk of disruption from unstable network conditions.


## The First Prototype


Cross-app interactivity at Palantir pre-dates our git history. From the early days of our Java Swing workspace, customers have needed to share data between our applications. The first iteration of this was accomplished using Drag-and-Drop.


Press enter or click to view image in full size


In the past couple of years, there has been increased user demand for more complicated types of app interactions. Seeing this need, one Palantirian designed a system for communication between Gaia and Workshop called “Gaia Controller.” This took form as a Workshop widget — a plug-in that extends Workshop’s functionality.


## Get Palantir’s stories in your inbox


Join Medium for free to get updates from this writer.


Remember me for faster sign in


To use Gaia Controller, the builder first has to configure it to target a specific map. Then, when the workshop is published and viewed, the controller locates the specific map by listening for messages sent through a browser` BroadcastChannel` , which is an API that allows different tabs to communicate. When the map is opened, the controller establishes a link, and the user is able to interact with various buttons in the workshop that update the state of the map.


Press enter or click to view image in full size


Although the widget was a proof-of-concept, it grew in popularity among different users and deployments that wanted more seamless cross-app UX. However, this approach was limiting: since all code lived in Workshop, it was impossible to extend its capability to work with other apps and workflows.


Our team was tasked with finding a way to recreate the logic of Gaia Controller as a first-class Palantir framework.


## Building Cross-App Communication APIs


To achieve this, the team isolated two separate behaviors implemented by Gaia Controller, and made a framework for each of them.


The first behavior was sharing state between multiple tabs. Gaia Controller let Workshop builders define variables tied to a map’s state and sync changes between the two automatically. To codify this, we developed a framework called “Shared State.” Shared State enables any application to define a data model and name for a piece of state. It then provides an API for receiving and sending updates to that state. State is organized by Media Type, giving any app access to the data it needs via the corresponding Media Type string.


The second behavior was triggering one-off imperative actions. Gaia Controller allowed users to create buttons that instantly changed the map viewport, such as with the “Zoom to” buttons in the demo above. When generalizing this, we created a framework called “Commands” that allows any application to expose arbitrary logic as an API that can be invoked by other applications. To invoke this API, an application provides a command ID and a request object, and Commands transmits the invocation and returns the response.


Press enter or click to view image in full size


The following is a demo that showcases both frameworks in action. First, Shared State is used to synchronize the selected NYC building from Gaia to Workshop. Then, the “Draw Circle” command is used to trigger a Gaia annotation workflow from Workshop.


Press enter or click to view image in full size


Both frameworks are being rapidly adopted across Palantir. A common configuration is a Workshop dashboard paired side-by-side with a different app, but new workflows are emerging, such as apps interacting with existing AIP agents.


In these cases, the agent uses Commands and Shared State as “tools” when responding to a prompt. These tools enable agents to lead the user by modifying applications on their machine while allowing user contribution throughout. Most importantly, all of these interactions happen client-side, which makes it a highly effective workflow for austere environments where backend latency is unreliable. For example, in the following demo, an agent helps the user navigate and annotate a map of New York, all based on a single prompt:


Press enter or click to view image in full size


Frontend at Palantir is ever-evolving. It spans from traditional single-app focused teams to “backend of the frontend” teams like mine. If you embrace difficult frontend challenges, come join us. Check out our open roles today:[https://www.palantir.com/careers/open-positions/](https://www.palantir.com/careers/open-positions/)


### ***Read more about Cross Application Interactivity and Commands in our Palantir docs:***


[https://www.palantir.com/docs/foundry/cross-app-interactivity/overview/](https://www.palantir.com/docs/foundry/cross-app-interactivity/overview/) **[https://www.palantir.com/docs/foundry/cross-app-interactivity/commands-overview/](https://www.palantir.com/docs/foundry/cross-app-interactivity/commands-overview/) **[https://www.palantir.com/docs/foundry/agent-studio/commands-as-tools](https://www.palantir.com/docs/foundry/agent-studio/commands-as-tools)


*If this sounds like the kind of project and impact you’re interested in, check out our open roles today:*[https://www.palantir.com/careers/open-positions/](https://www.palantir.com/careers/open-positions/) *. Our most applicable frontend postings are the “Web Application Developer” roles. We’re also hiring for these two specific roles right now:*[Software Engineer — Core Interfaces](https://jobs.lever.co/palantir/cf76738e-3030-42fa-92ac-a9446df956fc) *(Palo Alto), and*[Software Engineer — Defense Applications](https://jobs.lever.co/palantir/f7dbfdf1-0bb1-4c11-ac15-6a139cee3410) *(DC).*
