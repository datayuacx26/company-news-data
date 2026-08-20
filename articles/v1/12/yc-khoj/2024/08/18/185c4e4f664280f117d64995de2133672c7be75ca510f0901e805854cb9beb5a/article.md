---
schema_version: "1.0.0"
document_id: "185c4e4f664280f117d64995de2133672c7be75ca510f0901e805854cb9beb5a"
company_key: "yc-khoj"
company: "Khoj"
source_id: "yc-khoj-rss-06f7f2cb1884"
canonical_url: "https://blog.khoj.dev/posts/new-ux-fresh/"
published_at: "2024-08-07T00:00:00+00:00"
first_seen_at: "2026-07-25T10:45:44.578572+00:00"
fetched_at: "2026-07-28T22:01:08.511319+00:00"
content_hash: "sha256:a2e96f2c7b617977cba943a011306f61baac0ea7eca304027b360ab7f1133a6e"
---

# Springing forward in our web stack

# Springing forward in our web stack


front-end


web stack


UIUX


Aug 7, 2024


[Saba Imran](https://twitter.com/sabaimran_go)


---


Our web stack at Khoj has been quite unique. We have a Python web server using FastAPI as our app layer and Django for ORM and admin pages. While this served us fine initially, it’s led to some[performance issues](https://github.com/fastapi/fastapi/discussions/8009) that keep me up at night and deserve their own discussion.


Our front-end journey is equally interesting. We recently launched[a brand new UX](https://app.khoj.dev/) (we call it Spring UX), migrating our front-end code base in the process.


For three years, we served pages using Jinja templates with FastAPI. This approach allowed for partial server-side rendering but meant building web components from scratch. Yes, we were using plain HTML, CSS, and JavaScript even after reaching 10,000 users! We chose this path of under-engineering for several reasons:


1. Lightweight static HTML pages
2. Simplified development without cross-OS compatibility issues
3. Abundant solutions to common problems, making AI assistance more effective
4. Avoiding time-consuming migration to modern frameworks while validating our product


This strategy worked well initially, but as we grew, limitations became apparent. We faced development bottlenecks due to writing core code from scratch and struggled with code duplication. As we prepared for a major redesign, it became clear that our front-end needed an overhaul too.


We needed a graceful migration path that minimized server-side changes. After considering options like[Svelte](https://kit.svelte.dev/) and[HTML Components](https://www.w3.org/TR/NOTE-HTMLComponents) , we settled on Next.js with[static site generation](https://nextjs.org/docs/pages/building-your-application/rendering/static-site-generation) . This choice allows us to:


- Leverage popular component libraries
- Migrate without a complete overhaul
- Minimize new technology risks
- Potentially move our front-end entirely to a CDN in the future


We adopted the startup starter pack ™️, aka[Next.js](https://nextjs.org/) ,[Tailwind](https://tailwindcss.com/) , and[Shadcn](https://ui.shadcn.com/) .


An additional consideration was our[Python package on PyPI](https://pypi.org/project/khoj/) . We needed the new front-end pages to work within this package. Our solution involves building files in our GitHub action, packaging them into a distribution directory, and serving them from a static directory.


Since then, we’ve moved from a cartoon-like UX to a more colorful, modern interface we call “spring” – symbolizing both our visual refresh and a leap forward in user experience.


This journey highlights the evolution of our tech stack, balancing simplicity with the need for scalability and “modern practices”. Eventually, you do actually have to remove the stilts on your codebase as your product grows and user needs evolve.


Before After
