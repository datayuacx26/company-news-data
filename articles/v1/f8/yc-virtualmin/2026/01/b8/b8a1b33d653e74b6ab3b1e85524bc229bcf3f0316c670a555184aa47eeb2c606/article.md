---
schema_version: "1.0.0"
document_id: "b8a1b33d653e74b6ab3b1e85524bc229bcf3f0316c670a555184aa47eeb2c606"
company_key: "yc-virtualmin"
company: "Virtualmin"
source_id: "yc-virtualmin-rss-1bbb5effb21e"
canonical_url: "https://forum.virtualmin.com/t/webmin-2-621-and-usermin-2-521-released/136425"
published_at: "2026-01-27T17:08:23+00:00"
first_seen_at: "2026-07-28T18:40:24.886262+00:00"
fetched_at: "2026-07-28T20:54:41.720106+00:00"
content_hash: "sha256:3a92ef0722b7b3494e3e623de2bbc4a9d95a32a5c4a787f9b388953aa9db4571"
---

# Webmin 2.621 and Usermin 2.521 released

# [Webmin 2.621 and Usermin 2.521 released](https://forum.virtualmin.com/t/webmin-2-621-and-usermin-2-521-released/136425)


[News](https://forum.virtualmin.com/c/news/5)


[Ilia](https://forum.virtualmin.com/u/Ilia)


January 27, 2026, 5:08pm 1


Howdy all,


We’ve rolled out Webmin 2.621 and Usermin 2.521 to all Virtualmin repos.


Changes since 2.620 and 2.520:


- Fix to prevent NAT from dropping idle RPC sessions during long transfers
- Fix to improve the message when socket authentication is used in the MySQL/MariaDB module
- Fix to make upload tracking work correctly in all situations and on all systems
- Fix to correctly display the PHP version in the PHP Configuration module when managing packages
- Update Xterm.js to the latest version with lots of improvements and fixes
- Update Authentic theme to the latest version with various improvements and fixes:


- Fix the support for the cloned Terminal module
- Fix error handling for file uploads when the user is out of quota or the system is out of disk space in the File Manager module
- Fix to stop loading full file into memory for upload check to prevent memory leak on large uploads in the File Manager module
- Fix to permanently save the state of the navigation menu and right-side slider when toggled


4 Likes


[system](https://forum.virtualmin.com/u/system) Closed


February 6, 2026, 5:08pm 2


This topic was automatically closed 10 days after the last reply. New replies are no longer allowed.
