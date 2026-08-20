---
schema_version: "1.0.0"
document_id: "57becfd8069453361dc952fed2fba9d115aa0853bcb47d5c07ec5f7411fa1767"
company_key: "yc-extend"
company: "Extend"
source_id: "yc-extend-news-import-054f4f06cd55"
canonical_url: "https://www.extend.ai/resources/extend-ui-kit"
published_at: "2026-06-09T00:00:00+00:00"
first_seen_at: "2026-07-21T19:22:10.257736+00:00"
fetched_at: "2026-07-28T21:54:56.147054+00:00"
content_hash: "sha256:b92b8add7e4510779bda0f80a3913075faa1d173ecca1d0596ac06d6d94e8dba"
---

# Introducing Extend UI: open source UI kit for modern document apps

**TLDR**


- We're open sourcing the Extend UI kit, a React component library with a shadcn registry for building document products.
- 15 customizable components for PDF viewer, DOCX/XLSX/CSV viewer and editor, file systems, bounding boxes, and more.
- Built it for Extend Studio first, now open to all after customer requests.
- AI teams can combine these UI elements to build modern applications with documents, from agent handoff and internal tooling to real-time user-facing flows.


When we started building Extend, we tried every file viewer and document component library we could find. None of them had all the functionality and polish we wanted, so we ended up building our own. It turns out that building DOCX, XLSX, and PDF viewers that work at scale is not trivial.


The UI components we used were originally meant to be internal-facing but, to our surprise, sharing UI components was a frequent customer ask.


**Today, we're open sourcing a document UI kit for the broader community.** It's MIT licensed and free to use so anyone who wants to build modern document experiences can do it faster and with a production-ready system.


It's battle-tested on millions of pages running through our system every day. Plus, we use and maintain these components so the kit will improve over time.


Check it out and let us know what you think:[https://www.extend.ai/ui](https://www.extend.ai/ui)


## What Extend UI gives product teams


Extend UI gives teams 15 document-specific primitives including:


- Viewers and editors for PDF, DOCX, XLSX, CSV, and TSV files.
- File Upload and File Thumbnail components for intake and previews.
- Bounding box citation viewer for users to review extracted text for accuracy and with precise source tracing.
- Document split viewer for reviewing large document packets as separate files.
- E-signature for signing and document completion workflows.
- File system view for browsing packet files, folders, and document previews.


Extend UI is distributed as a shadcn-style registry, so teams can add individual components directly into their own codebase and customize as needed.


Using the component library is simple. Here's a <90 second walkthrough:


Your browser does not support the video tag.


Because the component library is open source, you can easily port the repo to your preferred agentic design platforms as well.


## Get started today


Extend UI is available as open source under the MIT license. Get started below:


Resources:


- Extend UI:[https://www.extend.ai/ui](https://www.extend.ai/ui)
- GitHub:[extend-hq/ui](https://github.com/extend-hq/ui)
