---
schema_version: "1.0.0"
document_id: "9410fbcc3b8c02b51ebf9ddfdb6740ab64a80e80cce2a71db347e7b7f5590c34"
company_key: "yc-virtualmin"
company: "Virtualmin"
source_id: "yc-virtualmin-rss-1bbb5effb21e"
canonical_url: "https://forum.virtualmin.com/t/webmin-2-652-and-usermin-2-551-released/137619"
published_at: "2026-07-21T21:12:09+00:00"
first_seen_at: "2026-07-28T18:40:24.886262+00:00"
fetched_at: "2026-07-28T20:34:24.680558+00:00"
content_hash: "sha256:d21f5bfd3c8b6fcaddfdd83aff6c30660a5c71dc4dd93a94379f2be94317393b"
---

# Webmin 2.652 and Usermin 2.551 released

# [Webmin 2.652 and Usermin 2.551 released](https://forum.virtualmin.com/t/webmin-2-652-and-usermin-2-551-released/137619)


[News](https://forum.virtualmin.com/c/news/5)


[Ilia](https://forum.virtualmin.com/u/Ilia)


July 21, 2026, 9:12pm 1


Howdy all,


We’ve rolled out Webmin 2.652 and Usermin 2.551 to all Virtualmin repos.


Changes since Webmin 2.651 and Usermin 2.550:


- Add a global per-user ACL control to block URL downloads from non-public IP addresses in File Manager, Mailboxes, and Upload and Download modules
- Fix to recognize hex numeric HTML entities to work in various elements
- Fix` patch` sub-command to reload Webmin instead of restarting, making it possible to run from Terminal module
- Fix SSL certificate and TCP monitors to report transient connection failures as down, and SSL check timeouts as timed out, rather than uninstalled
- Fix local file imports to enforce file access ACLs in Users and Groups, LDAP Users, MySQL/MariaDB, and PostgreSQL modules
- Fix Webmin user switching and session checks to find sessions stored with HMAC session keys
- Fix Usermin user switching to use one-time login URLs instead of the legacy cookie handoff and service restart flow
- Fix APT package architecture suffix handling to avoid false package update failure reports
- Fix missing Maildir folders to be counted as empty in Mailboxes module
- Fix Postfix version comparisons to handle version strings safely
- Fix SELinux labeling for Webmin and Usermin runtime data
- Update the Authentic theme to the latest version with various improvements:


- Fix inconsistent gaps around rounded UI elements
- Fix CPU usage values exceeding 100% in the dashboard
- Fix File Manager remote downloads to respect download address restrictions
- Fix spacing in the login page welcome message


6 Likes
