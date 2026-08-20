---
schema_version: "1.0.0"
document_id: "89830e654aeff40eb45c2a190ed2ac02caf4125d3cf88bbb79d879c3221ae592"
company_key: "yc-virtualmin"
company: "Virtualmin"
source_id: "yc-virtualmin-rss-1bbb5effb21e"
canonical_url: "https://forum.virtualmin.com/t/virtualmin-virtual-server-module-8-0-1-released/136549"
published_at: "2026-02-06T09:44:37+00:00"
first_seen_at: "2026-07-28T18:40:24.886262+00:00"
fetched_at: "2026-07-28T22:21:24.537254+00:00"
content_hash: "sha256:6fbf1b3e5ed6e84448a71cc73ed014c1fc00c0e2c016b655f5c679097552d004"
---

# Virtualmin virtual-server module 8.0.1 released

Repository-wise, this was a breaking change because after switching from the monolithic Webmin package to modular, an upgrade (like GPL → Pro) or a manual repo re-setup could end up installing only the Webmin core package, without the extra modules that used to come with the full Webmin package.


This update fixes that by detecting which Webmin modules were enabled before the switch and then installing the matching modular Webmin packages, so the same functionality remains available after the repository change.


The only known downside is that we don’t currently require the` webmin-at` package (should be installed manually), which the Virtualmin Support module uses. We can bring this dependency back later, once more users have upgraded to Virtualmin 8.0.1.


Other than that, upgrading to the new repository should be safe, and we’re not aware of any other issues at this time.
