---
schema_version: "1.0.0"
document_id: "121bcfa0ee6460b36801cff726ee38000d514a1947bc03856f4d452c46f37b18"
company_key: "yc-zeropath"
company: "ZeroPath"
source_id: "yc-zeropath-news-import-51f20a74ff70"
canonical_url: "https://zeropath.com/blog/how-zeropath-won-over-curl-with-170-valid-bugs"
published_at: "2025-10-21T00:00:00+00:00"
first_seen_at: "2026-07-24T06:51:45.842756+00:00"
fetched_at: "2026-07-28T21:27:39.672880+00:00"
content_hash: "sha256:02226e3800170bb2a27891ee34e36f06e8806f266da9e15319c4ad3e72e3ef97"
---

# How ZeroPath's AI Code Scanner Won Over the curl Project with 170 Valid Bug Reports

## Finding curl bugs with ZeroPath


Practically synonymous with HTTP (and other) content retrieval, curl powers an estimated 20+ billion devices around the world. With that success has come a wave of low-quality, AI-generated bug reports that overwhelm unpaid maintainers. Quite famously, curl’s Daniel Stenberg has written about these “beg-bounty” submissions overwhelming unpaid volunteers with AI-generated claims that do not match reality, draining their time and energy, and discouraging contributors to stay involved.


ZeroPath's AI-native source code scanner is now flipping the script, and in the last month has helped find *and fix* nearly 170 issues in curl, and it's still going. With a stamp of approval from the maintainers of curl, Daniel has publicly stated that he is "[almost blown away by the quality of some of \[the findings from ZeroPath\]](https://mastodon.social/@bagder/115241314074965388) ", with some "[actually truly awesome findings](https://mastodon.social/@bagder/115241241075258997) ". These issues ranged from traditional issues associated with C codebases all the way to esoteric issues which materialized from *business logic* flaws; the latter of which ZeroPath really differentiates itself in comparison to other source code scanners, due to it truly *understanding* the codebase.


Even though ZeroPath's scanner is a Static Analyzer analyzing *static source code* , its methodology and results are anything but static. Our scanner is not limited to pre-written fixed sets of rules or queries that only try to find old problems with new technology; results are effectively dynamic and source code is treated holistically with developer *intent* in mind -- all with the backing of the logical reasoning capabilities of[our custom AI agents](https://zeropath.com/blog/how-zeropath-works) . Our scanner is therefore able to not only find traditional vulnerabilities and bugs, but also identify completely novel issues, beyond what has been traditionally possible with static code alone. Not only that, ZeroPath's scanner takes every *potential* issue, and independently verifies it before reporting to users -- massively reducing false positives while surfacing logical flaws too, without compromising high quality results, allowing AppSec engineers and developers to spend more time working on difficult problems rather than spam.


Finding that many bugs in curl is no small feat. curl has billions of users, an active bounty, and many layers of tooling and review. As Daniel[wrote on his blog](https://daniel.haxx.se/blog/2025/10/10/a-new-breed-of-analyzers/) , " *in the curl project we continuously run compilers with maximum pickiness enabled and we throw scan-build, clang-tidy, CodeSonar, Coverity, CodeQL and OSS-Fuzz at it and we always address and fix every warning and complaint they report so it was a little surprising that this tool now suddenly could produce over two hundred new potential problems. But it sure did. And it was only the beginning* ".


So how did this all happen? Security researcher[Joshua Rogers](https://joshua.hu/) recently began using the ZeroPath tool to scan the curl codebase for bugs and vulnerabilities, using the results to open[20 pull requests](https://github.com/curl/curl/issues?q=author%3Amegamansec) . From here, he gave the curl maintainers access to the raw ZeroPath output, and they used this to produce around[130 more patches](https://github.com/curl/curl/pulls?q=zeropath+OR+joshua) -- and counting.


## Interesting Findings


With around 170 discovered and fixed, it's impossible to go through all of the findings in this post. However, we thought it would be interesting to list some of the more fascinating findings, which no other scanner had found in curl before.


### Developer Intent vs. Code


#### rustls


In curl's rustls integration, ZeroPath discovered that if a file size was a multiple of 256 bytes, curl would[error out](https://github.com/curl/curl/commit/e9455ea523d91c8f6f29c3ae1f37500f0b637204) .


#### SASL


In curl's SASL implementation, ZeroPath discovered that an incorrect bit manipulation failed to disable certain SASL mechanisms if they were advertised by the server but not supported by curl. In this case, our scanner noticed the developer intent from the[comment in the code](https://github.com/curl/curl/pull/18573/files) , which was in disagreement with the actual code written:


```text
/* Remove the offending mechanism from the supported list */
sasl  ->  authmechs   ^=   sasl  ->  authused  ;
```


The above code, which ZeroPath flagged, does the *opposite* of removing the offending mechanism from the supported list.


#### HTTP/3


In curl's HTTP/3 integration, the idle timeout checking code was incorrectly using *nanoseconds* to calculate whether a connection[has timed out](https://github.com/curl/curl/pull/18903/files) . In addition to that, it also worked out that the documented feature of an optional "no-timeout" was completely ignored if used.


#### SSH/SFTP


ZeroPath worked out that if a connection was resumed while uploading a file over ssh/sftp, the file would be[truncated](https://github.com/curl/curl/pull/18952/files) with no error reported, meaning an incomplete file would be uploaded.


### RFC Violations


This is where ZeroPath is really able to differentiate itself from traditional code scanners -- or actually, even human code reviewers. Our system can match developer *intent* with the reality of the code written, all while performing analysis on implementations of RFCs, checking and ensuring proper compliance to appropriate RFCs.


#### Telnet


In curl's Telnet implementation, ZeroPath discovered that subnegotiation payloads were written without escaping IAC (0xFF) symbols. If any user-controlled value contained 0xFF, it could be parsed as a command midstream and break the negotiation. The fix here was to just[refuse any IAC symbols](https://github.com/curl/curl/pull/18657) , as there's little legitimate real-world usage of this symbol.


Another bug in telnet that was discovered related to[an error](https://github.com/curl/curl/pull/18887/files) occurring in which case an error message was logged, but ... the program continued anyway (and not returning the error).


#### Dead Kerberos Code


ZeroPath discovered a buffer overflow in[Kerberos FTP handling](https://hackerone.com/reports/3341476) . However, it also correctly identified that the whole Kerberos code was broken, and that if anybody in the past year or so had actually used curl with Kerberos FTP, they would have been completely unable to. In the end, the solution to this was to completely[remove Kerberos FTP](https://daniel.haxx.se/blog/2025/09/19/bye-bye-kerberos-ftp/) in curl. Being able to identify these problems with ZeroPath allowed curl to do what some security teams could dream of: reducing attack surface *by having a valid reason to completely delete code* , reducing overall risk exposure to users.


#### TFTP


The TFTP RFC states that the client must stick to the server's first-chosen UDP port for the whole transfer, and any other packets from other ports should be discarded. In curl's TFTP client, ZeroPath[discovered](https://github.com/curl/curl/pull/18658/files) that packets were not validated against the initially chosen server port, which allowed an on-path or same-network attacker to inject a valid looking DATA or OACK and hijack the transfer.


#### SMTP


The RFC for SMTP states that certain keywords in the exchange between server and client must be treated as case-insensitive. ZeroPath discovered that the parsing of these keywords was[case sensitive](https://github.com/curl/curl/pull/18589/files) . This could lead to situations where encryption would not be used for communication, when an SMTP server responded with a lowercase keyword.


#### IMAP


Just like SMTP, the RFC for IMAP states that certain keywords in the exchange between server and client must be treated as case-insensitive. ZeroPath discovered that the parsing of these keywords was[case sensitive](https://github.com/curl/curl/pull/19090/files) . This could lead to situations where encryption would not be used for communication, when an IMAP server responded with a lowercase keyword.


### Documentation vs. Reality


Like the mismatch of developer intent and real code, documentation can also be found to be either outdated or just flat-out incorrect, resulting in broken code with broken contracts, resulting in bugs and vulnerabilities when documentation is followed by developers. In this bug, our system correctly identified that the documentation for the function` Curl_resolv` stated that the function parameter` entry` may be NULL. In reality, if it were NULL, a null pointer dereference could occur in certain circumstances.


```text
# `Curl_resolv`: NULL out-parameter dereference of `*entry`


* **Evidence:** `lib/hostip.c`. API promise: "returns a pointer to the entry in the `entry` argument (**if one is provided**)." However, code contains unconditional writes: `*entry = dns;` or `*entry = NULL;`.
* **Rationale:** The API allows `entry == NULL`, but the implementation dereferences it on every exit path, causing an immediate crash if a caller passes `NULL`.


```


### Memory Leaks


ZeroPath discovered dozens of real memory leaks in the curl codebase, by tracking every allocation of memory (and other resources like file descriptors), and checking whether in all cases, the allocations were correctly freed (and not overwritten and effectively lost). In a few cases too, it[discovered](https://github.com/curl/curl/issues/18587) issues where the incorrect functions were used to free allocated resources, such as mixing` malloc` with` FreeContextBuffer` . Similar issues were found elsewhere, such as in[socks_sspi](https://github.com/curl/curl/pull/19046/files) .


Imagine a codebase which integrates curl as a library (libcurl). As memory or other resources are leaked over time, eventually, the whole program is either going to crash, or get killed by the system's out-of-memory-killer.


### Other


In the case of an error in handling a certificate revocation, a variable containing an error message was set[after](https://github.com/curl/curl/pull/18642/files) the error message was actually used.


In a test program, an unimplemented[program flag](https://github.com/curl/curl/pull/19026/files) was sitting in the output of` --help` , with no associated code.


## Conclusion


None of the above findings would ever be detected by a traditional source code scanners, as they are *logic* flaws; not run-of-the-mill issues that can be found by simple pattern matching. Understanding developer intent, business logic, and impact; all while reasoning about code no different than a highly experienced human would (with the will-power to actually go off and read every RFC applicable to the codebase at hand), is what sets ZeroPath apart from the old "tried-and-true" source code scanners which have missed all of the issues ZeroPath has discovered in curl. Likewise, unlike human source code reviewers, ZeroPath does not discriminate from the areas of the source code that it reviews; old code, new code, neglected code, or dead code -- it is able to search for issues in whole codebases.


ZeroPath will continue working with the curl project, as well as other critical software codebases, to enhance the resilience and security of their codebases in the future. Be sure to keep up to date with our work!


If you're interested in seeing ZeroPath in action, you can[book a demo](https://zeropath.com/demo) of our tool, and see just how easy it is to start finding real bugs at the click of a button.
