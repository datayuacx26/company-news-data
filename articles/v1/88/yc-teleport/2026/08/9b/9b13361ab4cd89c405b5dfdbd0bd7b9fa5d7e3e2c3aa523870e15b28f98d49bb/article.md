---
schema_version: "1.0.0"
document_id: "9b13361ab4cd89c405b5dfdbd0bd7b9fa5d7e3e2c3aa523870e15b28f98d49bb"
company_key: "yc-teleport"
company: "Teleport"
source_id: "yc-teleport-news-import-16bebfbed724"
canonical_url: "https://goteleport.com/blog/ncc-cryptography-audit-go-ssh/"
published_at: "2026-08-10T00:00:00+00:00"
first_seen_at: "2026-08-10T18:17:57.160881+00:00"
fetched_at: "2026-08-10T18:17:58.271876+00:00"
content_hash: "sha256:4d46e96d513e052365ab50c0f3cc299bd3c1bb16e1f4e305f820a8b36dea3a16"
---

# How Two Small Bugs Led to a Critical Vulnerability and a Cryptography Audit of Go’s SSH Library

Last summer we found a critical vulnerability in Teleport, our first in a decade. It was assigned CVE-2025-49825. This vulnerability allowed Teleport SSH certificates, issued to users of a cluster, to sign other SSH certificates which would then be incorrectly accepted by Teleport as valid.


Once you can sign your own certificates, you can escalate privileges and bypass authentication controls. This is why we classified it as a critical vulnerability.


The issue was the result of faulty logic in our code, which accepted an SSH certificate as a valid signer, where only CA keys should be accepted.


On its own, this error would not be exploitable. The relevant[SSH Certificate Format RFC Draft](https://datatracker.ietf.org/doc/draft-ietf-sshm-cert/) specifies that “Implementations MUST NOT accept certificate keys as CA keys.”


However it coincided with an issue in` x/crypto/ssh` that allowed` IsUserAuthority` and` IsHostAuthority` to be passed certificates as CA keys and became a critical severity vulnerability in Teleport.


This prompted us to review our own code for similar issues, and sponsor a deeper level of cryptographic analysis of the excellent` x/crypto/ssh` package which is used so heavily by our code.


In collaboration with Filippo Valsorda and his team at Geomys, we hired NCC Cryptography Services to conduct a thorough review of the package.


Today NCC Group is publishing the results of this engagement. A total of nine CVEs were issued based on these results:


- [CVE-2026-39828](https://nvd.nist.gov/vuln/detail/CVE-2026-39828)
- [CVE-2026-39829](https://nvd.nist.gov/vuln/detail/CVE-2026-39829)
- [CVE-2026-39830](https://nvd.nist.gov/vuln/detail/CVE-2026-39830)
- [CVE-2026-39831](https://nvd.nist.gov/vuln/detail/CVE-2026-39831)
- [CVE-2026-39832](https://nvd.nist.gov/vuln/detail/CVE-2026-39832)
- [CVE-2026-39833](https://nvd.nist.gov/vuln/detail/CVE-2026-39833)
- [CVE-2026-39834](https://nvd.nist.gov/vuln/detail/CVE-2026-39834)
- [CVE-2026-39835](https://nvd.nist.gov/vuln/detail/CVE-2026-39835)
- [CVE-2026-46598](https://nvd.nist.gov/vuln/detail/CVE-2026-46598)


What we learned from these findings is that the` x/crypto/ssh` is a well hardened package that is maintained to the highest degree of professional security engineering standards. These bugs are subtle and require an intense expert review to discover.


We’ve also learned that small, subtle bugs in well hardened software can cascade into critical vulnerabilities when used incorrectly.


We’d like to take this opportunity to thank the[NCC Group Cryptography Services](https://www.nccgroup.com/technical-assurance/cryptography-encryption/) team, who did an excellent job with the engagement.


We’d also like to thank[Geomys](https://geomys.org/) , the team maintaining the` x/crypto/ssh` library for their collaboration with NCC on the engagement, quick fixes to the discovered issues, and coordination with the Google team on disclosure.


You can find the public report on[NCC Group’s website](https://www.nccgroup.com/research/public-report-go-xcryptossh-cryptographic-implementation-review/) . A copy is also available on[Teleport’s Trust Center](https://trust.goteleport.com/resources?s=vtulvv0p9hp48gw22zvlp4&name=ncc-group-go-x/crypto/ssh-cryptographic-implementation-review) .
