---
schema_version: "1.0.0"
document_id: "b773f85916d65763ca565884a0a4c29f6be1293a3e3413baea07ed7c8a07ddd5"
company_key: "shutterstock-inc-common-stock"
company: "Shutterstock Inc."
source_id: "shutterstock-inc-common-stock-rss-4c794ed87dc5"
canonical_url: "https://tech.shutterstock.com/2019/01/08/building-api-reference"
published_at: "2019-01-08T05:00:00+00:00"
first_seen_at: "2026-07-20T23:18:13.615263+00:00"
fetched_at: "2026-07-28T22:26:51.183990+00:00"
content_hash: "sha256:cca93bb5992d86b899f6c5ab23cce379e3ff3f1c921517de0d644c31e9e15ecd"
---

# How We Built Shutterstock's New API Reference

API reference information isn’t always given much attention in development processes. Teams often generate reference for commands or endpoints and assume that this is enough for developers to consume their API. However, we know that complete API reference includes much more information than just a list of commands or endpoints.


When we redesigned our[developer portal](https://developers.shutterstock.com/) , we prioritized API documentation and wanted to include complete information that would help everyone who wanted to use Shutterstock’s API. (For more information on that redesign, see[Behind the Scenes of Shutterstock’s API Documentation Redesign](https://www.shutterstock.com/blog/shutterstock-api-documentation-redesign) . The goal was to smooth the process of integrating with our API solutions, and a big part of that was to provide an API reference that helps developers, from first-time users to experienced software developers, easily plan, code, and debug applications that use the API.


We put together[API reference](https://api-reference.shutterstock.com/) that is technical enough for advanced developers but welcoming enough for developers who are exploring or getting started with the Shutterstock API:


## Docs as Code


Shutterstock’s API reference, like most of our internal documentation work, follows the principle of[Docs as Code](http://www.writethedocs.org/guide/docs-as-code/) , which handles documentation alongside application code wherever possible.


- We store the documentation source code in the same repository as the application source code or at least in the same source control system.
- We build the documentation with the same internal processes as the application, typically Jenkins builds with Docker images, hosted on Amazon Web Services.
- The development process is the same for the documentation as for the application, requiring code review, issue tracking, and prioritization.
- We use plain text markup (Markdown) so the process of authoring and reviewing the documentation is similar to that of the application code. Writing in plain text allows technical writers and application developers to collaborate without having to learn a new platform or documentation language.


We write the endpoint reference in[Swagger/OpenAPI](https://swagger.io/docs/specification/about/) format because it’s a flexible standard that many open-source tools support. OpenAPI documents can fit into the DevOps cycle in many places, including being generated from code and as a basis for automated tests.


## Merging Generated and Handwritten Information


One of the most common pieces of feedback on API documentation is that a list of endpoints and parameters is not sufficient information to integrate with an API. Developers don’t use API commands in isolation, even in simple integrations, and commands can have prerequisites or relationships that aren’t obvious from generated information.


For this reason, we summarized some of the[developer portal documentation](https://developers.shutterstock.com/documentation) and embedded it within the API reference. This way, developers who are referring to the API reference have easy access to task-based information about critical topics like authenticating to the API, searching for media, and licensing and downloading media, without having to leave the page. Because all of this information is on the same page, it shows up in the navigation and appears immediately in searches, whether the user uses the built-in search or the browser (` Ctrl+F` ) search.


## Providing Helpful Examples


API documentation isn’t complete or useful without good examples. We reviewed our server logs to see what languages commonly accessed our API and used that information to decide which languages to provide examples in. We settled on cURL, as an easily accessible and universal client, and JavaScript/Node.JS and PHP as the most common web frontend languages that our consumers use. We made sure that many of the cURL and Javascript examples can run without any modifications. That way, users can copy the examples into their platform and try them out without having to change anything.


We spent a lot of time polishing the templates that our tools use to generate examples from OpenAPI documents, but quickly realized that some of the generated examples were accurate and runnable but didn’t show realistic uses of the API endpoint. For these critical endpoints, such as the endpoints that search for media, license media, and download media, we added handwritten examples in the OpenAPI files and used them to override the generated examples with helpful and realistic examples.


## Building the API Reference


Based on the principles that we outlined in[Behind-the-Scenes Look at Shutterstock’s API Documentation Redesign](https://www.shutterstock.com/blog/shutterstock-api-documentation-redesign) , we set off to redesign the developer portal and API reference with significant updates to documentation by following this process:


- **Conduct in-depth user research** : Aside from the research on which languages our consumers use, we interviewed business partners and resellers to learn what information they needed the most and what format they liked reference information in. We reviewed our support information to see what common questions and problems consumers had.
- **Define the necessary documentation components** : Based on data from our user research, we identified several areas of improvement, ranked them in order of potential impact on the developer experience, and set up sprints for high-priority items.
- **Finalize UX and design elements to support documentation components** : With the documentation components in mind, we updated the format of the developer portal and documentation to match the company’s most recent UI guidelines. We also settled on a three-column format for the API endpoint reference, which lets us put examples right next to information about each endpoint.
- **Write the documentation** : We created the content for the updated documentation using two principles of technical writing and UX design: task-based writing and progressive disclosure. Task-based writing begins from the user’s perspective on the tasks they’d like to accomplish and aligns the documentation with the goal of helping users complete their tasks. Progressive disclosure focuses on providing the most important information first and revealing other levels of detail as the user needs them.
- **Expand on platforms** : We decided to use open-source tools to generate the documentation, including the[OpenAPI](https://www.openapis.org/) specification and the[Widdershins](https://github.com/Mermade/widdershins) and[Shins](https://github.com/Mermade/shins) documentation generation tools. It took some extra coding to set these tools up, integrate them with each other and with our internal build and deployment systems, and get the output we wanted, but now we have a lot of control over the documentation generation process.
- **Test and gather feedback for updated documentation** : After we completed the documentation and developer portal, we conducted an internal launch for Shutterstock developers during our recent Hackathon event. During the Hackathon, many developers worked directly with the new documentation and provided valuable user feedback, some of which were immediately implemented with others slated for the next iterations of developer portal optimization.


## Next Steps


As for our next steps, we’ve got lots of improvements in mind for the API reference, including continuing to refine the layout and organization of the API reference page itself. You can see what we’re working on and submit your feedback for improving our documentation at[api-feedback.shutterstock.com](http://api-feedback.shutterstock.com/) .


Now that we’ve redone our developer portal and API reference documentation, it’s a great time to give our API a try and see what you can build with it. We’re very interested to hear about how you use our API, how quickly you can get started with it, and what information you need to use it. See the API reference here:[api-reference.shutterstock.com](http://api-reference.shutterstock.com/) .
