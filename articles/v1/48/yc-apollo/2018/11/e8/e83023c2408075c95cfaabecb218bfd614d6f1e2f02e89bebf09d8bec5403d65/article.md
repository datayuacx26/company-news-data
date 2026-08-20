---
schema_version: "1.0.0"
document_id: "e83023c2408075c95cfaabecb218bfd614d6f1e2f02e89bebf09d8bec5403d65"
company_key: "yc-apollo"
company: "Apollo"
source_id: "yc-apollo-rss-acdf707d284a"
canonical_url: "https://www.apollographql.com/blog/security-notice-for-apollo-vs-code-11-28-18-aafa643765b6"
published_at: "2018-11-28T22:22:00+00:00"
first_seen_at: "2026-07-25T01:09:11.312803+00:00"
fetched_at: "2026-07-28T21:06:15.160827+00:00"
content_hash: "sha256:43f86c7540d7ede97640bdf73544e5533fe2fceb35163426f739cdb2bdb10a57"
---

# Security notice for Apollo VS Code 11/28/18

### tldr;


- A wide-spread,[industry-wide security vulnerability](https://github.com/dominictarr/event-stream/issues/116) impacted a dependency of a dependency of the Apollo VS Code plugin called` event-stream` .
- The editor extension (along with 38 others)[was removed from the VS Code Marketplace](https://code.visualstudio.com/blogs/2018/11/26/event-stream) . These extensions were also uninstalled for users and flagged as “malicious” within VS Code.
- We locked our extension to a safe version of the dependency and worked with the VS Code team to republish the Apollo package which is now safely back on the marketplace for download


## Timeline of events


### Monday November 26


On Monday morning news broke of a security vulnerability that impacted the JavaScript ecosystem at large. A popular dependency called` event-stream` was discovered to have been compromised. The package, when installed alongside a bitcoin wallet tool called` copay` or` copay-dash` , would attempt to siphon and steal bitcoins from users.


We determined that the` vscode` package that we use to build the Apollo VS Code editor extension was installing` event-stream` . We locked our versions down to a[previous safe version](https://github.com/apollographql/apollo-tooling/pull/739) and uploaded a release to the VS Code Marketplace.


### Tuesday November 27


We received reports of the editor extension being removed from the marketplace and flagged as malicious. Our team reached out to the VS Code team to ask what was happening and why were flagged.


The prior night, the[VS Code team removed 38 extensions](https://code.visualstudio.com/blogs/2018/11/26/event-stream) that depended on the` vscode` or other related projects that brought the compromised package into builds. After receiving our message on Tuesday, they responded to our team letting us know they were reviewing our new build.


### Wednesday November 28


The VS Code team let us know that our changes were sufficient and that the Apollo VS Code extension was published back onto the marketplace.


## Next steps


Due to the way VS Code extensions are installed, there is only a small chance that the vulnerability would have had any impact, however it is worth checking your machine to make sure that version of the package doesn’t exist.[Lauren Elizabeth Tan](https://medium.com/u/a4c91dcedec4?source=post_page-----aafa643765b6----------------------) put together a great tweet thread of steps to take:


don't forget to also check your local machine (for the backdoor,[https://t.co/9pjp2pYSwx](https://t.co/9pjp2pYSwx) , ICYMI) find / -type d -name "event-stream" -print 2>/dev/null


— lauren 나은 (@potetotes)[November 26, 2018](https://twitter.com/potetotes/status/1067147512065085442?ref_src=twsrc%5Etfw)


More specifically, search for \`flatmap-stream\`, as that is the malicious package: find / -type d -name "flatmap-stream" -print 2>/dev/null Thanks[@OCombe](https://twitter.com/OCombe?ref_src=twsrc%5Etfw) for pointing this out


— lauren 나은 (@potetotes)[November 26, 2018](https://twitter.com/potetotes/status/1067154651345113094?ref_src=twsrc%5Etfw)


✨ And finally you should be using \`npm ci\` OR \`yarn install –frozen-lockfile\` to install your deps on your servers: npm:[https://t.co/mXBK0KvAyB](https://t.co/mXBK0KvAyB) yarn:[https://t.co/mBcrpm9m3M](https://t.co/mBcrpm9m3M)


— lauren 나은 (@potetotes)[November 26, 2018](https://twitter.com/potetotes/status/1067202460593180673?ref_src=twsrc%5Etfw)


Thanks for your continued support using the Apollo platform. We take your security seriously and will always responsibly disclose any security vulnerabilities in our tools. To report a security vulnerability, please contactsecurity@apollographql.com .


Written by


James Baxley III


[Read more by James Baxley III](https://www.apollographql.com/blog/author/jbaxleyiii)
