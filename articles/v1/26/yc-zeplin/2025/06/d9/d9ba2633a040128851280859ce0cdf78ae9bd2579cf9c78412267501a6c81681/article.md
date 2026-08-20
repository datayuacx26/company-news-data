---
schema_version: "1.0.0"
document_id: "d9ba2633a040128851280859ce0cdf78ae9bd2579cf9c78412267501a6c81681"
company_key: "yc-zeplin"
company: "Zeplin"
source_id: "yc-zeplin-news-import-3304292ba89d"
canonical_url: "https://blog.zeplin.io/product-news/25-new-features-to-dig-into/"
published_at: "2025-06-10T13:00:00+00:00"
first_seen_at: "2026-07-22T21:02:03.888972+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:f9f2b0aa4c9a1c0950a1d10e1bd1408590262eeb7b0883f79732dca92064f8f5"
---

# 25 new features to dig into: 3 mains, 22 sides · Zeplin Gazette

Ooh là là! This one’s a feast.


For the past 6 weeks, we focused on areas of Zeplin that hadn’t gotten much love in a while — modernizing older parts of the app, cleaning up long-standing bugs and laying the groundwork for what’s next.


We ended up cooking up a full-course launch for you: **3 major updates, the mains, served with 22 (!) sides.** Let’s dig in.


## The mains


### Zeplin MCP server: AI-assisted UI development


You can now connect AI agents like Cursor, Windsurf, and VS Code (w/ Copilot) directly to Zeplin using our official MCP server. This lets developers generate code directly from designs in Zeplin — helping automate repetitive UI tasks and speed up development — and hey, if the AI gods are on your side, maybe even production-ready code.


Here’s how it works:


Using the MCP server, your AI agents can tap into:


- **Component and screen specs:** Detailed specs and assets for both components and entire screens — helping agents generate UI code that closely matches the designs.
- **Documentation:** Annotations added to screens that provide extra context, like how things should behave or tips for implementation — letting the agent go beyond static visuals and build real interactions.
- **Design tokens:** Colors, typography, spacing, and other design variables used across the project, so your agent can reuse existing tokens where possible.


To get started, head to a project in Zeplin and check out the instructions in the right panel!


### Dynamic project types


Until now, every project and styleguide in Zeplin had a set “type” — like iOS, Web, or Android — which we used to tailor specs, assets, and code snippets for each platform.


But over time, needs have changed. Responsive web development has come a long way, and many teams now use a single design across multiple platforms. This setup, though, forced designers to export the same screens into multiple projects just to support different platforms — making it harder to keep comments, documentation, and updates in sync.


So we fixed it: With this release, we’re removing project and styleguide types entirely. **Developers can now choose their platform dynamically and get the specs, assets, and snippets they need.**


### Tailwind CSS snippets


Zeplin now has an official Tailwind CSS extension, so devs using Tailwind can now grab:


- Utility class names from individual layers
- Theme variables from the styleguide — including colors, text styles and spacing


We’ll start to roll this feature out next week. When we do, head over to the[Extensions](https://extensions.zeplin.io/) page and add the Tailwind extension to your project!


*P.S. With this update, we also gave[Zeplin Extension Manager](https://github.com/zeplin/zem)[(ZEM)](https://github.com/zeplin/zem) a refresh making it easier to build and maintain your own extensions — it now fully supports TypeScript!*


## The sides


Alongside the mains, we’ve been busy shipping plenty of smaller improvements — some long-awaited fixes, some quality-of-life upgrades, and a few nice surprises. Here are some of our favorites:


### Reactions for comments


You can now add proper reactions to comments — the kind that show up right on the comment, not as separate messages. We picked a set of emojis we enjoy using, but if we missed your favorite emoji, just give us a shout at our[Discord](https://zpl.io/discord) .


### Version-aware comments


Ever find a comment and wonder "wait, was this for the latest version?" Now you’ll know. Zeplin shows which version a comment was added on — this makes it easier to track feedback over time and jump straight to the relevant version when you need to.


### Area comments


One dot isn’t always enough. If your feedback’s about a bigger part of the design, just click and drag to select an area while adding a comment — voilà!


Area comments are now available on screens, with flow support landing shortly.


### Unified web and desktop links


Since Zeplin launched back in 2015, we always had two separate links for our web and desktop apps. But times have changed — turns out, one smart link is just... smarter.


Now when you share anything in Zeplin, you get a single link. Whether you prefer web or desktop, it’ll open the app that works best for you.


### Sketch Stacks support — from day one


Our friends at Sketch recently introduced Stacks (their long-awaited version Auto Layout). Good news — Zeplin already supports it!


Big thanks to the Sketch team for working with us on this one. 🧡


### Min/max width and height — now visible in Zeplin


Figma lets designers set minimum and maximum sizes for elements. With this update, you’ll see these values right in Zeplin — making it easier to build responsive, flexible layouts.


### AVIF assets


Zeplin now supports AVIF, an image format that offers better compression compared to older formats — **typically 20–30% smaller** than WebP. This means faster load times for your web projects, all while maintaining high-quality images.


Starting today, for all screens exported to Zeplin, any exportable layer will now have an AVIF asset by default!


### Figma color variables in design token export


Zeplin makes it easy for developers to[export design tokens](https://support.zeplin.io/en/articles/5160298-exporting-design-tokens) in JSON format, providing consistent design data straight from your styleguide.


Until now, the export didn’t include color variables exported from Figma. With this update, Zeplin now includes color variables as well, making it easier to maintain the connection between design and code.


### Import GIFs (or… JIFs?)


This one was fun to build — and highly requested, especially by folks working on things like emails and marketing pages. You can now import animated images straight into Zeplin using the desktop app — simply select “Edit > Import PNG, JPG or GIF…” from the menu.


*P.S. Only Jen at Zeplin insists it’s “JIF”. Someone please have a word.* 🙄


### Re-request approval


[Approvals](https://blog.zeplin.io/we-fixed-approvals/) are a great way to share designs with stakeholders and get feedback or sign-off. But until now, you couldn’t re-request approval after making updates — you'd have to start fresh. Well… not anymore! You can now re-request approval on the same version, keeping everything in one place.


### Request approval from yourself


One of the first feature requests we got after launching Approvals was: “Can I request approval from... myself?” Now you can. It’s a handy way to keep a personal to-do list of what still needs polishing.


### Project dashboard improvements


A couple of updates to make your experience in the project dashboard smoother:


#### Sort preference saved


Previously, Zeplin kept defaulting to sorting by sections — even if you changed it. Frustrating, we know. Now, your preferred sort option actually sticks.


#### Collapse/expand all sections


You can now right-click anywhere on the dashboard to collapse or expand all sections quickly. A small improvement, but a big time-saver for staying organized.


### Figma plugin now launches faster


Earlier this year, we rolled out[major performance improvements](https://blog.zeplin.io/6-weeks-dedicated-to-performance/) across Zeplin and the Figma plugin — especially to speed up export times. With this latest update, we’re continuing to improve the experience: the plugin now launches significantly faster, so you can get started with exports without the wait.


### …and even more goodies


- Assets from Figma component variants would always have the same name (e.g. “Icon”) — now the **asset name will also include the props** (e.g. “Icon Arrow Right”).
- **Screen variants are now more prominent** , so devs will have to find a new excuse for missing them. Ahem.
- You can now **mention users in comments even if they’re not in the project** — typing @name will trigger an invite.


## A small thanks 💕


We hope you enjoy this update! It felt right to focus on items that had been in the backlog for a while, and we’re really excited about the new features and possibilities they bring.


As always, friends, we’re eager to hear your feedback — ping us on our[Discord](https://zpl.io/discord) or shoot an email to support@zeplin.io.


## What’s next: AI in Zeplin


Over the next couple of months, we’re bringing AI into Zeplin — but not just for the sake of it. We’ve been quietly observing, looking for the right moments where AI can genuinely help.


We’re starting with two key areas: **Organization and Reviews** . Our goal is to reduce manual work in organizing projects and help designers get effective reviews before sharing designs with stakeholders. If you have any thoughts or suggestions, we’d love to hear them — join our[Discord](https://zpl.io/discord) !
