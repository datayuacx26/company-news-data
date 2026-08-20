---
schema_version: "1.0.0"
document_id: "7f05be37be49cc841db7e836078e557b69205868e8a3e330891390ed5c97275a"
company_key: "yc-enode"
company: "Enode"
source_id: "yc-enode-news-import-98d1bcb59046"
canonical_url: "https://enode.com/blog/product-updates/high-resolution-vehicle-images-now-in-enode-api"
published_at: "2026-02-27T00:00:00+00:00"
first_seen_at: "2026-07-21T18:15:11.081738+00:00"
fetched_at: "2026-07-28T22:03:18.293552+00:00"
content_hash: "sha256:2ceb433e64e65b520249f20b8835a340d44974aad6bc003bc964479e1ab8ec86"
---

# High-resolution vehicle images now in Enode API | Enode

For many people, a car is more than just transportation. They research thoroughly before buying, choosing one that reflects who they are and what they value. Car brands understand this: open any of their apps as an owner, and your vehicle is displayed proudly and prominently.


We share this sentiment. Starting today, the Enode API includes an imageUrl for all vehicle objects. Every supported model comes with a studio-quality image, ready to display in your app.


## Consistent by design


All images share a uniform, professional aesthetic: side profile, studio lighting, and a black finish, fitting for any app or interface. We offer one image per model, independent of individual user configurations.


## Built for scale


Images are served through a global CDN with flexible transformation options:


- **Custom sizing:** Request any width from 1 to 2048 pixels
- **Modern formats:** PNG, WebP, and AVIF
- **Quality control:** Fine-tune compression for your performance requirements


A single URL with query parameters gets you exactly what you’re after: https://cdn.images.enode.com/assets/{id}.webp?width=400&quality=85


## Available now


The imageUrl field is now available for all supported vehicle models. It’s included in the GET /vehicles/{id} response and in webhook payloads, and it doesn’t require any migration or incur additional cost.


## Looking ahead


We started with studio-rendered representations, and have since begun delivering personalized, real-world vehicle imagery based on end-user configurations — including color and rims. Expect this level of detail to expand across more makes and models going forward.


→[API Reference](https://developers.enode.com/api/reference#getVehicle)
