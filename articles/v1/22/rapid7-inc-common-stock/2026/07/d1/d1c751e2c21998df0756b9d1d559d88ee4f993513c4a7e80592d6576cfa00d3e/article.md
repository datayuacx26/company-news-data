---
schema_version: "1.0.0"
document_id: "d1c751e2c21998df0756b9d1d559d88ee4f993513c4a7e80592d6576cfa00d3e"
company_key: "rapid7-inc-common-stock"
company: "Rapid7 Inc."
source_id: "rapid7-inc-common-stock-rss-ea5a9037191f"
canonical_url: "https://www.rapid7.com/blog/post/etr-kindarails2shell-cve-2026-66066-critical-arbitrary-file-read-and-possible-remote-code-execution-in-ruby-on-rails"
published_at: "2026-07-30T16:11:10+00:00"
first_seen_at: "2026-07-31T17:52:26.401864+00:00"
fetched_at: "2026-07-31T17:52:28.126602+00:00"
content_hash: "sha256:5f75b89acc0a7481e7a5b038f78825a91d15f54e9d134f8798efd931f1174224"
---

# KindaRails2Shell: CVE-2026-66066, Critical Arbitrary File Read and Possible Remote Code Execution in Ruby on Rails

## Overview


On July 29, 2026, the Ruby on Rails project


[published a security advisory](https://github.com/rails/rails/security/advisories/GHSA-xr9x-r78c-5hrm) for


[CVE-2026-66066](https://www.cve.org/CVERecord?id=CVE-2026-66066) , a critical vulnerability affecting Active Storage image processing when used in conjunction with the libvips image processing library. The vulnerability has a CVSSv4 score of


[9.5](https://www.first.org/cvss/calculator/4.0#CVSS:4.0/AV:N/AC:L/AT:P/PR:N/UI:N/VC:H/VI:H/VA:H/SC:H/SI:H/SA:H) and is classified as Initialization of a Resource with an Insecure Default (


[CWE-1188](https://cwe.mitre.org/data/definitions/1188.html) ). An unauthenticated attacker may be able to leverage CVE-2026-66066 and read files accessible to the Rails application process, potentially exposing secrets that could enable remote code execution (RCE) or access to connected systems.


An application is affected when it uses libvips for Active Storage image processing and accepts image uploads from untrusted users. Rails notes that generating image variants is not a separate requirement for exposure. Vips is the default Active Storage variant processor for applications configured with Rails


7.0


or later defaults. According to


[Ethiack](https://ethiack.com/info-hub/research/kindarails2shell-rails-rce-cve-2026-66066) , only the Vips processor is affected; applications using Magick are not affected through the reported vector.


As of July 30, 2026, Rapid7 is not aware of exploitation in the wild. Ethiack and GMO Flatt Security, who independently reported the vulnerability, have withheld proof-of-concept code and details of the full attack chain. Public code claiming to exploit CVE-2026-66066 exists, but it is unclear how closely it corresponds to the full attack chain reported privately to Rails. According to the


[Rails Security Announcement](https://discuss.rubyonrails.org/t/cve-2026-66066-possible-arbitrary-file-read-and-remote-code-execution-in-active-storage-variant-processing/91432) , additional details will be disclosed no later than August 28, 2026. Rapid7 recommends remediating affected applications on an urgent basis, outside of normal patch cycles.


**Update #1**


: On July 31, 2026, Rails


[published technical details and forensic tools](https://discuss.rubyonrails.org/t/cve-2026-66066-attack-details-and-tools-to-perform-a-forensic-investigation/91441) earlier than its planned August 28 disclosure date after several researchers reverse-engineered the attack and published proof-of-concept code.


## Technical overview


libvips uses operations to load and save image formats, including operations backed by third-party libraries. Some are marked "unfuzzed" or "untrusted" because they are unsafe for untrusted content. According to Rails, Active Storage did not disable these operations before processing user-supplied files, which may allow a crafted upload to trigger an unsafe operation and disclose files readable by the application.


The


[attack details published by Rails](https://github.com/rails/rails-forensics-CVE-2026-66066/blob/main/reference/the-attack.md) describe a chain in which an attacker creates a blob through Active Storage's direct-upload endpoint with a false image content type and obtains a genuine signed


variation_key


from a page that renders an Active Storage representation. A crafted file identifies itself to libvips as a MATLAB level 5 file but to libmatio as a MAT 7.3 HDF5 container. HDF5's External File List then reads bytes from an attacker-selected path, which are rendered as image pixels and returned in the resulting variant. This known chain also requires the deployed libvips build to include the


matload


operation.


For this documented chain, the Active Storage direct-upload route must be reachable. When Active Storage routes are mounted, the direct-upload route is present by default even if the application's own interface does not use direct uploads. Rapid7 testing found that ordinary server-side attachment does not satisfy this chain because Rails re-identifies the crafted file as MATLAB data before variant processing.


The arbitrary file-read stage does not require knowledge of


secret_key_base


or a forged variation key. Rapid7 also


[verified](https://github.com/rapid7/metasploit-framework/pull/21733) an RCE escalation in which recovered Rails signing material is used to forge an ImageProcessing 1.x variation; this path does not require Marshal deserialization.


The


[Rails patch](https://github.com/rails/rails/commit/349e7a5d5b4b715af1e416db824f3c078a7d59e5) that remediates CVE-2026-66066, disables untrusted operations during Active Storage initialization. When ruby-vips is installed, patched versions prevent the application from starting if ruby-vips or libvips is too old to support that protection.


## Mitigation guidance


Organizations running affected Ruby on Rails applications should upgrade to a fixed Active Storage release and ensure libvips is


8.13


or later. Updating Rails or Active Storage alone is not sufficient when an older libvips version is installed.


Rails has published


[forensic tools](https://github.com/rails/rails-forensics-CVE-2026-66066) to assess whether an application was vulnerable and search Active Storage data for crafted files. Because scheduled cleanup of unattached blobs may remove evidence, Rapid7 recommends beginning forensic assessment promptly.


The Rails advisory identifies patched Active Storage releases


7.2.3.2


,


8.0.5.1


, and


8.1.3.1


. The fixed Rails releases are:


**Rails branch**


**Affected versions**


**Fixed version**


Rails


7.x


7.0.0


through


7.2.3.1


7.2.3.2


Rails


8.0.x


8.0.0


through


8.0.5


8.0.5.1


Rails


8.1.x


8.1.0


through


8.1.3


8.1.3.1


The Rails advisory lists all Active Storage releases earlier than


7.2.3.2


as affected, which includes releases before Rails


7.0


. Ethiack reports that Rails


6.0.0


through


6.1.7.10


may be affected when Active Storage is configured to use Vips, and Rapid7 has


[verified](https://github.com/rapid7/metasploit-framework/pull/21733) that the known attack works on the Rails


6.0


and


6.1


branches under that non-default configuration. Rails has not published fixed releases for branches earlier than


7.2


, so affected applications on those branches should migrate to a supported fixed branch or apply the applicable workaround below.


When ruby-vips is installed, organizations should ensure it is


2.2.1


or later. Rails advises affected organizations to replace


secret_key_base


and other secrets accessible to the application process, including the Rails master key and the credentials it decrypts, storage service credentials, database credentials, and third-party service tokens or keys. Replacing


secret_key_base


expires active sessions and affects encrypted and signed cookies, signed global IDs, and Active Storage URLs.


As a temporary workaround on libvips


8.13


or later, organizations can set


VIPS_BLOCK_UNTRUSTED


or, with ruby-vips


2.2.1


or later, call


Vips.block_untrusted(true)


from an initializer. For libvips versions earlier than


8.13


, Rails states that the only workaround is to remove the libvips dependency.


For the latest mitigation guidance, please refer to the


[Ruby on Rails security advisory](https://github.com/rails/rails/security/advisories/GHSA-xr9x-r78c-5hrm) .


## Rapid7 customers


### Exposure Command, InsightVM, and Nexpose


Exposure Command, InsightVM, and Nexpose customers can assess exposure to CVE-2026-66066 with vulnerability checks expected to be available in the July 31 content release.


## Updates


-


**July 30, 2026**


: Initial publication.


-


**July 31, 2026**


: Updated with technical details and forensic resources published by Rails, and clarified the affected version range.
