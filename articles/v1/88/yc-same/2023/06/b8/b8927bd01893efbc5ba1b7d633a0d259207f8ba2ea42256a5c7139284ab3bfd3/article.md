---
schema_version: "1.0.0"
document_id: "b8927bd01893efbc5ba1b7d633a0d259207f8ba2ea42256a5c7139284ab3bfd3"
company_key: "yc-same"
company: "Same"
source_id: "yc-same-news-import-393e713dbea1"
canonical_url: "https://million.dev/blog/behind-the-block.en-US"
published_at: "2023-06-01T00:00:00+00:00"
first_seen_at: "2026-08-10T02:52:25.226434+00:00"
fetched_at: "2026-08-10T02:52:27.075037+00:00"
content_hash: "sha256:fb1efb916e1ab3b466d52fa5211326473b743998099379f03d1ffe16f956a927"
---

# Behind the block() | Million.js

### React renders` <


Loader


/>


`


component


Initially, React is responsible for rendering the` <


Loader


/>


`


component. This process involves creating the necessary DOM elements and applying any initial properties or styles. During this phase, React is managing the lifecycle and state of the component, allowing for rich features such as state management, lifecycle methods, and more.


### React mounts` <


Loader


/>


`


and puts the DOM element in the ref


Following the rendering process, React then mounts the` <


Loader


/>


`


component. This involves inserting the component into the DOM and making it visible to the user. At this point, React also updates the ref with the DOM element. A ref in React is a way to hold local state that doesn't invoke rendering, and in this case, it's being used to store a reference to the DOM element.


### Million.js renders` <


App


/>


`


into the ref


Finally, the ref is handed over to Million.js, a fast, lightweight virtual DOM. Using the DOM reference stored in the ref, Million.js renders the` <


App


/>


`


component directly into this element. This allows Million.js to manage the` <


App


/>


`


component separately from React, leading to potential performance benefits and isolation of responsibilities.
