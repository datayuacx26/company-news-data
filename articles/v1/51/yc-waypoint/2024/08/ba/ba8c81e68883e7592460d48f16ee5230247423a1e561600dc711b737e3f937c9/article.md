---
schema_version: "1.0.0"
document_id: "ba8c81e68883e7592460d48f16ee5230247423a1e561600dc711b737e3f937c9"
company_key: "yc-waypoint"
company: "Waypoint"
source_id: "yc-waypoint-rss-69637758f48b"
canonical_url: "https://www.usewaypoint.com/changelog/dynamic-colors-on-templates/"
published_at: "2024-08-21T00:00:00+00:00"
first_seen_at: "2026-07-26T05:13:45.651737+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:00b7e1edbdbf140f8d0cc94ef55147b9d48cb06f6ca18a00be0fdc563f967d21"
---

# Dynamic Colors on Templates

Waypoint’s new dynamic colors feature allow teams to theme email templates based on data. This is especially useful for vertical SaaS platforms (eg. software for yoga studios) that want to apply their customer’s branding and colors on email templates instead of their own.[View an example/tutorial](https://www.usewaypoint.com/docs/template-color-variables#tutorial) .


At the heart of Waypoint’s[template builder](https://www.usewaypoint.com/docs/template-basics) is the use of[data variables](https://www.usewaypoint.com/docs/liquidjs-templating) to apply the dynamic elements of a template. While these variables are most often used for dynamic text and images, they can now be used to set dynamic colors on a template or[layout](https://www.usewaypoint.com/docs/template-layouts) .


From a template or layout builder, simply click on the` {}` icon next to a color within the global ‘Styles’ tab to set a theme color override based on data. Example:` {{company.brandColor}}` .
