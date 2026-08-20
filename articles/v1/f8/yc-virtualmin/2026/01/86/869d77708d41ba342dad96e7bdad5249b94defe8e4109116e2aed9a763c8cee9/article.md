---
schema_version: "1.0.0"
document_id: "869d77708d41ba342dad96e7bdad5249b94defe8e4109116e2aed9a763c8cee9"
company_key: "yc-virtualmin"
company: "Virtualmin"
source_id: "yc-virtualmin-rss-1bbb5effb21e"
canonical_url: "https://forum.virtualmin.com/t/virtualmin-virtual-server-module-8-0-0-released/136390"
published_at: "2026-01-25T08:44:43+00:00"
first_seen_at: "2026-07-28T18:40:24.886262+00:00"
fetched_at: "2026-07-28T20:54:41.720106+00:00"
content_hash: "sha256:ab45dddda952d75503d28b09d4864da59b6c78d254d8271dabb3c61c27577ebc"
---

# Virtualmin virtual-server module 8.0.0 released

Howdy all,


We’ve rolled out Virtualmin virtual-server module 8.0.0 for all repos. Note this is *not* the Virtualmin 8 installer, which will be rolled out in a few hours.


The Virtualmin virtual-server module does not add support for any new distros to the current stable Virtualmin 7 installer, so the supported OS list has not changed and won’t change until the new installer is pushed.


Changes since 7.50.1:


- Add support for systemd resource limits for Virtualmin Pro users
- Add support for SFTP backups and restores, including the ability to purge SFTP backups
- Add support for paginated display of large user lists
- Add backup signing improvements, including the ability to skip signing when necessary
- Add option to forward the original HTTP hostname when proxying requests
- Add phpMyAdmin integration (if installed) when editing databases for virtual servers
- Add a row showing when and why a domain was disabled in the virtual server summary
- Add improvements to ACME service notifications
- Add reseller access to edit PHP-FPM configs
- Add improvements to handling of remote/cloud DNS hosting
- Fix validation of A and AAAA DNS records when using` modify-dns` CLI
- Fix reliability of remote backups during long-running tasks using Webmin RPC
- Fix several DKIM-related issues
- Fix handling of EC SSL certificates
- Update the repo setup script and workflow to match the newer packaging/CI layout


As always, if you run into any problems, let us know in a *new* topic.
