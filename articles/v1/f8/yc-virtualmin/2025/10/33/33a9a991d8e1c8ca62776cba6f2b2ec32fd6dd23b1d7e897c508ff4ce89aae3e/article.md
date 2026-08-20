---
schema_version: "1.0.0"
document_id: "33a9a991d8e1c8ca62776cba6f2b2ec32fd6dd23b1d7e897c508ff4ce89aae3e"
company_key: "yc-virtualmin"
company: "Virtualmin"
source_id: "yc-virtualmin-rss-1bbb5effb21e"
canonical_url: "https://forum.virtualmin.com/t/virtualmin-virtual-server-7-50-0-released/135550"
published_at: "2025-10-18T14:26:30+00:00"
first_seen_at: "2026-07-28T18:40:24.886262+00:00"
fetched_at: "2026-07-28T20:55:41.466450+00:00"
content_hash: "sha256:83047591ef964cd3efdf1d58e1646c7fbae98a5932b2f4598b23bc96456c9c8f"
---

# Virtualmin virtual-server 7.50.0 released

# [Virtualmin virtual-server 7.50.0 released](https://forum.virtualmin.com/t/virtualmin-virtual-server-7-50-0-released/135550)


[News](https://forum.virtualmin.com/c/news/5)


[Joe](https://forum.virtualmin.com/u/Joe)


October 18, 2025, 2:26pm 1


Howdy all,


We’ve rolled out Virtualmin virtual-server module version 7.50.0.


Changes since 7.40.1:


- Add support for Bunny DNS for Virtualmin Pro users
- Add improvements to external IPv4 and IPv6 address detection
- Add improvements and simplifications to the post-installation wizard
- Add pure-Perl implementation for retrieving SSL certificate information
- Fix to significantly improve support for IPv6 across different services
- Fix Apache and Dovecot config issues when restoring the backup
- Fix to stop breaking Apache config if hostname SSL request fails during Virtualmin installation
- Fix not to smoosh DNS TXT records together when using CLI[#1104](https://github.com/virtualmin/virtualmin-gpl/issues/1104)
- Fix to disallow out-of-domain DNS records when using CLI
- Fix to correctly add IPv6 to SSL virtual hosts
- Fix incorrect logic when checking IPv4 and IPv6 addresses in the config check
- Fix mailbox cleanup to correctly handle messages moved between folders, like to trash or spam
- Fix missing POP port in mail auto-config that caused some email clients to fail automatic configuration
- Fix to properly use the global Webmin notification email address for alerts
- Fix to hide` localhost` DNS record unless explicitly enabled
- Fix to completely remove the obsolete` m` DNS record


As always, if you run into any problems or have questions, open a *new topic* .


Cheers,
Joe


3 Likes


[DNS "m." and "localhost." records are not completely dropped in Virtualmin 7.50.0](https://forum.virtualmin.com/t/dns-m-and-localhost-records-are-not-completely-dropped-in-virtualmin-7-50-0/135559)


[shoulders](https://forum.virtualmin.com/u/shoulders)


October 18, 2025, 2:52pm 2


Very technical


[Ilia](https://forum.virtualmin.com/u/Ilia) Split this topic


October 19, 2025, 12:45pm 3


A post was split to a new topic:[DNS “m.” and “localhost.” records are not completely dropped in Virtualmin 7.50.0](https://forum.virtualmin.com/t/dns-m-and-localhost-records-are-not-completely-dropped-in-virtualmin-7-50-0/135559)


[system](https://forum.virtualmin.com/u/system) Closed


October 29, 2025, 12:45pm 4


This topic was automatically closed 10 days after the last reply. New replies are no longer allowed.
