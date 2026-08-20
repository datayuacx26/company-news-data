---
schema_version: "1.0.0"
document_id: "66e63e82270ee7cabfa3858fe87fb3500c6bba6474252368bb431e77bb1f1487"
company_key: "fastly-inc-class-a-common-stock"
company: "Fastly Inc."
source_id: "fastly-inc-class-a-common-stock-rss-83c7761b19d9"
canonical_url: "https://www.fastly.com/blog/what-is-cve-2026-66066-protecting-rails-app-active-storage-rce/"
published_at: "2026-08-03T00:00:00+00:00"
first_seen_at: "2026-08-03T17:52:48.143995+00:00"
fetched_at: "2026-08-03T18:22:19.433930+00:00"
content_hash: "sha256:3e9f868e22818ca024ac5a79e301baa17a3aa9fe9a9bcdae63f07cc45cc15cbc"
---

# What is CVE-2026-66066? Protecting Your Rails App from Active Storage RCE

*Navigating CVE-2026-66066: Threat details, affected components, and how to stay protected*


## CVE-2026-66066: What You Need to Know


-


On July 29, 2026, Rails released patches and[an advisory](https://github.com/rails/rails/security/advisories/GHSA-xr9x-r78c-5hrm) for CVE-2026-66066, an arbitrary file read vulnerability in Active Storage that can enable remote code execution (RCE)


-


Exploiting this vulnerability also requires the use of libvips < version 8.13


-


Fastly Next-Gen WAF customers can enable our[new virtual patch](https://www.fastly.com/documentation/reference/changes/2026/07/added-virtual-patch-for-cve-2026-66066-kindarails2shell/) (released July 31, 2026) to gain immediate protection against exploitation attempts while the underlying components are patched.


-


Affected components


-


Active Storage


-


< 7.2.3.2, 8.0 up to 8.0.5.1, 8.1 up to 8.1.3.1


-


Mitigating this vulnerability requires updating **both** Active Storage and libvips


-


Active Storage


-


>= 7.2.3.2, >= 8.0.5.1, >= 8.1.3.1


-


libvips


-


>= 8.13


## What are the impacts of CVE-2026-66066?


CVE-2026-66066 is an arbitrary file read vulnerability, meaning it enables an attacker to read arbitrary files (and process memory) from a vulnerable web server. If certain files are read, an attacker can use this to gain remote code execution on the vulnerable server. For more in-depth details on the exact cause of the vulnerability, see[Ethiack’s detailed research post](https://ethiack.com/info-hub/research/kindarails2shell-how-a-matlab-file-reads-your-secrets-and-pops-a-shell-on-ruby-on-rails) .


## What to do next: Virtual patching and remediation


While our virtual patch mitigates the vast majority of attack vectors, edge cases may exist depending on your specific environment and configuration. Applying the official patches to Active Storage and libvips as soon as possible remains the best way to ensure total protection.


To enable the virtual patch, follow the enablement instructions in our[official documentation](https://www.fastly.com/documentation/reference/changes/2026/07/added-virtual-patch-for-cve-2026-66066-kindarails2shell/) .


If you are concerned about potential exploitation, Rails has also[published details on how to perform forensics](https://discuss.rubyonrails.org/t/cve-2026-66066-attack-details-and-tools-to-perform-a-forensic-investigation/91441) for this vulnerability. This includes a[forensics repo](https://github.com/rails/rails-forensics-CVE-2026-66066) with Claude skills to assist in forensics.


## References


Rails advisory & technical details:


-


[https://github.com/rails/rails/security/advisories/GHSA-xr9x-r78c-5hrm](https://github.com/rails/rails/security/advisories/GHSA-xr9x-r78c-5hrm)


-


[https://discuss.rubyonrails.org/t/cve-2026-66066-attack-details-and-tools-to-perform-a-forensic-investigation/91441](https://discuss.rubyonrails.org/t/cve-2026-66066-attack-details-and-tools-to-perform-a-forensic-investigation/91441)


Research from Ethiack:


-


[https://ethiack.com/info-hub/research/kindarails2shell-rails-rce-cve-2026-66066](https://ethiack.com/info-hub/research/kindarails2shell-rails-rce-cve-2026-66066)


-


[https://ethiack.com/info-hub/research/kindarails2shell-how-a-matlab-file-reads-your-secrets-and-pops-a-shell-on-ruby-on-rails](https://ethiack.com/info-hub/research/kindarails2shell-how-a-matlab-file-reads-your-secrets-and-pops-a-shell-on-ruby-on-rails)
