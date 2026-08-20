---
schema_version: "1.0.0"
document_id: "69cd791e7729d4f8444cebc86bd7a58d40b857a64994e9390c4139a4dd72b963"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/designing-engineering"
published_at: "2023-08-08T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:01:50.039250+00:00"
content_hash: "sha256:b5220860de933edbd8c6c0a095fcf57a8767dd1a7e821dc85c89c02350f9dbb0"
---

# Designing Engineering

Target has long valued design for all. From Michael Graves teapots to Christopher John Rogers dresses, valuing design has brought high-end designs to mass-distributed products at accessible prices.


I first encountered this as an exceptionally nerdy 14-year-old, when I couldn’t explain why I needed the Michael Graves teapot, but I absolutely had to have it. Growing up in northern Minnesota in the ‘90s, I had little exposure to innovative product design. I had never seen an item that was not only utilitarian but so invitingly playful. It was the first time I remember being enamored by design, but certainly not the last. I craved art, design, architecture, and fashion and got a master’s degree from Cranbrook Academy of Art (one of the most well-known institutions for pumping out heavy-hitting designers like Eames and Knoll) prior to entering a career in technology. What surprised me when transitioning from art into application development is how much repository design affects how we work, and how we often push through using terribly designed repos in the name of speed-to-production.


World-class chefs don’t cook with dull knives. World-class athletes don’t play with last year's equipment.


World-class engineers shouldn't code with poorly designed repositories.


Designing great repositories makes our day-to-day lives more pleasant. However, there’s one beautiful outcome of designing your repo for all: it gives you the ability to open your team and your organization up to a broader group of engineers. The ability to pull in talent from any level or background makes for a more agile team and ultimately increases speed-to-production.


I currently lead the team that owns Target's internal design system. We have a small team maintaining a product that serves over 700 repositories. Designing our repos for all has been essential in cultivating a large community of contributors to our product. The small amount of effort we have put into creating approachable repos has resulted in a community of engineers who are willing to jump in and add value to our products.


Below are a few guidelines that are at the forefront of how my team thinks every day as we make choices for ourselves and for our hundreds of users. You can adopt these practices to elevate your own repositories.


Build local environments that are easy to use.


Painless and swift set-up with clear onboarding instructions and automation means external contributors are encouraged and enabled to submit PRs, and imposter syndrome is minimized for new team members because they are quickly successful.


- Write comprehensive documentation for local environment setup that is aimed at entry-level engineers.


- Build services that are easy to connect to or have mock services.


- Be able to run parts of large applications semi-independently.


- Be conscious of the number of micro


front-ends


(MFEs) you use.


Make your file structure shallow and consistent.


File structure should be easy to understand within minutes of opening a repo in an editor.


- Don’t reinvent the wheel, follow industry standards.


- Keep a flat file structure.


- Make sure file names indicate what the file contains and what files are related.


- Put tests next to source files with the same naming convention, or in an adjacent file with the same naming and structure, or follow the convention established in the tooling’s ecosystem.


Use a design system and override it minimally.


Keeping styling simple makes component library upgrades predictable and makes UI implementation approachable for entry-level and backend engineers.


- Make any needed modifications to components in one clearly named CSS file and import it globally.


- Ensure that custom styles are agreed upon by Engineering, UX, and Accessibility.


- Avoid writing styles with Javascript, TypeScript, styled components or inline styles.


- Aim to rely on the design system for 90% of your application’s styles.


- Learn to use a grid system effectively.


Invest time in the deployment infrastructure of your application.


Complicated and brittle pipelines don’t allow entry-level engineers to take on deployment responsibilities.


- Automate your deployment with Git tagging or branch merging.


- Use the same deployment system across all of your team’s assets.


- Aim to enable all engineers to make releases.


Make a testing strategy.


Strict testing patterns will allow you to see missing or incorrect tests in pull requests and will result in a greater likelihood that new engineers can guess what should be tested just by looking at your repo.


- Decide as a team what parts of your application to test with which testing tools.


- Test utility functions with unit tests.


- Test major workflows and complicated components with functional tests.


- Test application layouts with a visual regression tool.


- Use test-driven development to build testing culture from the inception of a codebase.


Adopt new technology at pace with the industry average.


Shiny new tools are exciting, but being on the bleeding edge will make it harder to find engineers who are proficient in your tech stack. When using new or experimental technology, it is more likely to accidentally deviate from industry best practices because they may not yet be established.


- Keep your core tech stack boring by using the industry’s most common tools and frameworks.


- Use learning time to experiment with new technology and push adoption of beneficial advancements industry-wide and within your company.


Adopting these practices may feel at first like you are


“dumbing down”


your repositories. You may feel some fear of missing out on the latest technology tools. Try to keep in mind that the end goal is to create a technology industry that is more approachable to a broader spectrum of people. Together, we can change the exclusive culture of software engineering by simply converting our repositories, one at a time, to be better designed for all.


## RELATED POSTS


### 👍 Custom Emoji Management: How Target enhances its tech culture with creativity 🎨 and technology 🕹


By Jay Kline, July 28, 2022


When Target HQ first started to use chat systems, those systems allowed simple emoji usage, quickly turning :-) into 😀, and a few other simple faces. As chat technology evolved, Unicode standardized more sophisticated emoji. Eventually, many chat systems allowed administrators and sometimes users to add custom emoji. This gave us some leeway and ability to get creative when it came to what emoji to use when chatting internally at Target.


### Sharing Knowledge on Image Optimization Using Internal Blogs


By JJ Wittrock-Roske, June 1, 2023


Learnings from the recent Smashingconf, and how they apply to the Web team's work at Target.
