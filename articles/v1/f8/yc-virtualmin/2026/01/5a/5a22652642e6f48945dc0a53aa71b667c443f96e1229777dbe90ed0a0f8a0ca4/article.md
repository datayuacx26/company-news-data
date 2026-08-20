---
schema_version: "1.0.0"
document_id: "5a22652642e6f48945dc0a53aa71b667c443f96e1229777dbe90ed0a0f8a0ca4"
company_key: "yc-virtualmin"
company: "Virtualmin"
source_id: "yc-virtualmin-rss-1bbb5effb21e"
canonical_url: "https://forum.virtualmin.com/t/virtualmin-8-released/136398"
published_at: "2026-01-26T07:41:52+00:00"
first_seen_at: "2026-07-28T18:40:24.886262+00:00"
fetched_at: "2026-07-28T20:54:41.720106+00:00"
content_hash: "sha256:e591242c950d30bc1573ccc3108d6ba145ca5d79b6c27916ccc4513a44f7e044"
---

# Virtualmin 8 released

Howdy all,


We’ve rolled out version 8 of the Virtualmin installer, along with several other updates to support the new version of the Virtualmin virtual-server 8.0.0 module.


A few of the big new features that have been introduced since the 7.0 release back in July of 2022.


Changes in Pro


- [WP Workbench](https://www.virtualmin.com/docs/plugins/wp-workbench/) , a plugin for centralized management and automation of many WordPress installations
- Major cloud provider enhancements and additions for managing DNS and backups in the cloud, including several new DNS providers and support for most S3-compatible storage providers, as well as managing Cloudflare DNS and proxy features
- systemd/cgroups resource limits for users (still in development, but usable, and a great improvement over the previous` limits.conf` resource limits support)
- Jailkit enhancements to ease management and configuration of jails
- Add support for multiple ACME-compatible SSL providers, including ZeroSSL, Sectigo and many others


Changes in GPL and Pro


- Support for the latest versions of our supported Linux distributions (EL 10 and Debian 13, and we expect Ubuntu 26.04 to be an easy lift when it is released)
- Modern integrated password recovery (no` virtualmin-password-recovery` module needed, plain text passwords not required)
- WebSockets-based terminal to provide a real interactive terminal in Webmin and Virtualmin
- Many CLI enhancements and additions including` --json` flag for JSON output
- Many new translations, and expansion of existing translations
- Many new features for managing PHP packages, configuration, and versions
- Hundreds of bugfixes and minor enhancements and additions


This is also a major refactoring of our software repo infrastructure, which may still have issues that we haven’t shaken out.


As always, if you run into any problems let us know in a *new* topic.


Cheers,
Joe
