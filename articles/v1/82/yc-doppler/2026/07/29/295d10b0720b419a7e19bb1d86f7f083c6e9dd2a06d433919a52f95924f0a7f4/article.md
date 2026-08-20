---
schema_version: "1.0.0"
document_id: "295d10b0720b419a7e19bb1d86f7f083c6e9dd2a06d433919a52f95924f0a7f4"
company_key: "yc-doppler"
company: "Doppler"
source_id: "yc-doppler-news-import-86fd2de2d678"
canonical_url: "https://www.doppler.com/blog/june-26-product-update"
published_at: null
first_seen_at: "2026-07-25T01:53:43.668103+00:00"
fetched_at: "2026-07-28T21:38:27.854613+00:00"
content_hash: "sha256:07ba6c89ade56d8fe0947eb3a56697594ed33ad3ab89ec3434e42d6492db7285"
---

# June '26 product updates

Terraform workflows just got an upgrade. This month, we're introducing OIDC authentication for Terraform authentication, a new resource for managing secret notes as code, and a collection of improvements across the platform to make managing secrets even smoother.


## New and improved Terraform


Terraform just got even more capable. You can now manage secret notes as code with the new doppler_secret_note


resource, and authenticate to Doppler using OIDC.


[Learn More](https://registry.terraform.io/providers/DopplerHQ/doppler/latest/docs)


## What else have we been up to?


- Team and project[members](https://docs.doppler.com/docs/workplace-team)


can now be filtered by their role on the dashboard.
- Added API and Terraform support for[Integration Access Scoping](https://docs.doppler.com/docs/integration-access-scoping)


.
Added warnings when deleting inherited or referenced data, so users see downstream impact before confirming.
- Added fuzzy matching to the "view all secrets" search when searching by secret name.
- Added list capability to the rotated secrets API endpoint, returning per-secret data (including slugs) for use in other API operations.
- Increased the timeout duration for[Railway](https://docs.doppler.com/docs/railway)


syncs.
- Renamed the "Disconnect" action in the Integration menu to "Remove", with a clearer warning on integration delete.
- Added the ability to redact all historical versions of a secret by name or secret ID, so deleted secret history can be fully scrubbed.
- Added two additional reserved variables for the Heroku integration based on Heroku's updated[dyno metadata docs](https://devcenter.heroku.com/articles/dyno-metadata)


.
- Added search to the External References table (by secret name or reference name) and increased default page size from 6 to 10 across many dashboard tables.
