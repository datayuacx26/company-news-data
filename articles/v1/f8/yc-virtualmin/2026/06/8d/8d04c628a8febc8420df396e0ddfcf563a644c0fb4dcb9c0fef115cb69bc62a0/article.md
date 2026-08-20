---
schema_version: "1.0.0"
document_id: "8d04c628a8febc8420df396e0ddfcf563a644c0fb4dcb9c0fef115cb69bc62a0"
company_key: "yc-virtualmin"
company: "Virtualmin"
source_id: "yc-virtualmin-rss-1bbb5effb21e"
canonical_url: "https://forum.virtualmin.com/t/webmin-2-651-and-usermin-2-550-released/137489"
published_at: "2026-06-29T00:52:34+00:00"
first_seen_at: "2026-07-28T18:40:24.886262+00:00"
fetched_at: "2026-07-28T21:08:50.237722+00:00"
content_hash: "sha256:4678e6b657ba3011f1bc2be460ce6c529766772cf507cb2b7fad143206685eea"
---

# Webmin 2.651 and Usermin 2.550 released

# [Webmin 2.651 and Usermin 2.550 released](https://forum.virtualmin.com/t/webmin-2-651-and-usermin-2-550-released/137489)


[News](https://forum.virtualmin.com/c/news/5)


[Ilia](https://forum.virtualmin.com/u/Ilia)


June 29, 2026, 12:52am 1


Howdy all,


We’ve rolled out Webmin 2.651 and Usermin 2.550 to all Virtualmin repos.


Changes since Webmin 2.650:


- Fix Certbot-backed certificate requests and renewals to correctly parse PEM paths after issuance
- Fix live activation of Linux bond interfaces[#2777](https://github.com/webmin/webmin/pull/2777)
- Update the Authentic theme to the latest version with various improvements and fixes:


- Fix search-result all-items delete in File Manager
- Fix search-result delete ordering in File Manager
- Fix to speed up search-result deletion cleanup in File Manager


Changes since Webmin 2.641 and Usermin 2.540:


- Add new Systemd Services and Units module
- Add new GRUB 2 Boot Loader module
- Add new Kea DHCP Server module
- Add WebSocket proxy support to the Webmin Servers Index module
- Add basic Alpine Linux support
- Add IP-based Let’s Encrypt certificate support with Certbot 5.3
- Add editable SSH public keys for newly added Unix users in Users and Groups module
- Add improvements to custom Webmin temporary directory handling
- Add quick service and port forwarding controls to the nftables module
- Add optional pre- and post-scripts for scheduled package updates
- Add option to control when scheduled package update email is sent
- Add per-user RPC/API-only access option to the Webmin Users module
- Add Apache 2.4 MPM process limit directives[#1821](https://github.com/webmin/webmin/issues/1821)
- Add dhcpcd network backend for Debian and Raspberry Pi OS[#1607](https://github.com/webmin/webmin/issues/1607)
- Add hardware RAID passthrough devices config in the SMART Status module[#1704](https://github.com/webmin/webmin/issues/1704)
- Update Xterm.js to fix Control-C handling on iPadOS/Safari terminals
- Update Webmin systemd service unit to run without forking
- Fix IPv6 CIDR access control matching[#1570](https://github.com/webmin/webmin/issues/1570)
- Fix Bootup and Shutdown module to show only services and not all units on systemd systems
- Fix Let’s Encrypt renewal scheduling to count from the last successful request
- Fix NetworkManager detection on Debian and IPv6 DNS nameserver saving
- Fix Dovecot configuration file handling when saving extra configs
- Fix mailbox listing to skip unusable Maildir entries and remove stale deleted or moved entries
- Fix Postfix module labels to identify virtual alias maps instead of virtual mailbox domains #1541


- Fix Apache module to hide disabled default virtual hosts from the active server list
- Fix Netplan DNS saving to preserve YAML structure
- Fix BIND DNS handling of underscores, trailing dots, and mass record length checks
- Fix MariaDB user creation when using auth plugin syntax
- Fix PHP-FPM monitor on EL systems when using` /etc/php.ini` as the config file
- Fix RPC-only accounts to block browser/module access before module ACL checks
- Fix reflected XSS in Webmin status messages
- Fix path validation in File Manager, package delete helpers, and Apache virtual host files
- Fix authentication state handling for SSL certificate logins and proxied keep-alive requests
- Update session handling to improve security, which will require users to re-authenticate after upgrading


- Add zooming to stats history graphs by holding shift and scrolling in the dashboard
- Add support for saving live stats history for up to 24 hours without performance impact
- Add better support for the new Nginx, nftables, and upcoming systemd, Kea-DHCP, and GRUB 2 Webmin modules
- Add ability to always show available dashboard panels in theme configuration
- Add support for live stats and terminal WebSocket connections through Webmin Servers Index proxy links
- Fix proxying when Webmin is accessed with a webprefix using Webmin Servers Index module
- Fix theme UI helpers to escape generated markup more safely
- Fix iOS terminal viewport sizing
- Fix editor save handling, clean-state indication and dirty reload guard
- Fix popover positioning, z-index and border color for help bubbles
- Fix the active product switch border in the navigation menu for the dark palette
- Fix to validate password reset return URLs


4 Likes


[Ilia](https://forum.virtualmin.com/u/Ilia) Split this topic


June 29, 2026, 11:16am 2


6 posts were split to a new topic:[Usermin 2.550 login from the Virtualmin Edit Users page stopped working](https://forum.virtualmin.com/t/usermin-2-550-login-from-the-virtualmin-edit-users-page-stopped-working/137491)


[system](https://forum.virtualmin.com/u/system) Closed


July 9, 2026, 11:16am 3


This topic was automatically closed 10 days after the last reply. New replies are no longer allowed.
