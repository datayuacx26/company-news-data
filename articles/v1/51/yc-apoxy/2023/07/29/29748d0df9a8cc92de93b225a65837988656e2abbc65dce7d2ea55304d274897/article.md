---
schema_version: "1.0.0"
document_id: "29748d0df9a8cc92de93b225a65837988656e2abbc65dce7d2ea55304d274897"
company_key: "yc-apoxy"
company: "Apoxy"
source_id: "yc-apoxy-rss-ae7e2d86e063"
canonical_url: "https://apoxy.dev/blog/announcing-apoxy"
published_at: "2023-07-26T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:03.880414+00:00"
fetched_at: "2026-07-28T22:00:21.221344+00:00"
content_hash: "sha256:33bbee3c968a18ebd7b2a9c4e0aa01f2674052561dde6721113df9045b9807f5"
---

# Announcing Apoxy

Hello, World! – if only real code was ever actually that simple. In the real world, our software applications are covered in “glue code” that helps ensure the security, usability, and operations of the products we use everyday.


Throughout our years of operating software at scale working at companies like YouTube, Google, Cruise, and most recently Mux (where Dmitry and I met), we’ve seen teams move from code to config and back again. As teams scale and operational needs grow, they evaluate various API gateways and service meshes to reduce the complexity of their application. A lot of these products have configurations that get you going, but in our experience you end up wanting to do more, which requires code, and you end up right back at the point of writing it yourself in your application.


So teams end up writing a bunch of custom complex operational logic in their applications and eventually someone comes along and proposes pulling it back out via some common configuration… and so goes the unbundle and bundle cycle of software operations.


Apoxy is the proxy platform for improving the security, observability, and usability of APIs and services at scale. Our programmable proxy enables developers to focus on writing code for their applications' operational concerns, rather than dealing with extensive configurations.


We’re starting by building on Envoy, a hardened proxy that has been adopted by many of the largest companies on the planet like Netflix, Pinterest, Spotify, and Dropbox. The WebAssembly runtime in Envoy is a powerful platform that enables developers to extend Envoy in many different languages. However, we believe there’s some work to be done to make the developer experience of compiling the source code to WASM bytecode and deploying it on your infrastructure as easy as your Next.js app on Vercel. So stay tuned!


So if you’re building with Envoy, love WASM, or you just want the inside scoop on what’s next,[follow us on LinkedIn](https://www.linkedin.com/company/apoxy/) or send an email tohello@apoxy.dev if you’d like to say hi.


Matt Ward


Co-founder & CEO


Matt is co-founder and CEO of Apoxy. He previously scaled infrastructure for YouTube and Mux. Outside of work, you can find him skiing down or pedaling up a mountain if he's not at home trying to be the kind of husband and father his family deserves.


[GitHub ↗](https://github.com/therealwardo)[X ↗](https://x.com/therealwardo)


[← Back to all posts](https://apoxy.dev/blog)
