---
schema_version: "1.0.0"
document_id: "79bea95709b87dd9662f4f9a429e9748336eeea165f21bbcc2888eceab602769"
company_key: "yc-zeplin"
company: "Zeplin"
source_id: "yc-zeplin-news-import-336c2f98fc66"
canonical_url: "https://blog.zeplin.io/collaboration/what-are-user-flows/"
published_at: "2023-04-07T18:25:07.076+00:00"
first_seen_at: "2026-07-22T21:02:04.822116+00:00"
fetched_at: "2026-07-28T21:33:46.196319+00:00"
content_hash: "sha256:664c3348bc253fe090d2c3b7fb9b13185b0a3369958a17275394fce8dc2ebee3"
---

# What are user flows?

A user flow is a series of potential steps and paths a user might take as they interact with a product. A user flow diagram is a visual representation of all of these potential user paths, starting with a single entry point and spreading out into multiple branches that represent different user actions and decisions. User flows are designed to depict the full range of experiences a user might have within a product — the good, the bad, and the ugly.


Though user flows are often thought of as part of the UX design process, flow diagrams are used throughout all stages of the product life cycle and beyond. At the design stage, the user flow maps out the plan for the product being built. During development, the user flow guides developers as they execute the product design. And even after the product is released, the user flow is used to find friction points, optimize the user experience, and add new features.


As the product's "blueprint," the user flow is the backbone of any development project. And just like your actual spine, it requires maintenance and care — otherwise, it can cause the entire body (or team) a lot of pain.


## 3 types of user flow diagrams


A user flow diagram maps out all of the steps a user might take as they move through the product, from entry point to final action. One flow diagram might map out an entire product landscape, or it might concentrate on one feature, set of features, or an individual task flow. It all depends on what the product team is working on at any given time.


User flow diagrams can take different forms depending on how simple or complex they are. The three main flow subtypes are chart flows, wire flows, and screen flows.


### Flowcharts


A flowchart is the simplest type of flow diagram. Using basic shapes to represent touchpoints, the flowchart maps out all of the user's possible decisions and paths at a high level without yet diving into the details of what those decisions might look like. It's used to design, QA, and correct the product's functionality before starting on any wireframes or UI design.


Here we have the flowchart for Snack Overflow's onboarding sequence. It just outlines the basic functional steps the user will take to begin using the product. There's no information about what these steps will look like or how the user will get between them — we're just laying out the essentials.


### Wire flows


Once UI design begins, the user flow diagram evolves. As designers complete the product's wireframes, they'll replace the chart flow's nodes with the wireframes that correspond to each user action. These updated flow diagrams are called wire flows.


Chart flows explain what decisions a user will face, what order those decisions will appear, and what outcomes each decision will have. Wire flows offer another layer of information, illustrating *how* the user will make each decision.


The wire flow example establishes what *type* of information the user will use to customize their profile, as well as *how* that information will be gathered.


### Screen flows


If wire flows are flows with wireframes, then it doesn't take much to figure out that screen flows are flows with mockups or product screens. The screen flow offers the most detailed visual map of the product, giving teams a final opportunity to make changes before the design moves to prototype. It's also the version that goes on to become the main user flow document used by all product and non-product teams.


The final screen flow features full designs, with the actual buttons, selections, and copy the user will see as they move through the sequence.


## User flows vs. user journeys


It's very common to see designers use the terms "user flow" and "user journey" interchangeably. Though both are similar and they do overlap, there are important differences between the two. User flows and user journeys are a lot like identical twins — they look very similar, but when you get to know them, you'll find they're two completely different people.


Like a user flow, a user journey depicts the path a user takes to accomplish a goal. Unlike user flows, the user journey includes steps both inside and outside the product. If a user switches from the mobile app to the web page, opens up the chatbot, or picks up the phone to call customer service, all of those steps will be included in their user journey. User flows, on the other hand, are contained in a single product's UI.


## Two user flow examples


There are two main "levels" of user flow complexity. There are standard user flows, which branch out to illustrate user decisions and their results. Then there are task flows, which depict a single linear sequence with no branches or decisions. Let's take a look at some examples.


### Standard user flow example


We'll start with what most people would call a "regular" user flow, which has user decisions and branch points. For this example, let's take a look at the marketplace search sequence from our demo food delivery app.


The entry point for this flow is the "Market" icon on the app's homepage. From there, the user moves through the following steps:


1. From the main marketplace screen, the user taps the search bar and triggers a dropdown of recent and suggested searches.
2. The user either selects one of the suggestions or enters their own search term.
3. The app checks for vendors matching the user's search term. Here, the flow splits into two branches:


1. If the app finds no relevant vendors, it brings the user to a "no results" screen and suggests a keyword it knows is valid: baklava.
2. If the user's search is valid, the app takes the user to the food item page to make a purchase or add the item to a wishlist for later.


Of course, this isn't the end of the user's journey; it's just the end of the marketplace flow sequence. From the endpoint of the search sequence, the user has several options: they might close out of the app entirely, return to the top of the sequence and repeat the flow with a new search term, or they might place an order. If they choose to order an item and check out, they'll move from the marketplace flow to the app's payment flow.


### Task flow example


The payment sequence is an example of a task flow. It has no branch points or user decisions; every user that enters the payment flow will move through the same steps to achieve the same outcome. In many cases, a task flow reflects the[happy path](https://blog.zeplin.io/what-is-the-happy-path) — the shortest path a user can take within a product to achieve their desired result.


The endpoint of the marketplace sequence, the "add to cart" button, is also the entry point of the payment sequence. From there, the flow is simple:


1. The user finishes adding all of their desired items to the cart and presses the pay button.
2. The app prompts the user to authorize the payment via ApplePay.
3. Once the order is processed, the payment flow ends by returning the user to their account page.


From here, the user can close out of the app and wait for their delivery to arrive. Or, they can click the button at the bottom of the screen to move to the entry point of the app's tracking delivery flow.


## How to use user flows in each phase of production


It's not just the format of the flow diagram that changes as the product evolves. User flows have different uses at different stages of a product's life cycle:


- **Product ideation:** Designers and PMs use simple flowcharts to sketch, compare, and refine different potential solutions to achieve the design objective.
- **UX design:** Once a design solution is approved, the UX designers use the flow diagram and wireframes as a starting point to break the product's broad functionality down into step-by-step user paths.
- **UI design:** UX flows act as blueprints for user interface designers to understand what wireframes and mockups they need to design and where each screen will go in relation to one another.
- **Development:** User flow charts are an essential part of handoff documentation, putting screens into context and giving developers key information about how the design's components should behave and interact.
- **User testing:** PMs use flow diagrams to evaluate real user behavior and determine where they commonly encounter errors, then work with designers to eliminate product issues and guide users along the ideal path to their objective.
- **Optimization:** Sales, marketing, and revenue teams evaluate flows to identify low conversion rates, then work with UX and UI designers to more effectively move users through sales and conversion funnels.
- **Fixes and new features:** As product managers collect user feedback and teams iterate on ways to improve the product, the product's original flow diagrams make sure all new projects start on the same page.


A product's user flows are the structural foundation for the whole project. It's a staple resource for nearly every team that touches the product: UX designers, researchers, copywriters, UI designers, developers, project managers, and even departments like sales and marketing. When creating and maintaining an essential artifact, it only makes sense to use tools that make it easy to organize, update, and maintain. Check out[how we recommend using Flows](https://blog.zeplin.io/introducing-flows-and-annotations-a-new-way-to-communicate-design-intention) to create perfect flow diagrams and use them to power the rest of your product process.
