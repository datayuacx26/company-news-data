---
schema_version: "1.0.0"
document_id: "412abd0aa42c789971064082b7713b97e4aec750fd358bdb7a44a95b24530ef3"
company_key: "yc-wasp"
company: "Wasp"
source_id: "yc-wasp-news-import-57e353815c4b"
canonical_url: "https://docs.opensaas.sh/blog/2025-11-19-gemini-3-open-saas/"
published_at: "2025-11-19T00:00:00+00:00"
first_seen_at: "2026-07-24T06:46:50.580409+00:00"
fetched_at: "2026-07-28T21:58:34.938322+00:00"
content_hash: "sha256:5031ec3049e813e49b8180eb523af953a51e4382d76b216ca3d264b8c612e3a8"
---

# Building a SaaS with Gemini 3 and Open SaaS

Nov 19, 2025


[Vince Dev Rel @ Wasp](https://wasp.sh/)


I’m always skeptical of demos whenever a new model gets released. So I had to test Google’s new Gemini 3 model myself.


I wanted to give a more unfiltered look at some real-life SaaS development tasks with **Gemini 3** , **Cursor** and our very own **Open SaaS boilerplate** , so I recorded a quick video of the process.


The results were impressive and it was able to handle 3 tasks of increasing complexity. I’ll definitely be using this as my new go-to model for feature development in future projects!


Check out the full video below to see it in action:


## Refactoring the Landing Page


Section titled “Refactoring the Landing Page”


I started with a fresh copy of the[Open SaaS template](https://opensaas.sh/) and gave Gemini 3 a simple prompt: **look at the demo AI app and update the landing page to reflect it.**


Using Cursor’s “Plan Mode,” Gemini 3 proposed three different design styles. I chose an “interactive product-led demo” approach. The model generated a plan and executed it on the first try, building a custom interactive component and integrating it seamlessly into the existing landing page hero section.


This was a cool surprise, as I hand’t thought about building an interactive component to showcase the app’s features. A nice win!


## Refactoring with Structured Outputs


Section titled “Refactoring with Structured Outputs”


Next, I ramped up the difficulty. The demo app originally used OpenAI’s function calling to generate a daily schedule. I asked Gemini 3 to **refactor this to use OpenAI’s “Structured Outputs” feature instead.**


The model:


1. Identified the need for a Zod schema to define the response format.
2. Switched the model to` gpt-4o-mini` .
3. Replaced the legacy tool definitions with the new` response_format` parameter.
4. Updated the parsing logic to handle the structured response.


This refactor modernized the codebase without breaking existing functionality. Nice. This saved me a ton of time reading the docs and figuring out how to use the new feature myself.


## Adding a Collaborative Chat Interface


Section titled “Adding a Collaborative Chat Interface”


For the final test, we asked for a major feature addition: **a chat interface to collaborate with the AI on the creating a daily plan (with subtasks) before committing to it.**


Gemini 3 broke this down into a multi-step plan:


- **Backend:** Define new schemas and actions to handle the chat history and plan proposals.
- **Frontend:** Implement a chat component using` shadcn/ui` components (specifically the ScrollArea).
- **Integration:** Connect the chat interface to the planning logic, allowing users to refine tasks (e.g., “prioritize filming the video”) before accepting the final schedule.


The result was a functional chat interface where the AI intelligently adjusted the schedule based on user feedback, embedded directly into the app. I loved how it injected a nice styled component into the chat window, something that I wanted (it read my mind!) and would have taken me hours to build in the past.


I was really happy with the results of this challenge!


## Conclusion


Section titled “Conclusion”


The combination of **Gemini 3** , **Cursor** , and **Open SaaS** proved to be pretty powerful. The model didn’t just generate code; it understood the project structure (Open SaaS organizes code vertically by feature), followed architectural patterns, and successfully implemented complex refactors and new features with minimal human intervention.


Overall, I was very impressed and I think other SaaS builders would be smart to give this stack a go.


⭐️ Star[Open SaaS repo](https://github.com/wasp-lang/open-saas) and start building your next idea today!


**Tags:**


- [gemini](https://docs.opensaas.sh/blog/tags/gemini/)
- [cursor](https://docs.opensaas.sh/blog/tags/cursor/)
- [ai](https://docs.opensaas.sh/blog/tags/ai/)
- [tutorial](https://docs.opensaas.sh/blog/tags/tutorial/)


[The Public Development Roadmap for Open SaaS](https://docs.opensaas.sh/blog/2025-11-21-open-saas-public-roadmap/)


[Open SaaS v2.0 -- ShadCN UI, LLM-friendly, MoRs, and more.](https://docs.opensaas.sh/blog/2025-07-29-open-saas-version-2/)
