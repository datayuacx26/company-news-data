---
schema_version: "1.0.0"
document_id: "64592cc6b5854d0864e01b6f559059b4cf79c347706b0ca638e3f632c27e9693"
company_key: "yc-ncompass-technologies"
company: "nCompass Technologies"
source_id: "yc-ncompass-technologies-rss-225d7ec730f1"
canonical_url: "https://community.ncompass.tech/t/vscode-extension-attempted-nested-profiling-removes-existing-profile/22"
published_at: "2025-12-01T16:31:26+00:00"
first_seen_at: "2026-07-25T15:45:43.528659+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:94c0be26f7d4d46bec78d741015d38f208206fd23240b2e1301f229f18c551f3"
---

# VSCode Extension: Attempted nested profiling removes existing profile

# [VSCode Extension: Attempted nested profiling removes existing profile](https://community.ncompass.tech/t/vscode-extension-attempted-nested-profiling-removes-existing-profile/22)


[Bug Reports](https://community.ncompass.tech/c/bug-reports/5)


[DiederikVink](https://community.ncompass.tech/u/DiederikVink)


1 December 2025 16:31 1


When selecting a region for profiling that entirely encapsulates an existing region, it will always remove first region. The same happens when the attempting to create a region inside an existing region, which removes the original region.
