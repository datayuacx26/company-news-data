---
schema_version: "1.0.0"
document_id: "6d07ae4128ebb5a46b081028dde5f952ae0822d009fe3c05fbea09977acd283f"
company_key: "yc-virtualmin"
company: "Virtualmin"
source_id: "yc-virtualmin-rss-1bbb5effb21e"
canonical_url: "https://forum.virtualmin.com/t/webmin-version-2-610-released/135889"
published_at: "2025-11-26T21:07:45+00:00"
first_seen_at: "2026-07-28T18:40:24.886262+00:00"
fetched_at: "2026-07-28T21:58:33.663804+00:00"
content_hash: "sha256:f1851afeb73ca040303dbc218ca8d7942b03a705ed87430cdfce86d39d0f1bbb"
---

# Webmin version 2.610 released

# [Webmin version 2.610 released](https://forum.virtualmin.com/t/webmin-version-2-610-released/135889)


[News](https://forum.virtualmin.com/c/news/5)


[Ilia](https://forum.virtualmin.com/u/Ilia)


November 26, 2025, 9:07pm 1


Howdy all,


We’ve rolled out Webmin version 2.610 to all Virtualmin repos.


Changes since 2.600:


- Fix to drop dependency on` IO::Pty` Perl module
- Fix` virtual-server` module server-side search to work correctly
- Update the Authentic theme to the latest version with various improvements and fixes:


- Add a range slider to adjust content page margins more precisely
- Add an option to enable rounded corners for content page
- Add more customization options for pie charts
- Fix to increase clickable area for checkboxes in File Manager
- Fix to correct rotation of pin and unpin button for right side slider
- Fix color of selected items in the multiselect dropdown
- Fix to improve the visibility of disabled checkboxes
- Fix to send saved params in the post body when saving theme configuration
[More details…](https://github.com/webmin/authentic-theme/releases/tag/26.20)


Changes since 2.520:


- Add an options to enable the slow query log in the MySQL/MariaDB module[#2560](https://github.com/webmin/webmin/issues/2560)
- Add ability to install multiple PHP extensions at once in the PHP Configuration module
- Add ability to show package URL in the Software Packages module[#1141](https://github.com/virtualmin/virtualmin-gpl/issues/1141)
- Add support to show Debian package install time in the Software Packages module
- Add support to show detailed Webmin server stats using new` webmin stats` CLI command[forum.virtualmin.com/t/135556](https://forum.virtualmin.com/t/is-this-memory-used-a-bit-high/135556/6)
- Add a major Authentic theme UI update with lots of visual and structural improvements for a smoother and more modern experience
[More details…](https://forum.virtualmin.com/t/authentic-theme-version-26-00-release-overview/135755)
- Fix EOL library fatal error for OS in development[#2121](https://github.com/webmin/webmin/issues/2121)
- Fix correctly saving jails with parameters containing quotes in the Fail2Ban module[#2572](https://github.com/webmin/webmin/issues/2572)
- Fix file is always renamed as the effective user in the Upload and Download module[#1054](https://github.com/webmin/webmin/issues/1054)


---


As always, if you run into any problems let us know in a new topic.


3 Likes


[system](https://forum.virtualmin.com/u/system) Closed


December 6, 2025, 9:08pm 2


This topic was automatically closed 10 days after the last reply. New replies are no longer allowed.
