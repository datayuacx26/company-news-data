---
schema_version: "1.0.0"
document_id: "27dd158a38cf7a48750a91320c2057633bf657270ff404033fc10af2e69be94a"
company_key: "gitlab-inc-class-a-common-stock"
company: "GitLab Inc."
source_id: "gitlab-inc-class-a-common-stock-atom-8616b2ef668b"
canonical_url: "https://about.gitlab.com/blog/improved-scope-offset-fingerprinting/"
published_at: "2026-08-12T00:00:00+00:00"
first_seen_at: "2026-08-12T14:43:23.938208+00:00"
fetched_at: "2026-08-12T14:43:25.847625+00:00"
content_hash: "sha256:c03a1052db84046d55c148559f31ab5818d551cc22737efe29293764366539f4"
---

# How GitLab tracks vulnerabilities through refactors and reformatting

Every day, security scans face the same problem: an agent or a developer adds a comment, reformats a file, or moves a function, and a naive vulnerability tracker suddenly reports the same finding twice. Security teams end up re-triaging issues they already dismissed, which causes futile auditing effort and erodes trust in the scan results.


In 2022, we[introduced advanced vulnerability tracking](https://about.gitlab.com/blog/introducing-gitlab-advanced-vulnerability-tracking/) to tackle exactly this problem of *code volatility* . It is based on our *Scope+Offset* fingerprinting method: instead of identifying a finding by file and line number, we identify it by its narrowest enclosing *scope* (module, class, function) plus its line *offset* within that scope. That made tracking robust against code moving around the file and reduced futile re-auditing by about 30% compared to line-based tracking.


But one class of edits still slipped through: **non-functional changes** . The offset counted every line between the scope boundary and the finding, including comments and blank lines. Add a comment above a vulnerable statement, and the offset shifts. The tracker sees a "new" vulnerability; you see a duplicate.


Our improved method addresses this by simply ignoring non-functional code (comments and blank lines) when computing the fingerprint. Since these lines do not affect the program's behavior, they should not affect the identity of a vulnerability either. With this normalization in place, adding a comment or reformatting a file no longer changes the fingerprint, while the precision of the tracking remains the same as before. The details of the approach are described in our accompanying research paper.


We evaluated the normalized method on a targeted benchmark: 439 source files across C/C++, C#, Go, Java, JavaScript, Python, and Ruby. We generated 2,247 commits, each inserting a single comment or blank line directly before a known vulnerability, and scanned the code as the history was replayed. The benchmark deliberately stresses the worst case: every commit is a non-functional edit right next to a finding.


On this benchmark, the original *Scope+Offset* method accumulated 1,361 duplicate fingerprints, a 77% growth over the baseline. The normalized method produced zero duplicates and reduced unique fingerprints by 43% overall.


*Normalized Scope+Offset* ships in GitLab as the` scope_offset_compressed` tracking algorithm, supporting C#, C/C++, Go, Java, JavaScript, Python, Ruby, and PHP. It reuses the parse tree the scanner already constructs, so scan times are unaffected. The security report format is unchanged, so it composes with any combination of SAST tools in a heterogeneous setup.


The preprint of our study "[Vulnerability Tracking using Normalized Scope+Offset](https://res.cloudinary.com/about-gitlab-com/image/upload/v1786457681/jm8oieyjhwut8ed2cpaq.pdf) " by Julian Thome, Hua Yan, Lucas Charles, Craig Smith, and Jason Leasure will be presented at the[ASE 2026 Industry Showcase](https://conf.researchr.org/track/ase-2026/ase-2026-industry-showcase) .


*[Hua Yan](https://gitlab.com/hyan3) ,[Lucas Charles](https://gitlab.com/theoretick) ,[Craig Smith](https://gitlab.com/craigmsmith) , and Jason Leasure contributed to this article and study.*


##


Are you trading speed for security?


[Get your security maturity score](https://about.gitlab.com/assessments/security-modernization-assessment/)


Quiz will take 5 minutes or less
