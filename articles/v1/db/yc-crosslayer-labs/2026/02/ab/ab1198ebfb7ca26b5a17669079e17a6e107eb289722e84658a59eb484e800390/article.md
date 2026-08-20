---
schema_version: "1.0.0"
document_id: "ab1198ebfb7ca26b5a17669079e17a6e107eb289722e84658a59eb484e800390"
company_key: "yc-crosslayer-labs"
company: "Crosslayer Labs"
source_id: "yc-crosslayer-labs-news-import-1ff9699339a9"
canonical_url: "https://www.crosslayerlabs.com/blog/the-future-of-domain-control-validation"
published_at: "2026-02-09T00:00:00+00:00"
first_seen_at: "2026-07-25T01:05:23.009021+00:00"
fetched_at: "2026-07-28T22:21:23.355424+00:00"
content_hash: "sha256:909892ca05deb9e861aee9b279cfc0adab9f7143261b032a8d179a9238bba5bf"
---

# The future of Domain Control Validation (a.k.a. Why you should use DNS persist today)

## Introduction


Disclaimer: The text discussed in this post is my personal opinion. Nothing discussed reflects upon the compliance or behavior of any of the entities involved. I am not making any statements of noncompliance by any CA. cloud provider, or browser. Also as an Interested Party at the CA/Browser Forum I interact regularly with several of the entities discussed in this post. Everything discussed is from public knowledge and not my personal interactions.


I recently came across[Certkit](https://www.certkit.io/) , and while I found it interesting in general (sort of a combination of crt.sh + SSLMate + cloud provider managed SSL), one particular section jumped out at me:


> Do I need a DNS API?


> No! We think giving systems DNS access is dangerous. One compromised credential and an attacker controls your entire domain. Instead, you manually point a CNAME record at us for _acme-challenge and we handle the validation responses. It's a one-time setup, your DNS credentials stay with you, and the worst we could ever do is mess up your certificate challenges. That's a much smaller blast radius.


At first glance this makes sense. Not giving away DNS access means your domain is safe. But I think this approach creates a hidden vulnerability that I have actually been working to prevent for some time now. Understanding what's happening here can shed some light onto the future of Domain Control Validation.


## CNAME Magic


The approach used by Certkit is not new and is a common approach already employed by many cloud providers (see[Fastly](https://www.fastly.com/documentation/guides/getting-started/domains/securing-domains/setting-up-tls-with-certificates-fastly-manages/) ). Essentially the ACME protocol performs domain control verification at the _acme-challenge.<your domain> label. The ACME RFC permits ACME CAs to follow CNAMEs (and so does the CA/Browser Forum) so if you CNAME _acme-challenge.<your domain> to a 3rd party that 3rd party can get certificates for your domain. Even though you only set the CNAME up once, the 3rd party can pass the online DNS TXT record change requested by the CA by uploading the required TXT record to the CNAME target.


In some ways this is truly magical. It removes the need to share live DNS credentials with an ACME client. Less DNS credentials means less likelihood of a DNS breach. Problem solved, right????


## Where CNAME Magic is going


So people like CNAME magic. It's just an easier/cleaner way to manage issuance. Also, given the nature of the ACME DNS DCV method, there is really nothing to stop a cloud provider or subscriber from using it.


People liked CNAME magic so much that CAs themselves wanted to use it. But here is where there is a catch. Per the CA/Browser Forum guidelines a CA needs to directly interact with an Applicant and have the subscriber make the intended DNS record change required to complete the challenge. If a CA instead of telling a subscriber "make this one-time change to get your certificate" has a subscriber put in a CNAME record so the CA can continue to perform pseudo DCV with itself, it's sort of not really what the original DNS domain control validation asked for.


But some CAs wanted to use this.... So the CA/Browser Forum assembled a team to look into how CAs might be able to do CNAME magic themselves. What ultimately came out was CA/Browser Forum Ballot SC-082 which went to voting. **WHERE IT FAILED** .


Who didn't like this? well the browsers (this is not an opinion, the ballot literally failed due to lack of certificate consumer support). This put CNAME magic in a strange middle ground where you can do it if you are not a CA but if you are a CA, you sort of can't do it. Thus there becomes a need for more middlemen (like certkit) that perform CNAME magic for customers and interact with CAs who can't perform CNAME magic. The other trick in the book is what really is a CA. If a cloud provider owns an LLC and the LLC is the audited entity that runs the certificate operation, can the CA LLC perform CNAME magic with the cloud provider (I will leave this unanswered, but I do permit the reader to **google** a bunch of CAs that are owned by cloud providers).


So CNAME magic lives on in limbo.


This is sort of where I got involved. I didn't like that SC-082 failed. I get why the browsers blocked it, but it didn't seem to make sense in the grand scheme of things. I sort of imagined a CA launching some spinoff org to be a CA helper to run CNAME magic which could just be a separate business entity than the original CA.


## There is no limit to how much magic you can do... (although there probably should be)


This train of thought also got me thinking, CNAME magic works because it's a redirect protocol. With the static CNAME redirect in place, the dynamic DNS change can happen at a 3rd-party domain. What other protocols used by the CA/Browser Forum support redirects?? Turns out a lot of them, including HTTP (the OG redirect).


DCV via HTTP happens in the .well-known subdir of a webpage (BTW, new pastime, walk up to random sysadmins and ask how many know about the well-known directory). If you use a 302 redirect to send queries to this directory (and subfolders) to a 3rd party, similar to the DNS trick that 3rd party now gains the authority to request certificates on behalf of the domain in question. One of my favorite late-night hobby projects was throwing together what I think of as the ultimate manifestation of this redirect madness: a library where, after putting in the redirect, you can get a cert via a **single** REST API query:[https://github.com/birgelee/passive-dcv](https://github.com/birgelee/passive-dcv) . To me this is like certkit on steroids. No registration, no DNS changes, literally one 302 redirect rule and then access to unlimited certs via a single rest API query (it authenticates your query against the redirect target you use to ensure only the person who put in the original redirect can get the cert).


To the best of my knowledge no one uses this for production. And that's probably a good thing. Putting in such a redirect hands the keys to your kingdom to whoever runs this passive DCV service. One breach there, and people can get certs for your domain.


**But this is no different than CNAME magic** , the real problem with CNAME magic is that it introduces a middleman, a centralized service, where one compromise of a 3rd party could lead to malicious certificates for thousands or millions of domains. The truth is DCV is best run as a protocol between exactly 2 parties: the CA and the subscriber. As is commonly the problem with cert resellers (which are not WebTrust or ETSI audited), key material or DCV challenges being handled by another party is an inherent risk.


## What's the future?


I remember Aaron at Let's Encrypt discussing how a possible way forward would be to just do a new DCV method and remove the need to have the whole DNS change thing. I really liked this idea and spoke out in support. Michael Slaughter (Amazon Trust Services and author of the original Ballot SC-082) wrote up a proposal for an SC-082 redux that proposed the new approach now known as persistent DCV.


Persistent DCV eliminates the middleman. It lets a CA have a subscriber make one change to a DNS record with a static value and then just has the CA check that static value. No 3rd party, no changing records at CNAME domains, no live DNS keys, no competitive advantage from CA cloud affiliations. Just one clean DNS change and then certs forever. It's like CNAME magic convenience without CNAME magic infrastructure.


In (what I think is an uncommon move) I wrote up a whitepaper on the security considerations of the DCV approach and shared it with the[CA/Browser Forum in official Forum discussions](https://groups.google.com/a/groups.cabforum.org/g/servercert-wg/c/4tATbCpQRpM/m/8mH-SX87BAAJ?utm_medium=email&utm_source=footer) . I feel it's somewhat uncommon for a security researcher/interested party to come out in favor of a new way to get certs, but I strongly felt that putting this into the BRs will make the ecosystem stronger, and I wanted to be supportive of it. Michael Slaughter's Ballot passed so today CAs can perform DCV using a persistent DNS record.


I also joined Michael Slaughter and an implementor at Fastly (Shiloh Heurich) to write up the[Internet Draft for this method](https://github.com/ietf-wg-acme/draft-ietf-acme-dns-persist) which is now adopted at the IETF.


I strongly think this is the future because the security implications are much cleaner than the CNAME magic approach. It brings DCV back to just a 2-party protocol: the CA and the subscriber while still providing improved usability. Certkit discusses the need for better cert management now that cert lifetimes are getting shorter, but I think DNS persist is a much better long term solution to get to than CNAME magic.


So moral of the story, switch to DNS persist DCV if you are using DNS DCV and need to improve cert management. Also, be critical of 3rd parties that get between you and your CA.


## The future future?????


If DNS persist DCV is the future, there is a future to the future, and we need to look to Cloudflare to see what it is.


First, let's talk about the one loose end associated with DNS persist DCV that has (sort of) not been solved. The one downside of DNS persist DCV (as it is commonly envisioned today) is that subscribers need to put their ACME account URI into DNS. This is not inherently bad (RFC 8657 already has people do that) but it does lead to a level of infrastructure discovery and ownership correlations that is not currently available in the certificate ecosystem today. If org X has one ACME account and does DNS persist validation from domain D and domain E, one can deduce org X owns D and E given they both use the same ACME account URI. Better yet, assume domain D is a sensitive domain with no vulnerabilities but domain E is a low-priority domain that is poorly maintained. Potentially an adversary can realize that by breaking into domain E it can gain access to the infrastructure behind domain D. This is bad. There is an easy answer which is to use some derived or alternate agreed upon ACME account URI instead of the true URI so that one ACME account can be authorized with multiple different URIs. Many people are already thinking of this and I think **obfuscated account URIs are a fundamental part of using DNS persist validation. Without them in place, DNS persist validation introduces a discovery angle for adversaries** . However I see this as largely procedural. The language in the DNS persist ballot as well as our Internet draft permits this. It's more a matter of CAs deciding how they want to implement it.


But can we take the idea of reducing infrastructure exposure from DNS persist DCV a bit further? One interesting angle is that **DNS persist DCV (even with non-linkable account URIs) still leaks which CA(s) are being used for the domain** . This has one downside of letting adversaries know who your backup CAs are (although this is likely a small consideration).


However there is an operational side to this as well which we can understand by looking at one of the largest delegated cert managers in the world: Cloudflare. Cloudflare uses not one but like 5 different CAs. They circulate through them for failover, volume, and (my personal hunch) to maintain negotiating power with each of the CAs and not become dependent (remember when Amazon sort of round robbined their delivery companies?). But how does this flexibility look with DNS persist? There need to be a bunch of records authorizing these CAs even for them to be rarely used.


But what if a unique record did not need to be made for each CA. We started to explore this in our "security"[CAA tag work](https://datatracker.ietf.org/doc/draft-ietf-lamps-caa-security/) . Without going too much into the details there is a method where a subscriber can have a private key and then put the corresponding public key into DNS. Regardless of which CA the subscriber approaches, the subscriber shows control of the private key and then can get a certificate for the domain in question (after also doing normal DCV since we didn't **yet** get this through the CAB/F). The private key functions as a form of subscriber identity that exists across different CAs. If this became a true DCV method, each domain would have one (or more) authorized keys. Subscribers would just take that key to any trusted CA and get a cert with unlimited runtime CA failover. In fact your ACME client could just be configured with a whole bunch of CAs and you might never need to contact them before requesting a cert (well, you might have to have a way to pay them, but that's what crypto is for #acmecoin). Signatures by these keys authorizing certificate issuance could even be put into CT so CAs could have accountability.


I personally think this is the end game. And it comes full circle, keys for keys. TBH the web PKI should have probably just been baked into DNS. Like a DNS lookup should be cryptographic and have a key and an IP (long live DANE!). We don't live in that world and I don't think we will. But having authorized keys be the backbone of domain control validation I think will be the future.


## Conclusion


For now though, I think the best response by the ecosystem to shortening certificate lifetimes is not unnecessary middlemen in the issuance chain, it's better automation (e.g., ACME + DNS persist). Enterprises should look to reduce middlemen and maintain direct relationships with CAs. In the future, I think DCV should be based on keys that prove subscriber identity across different CAs. In addition to improved DCV, I think web PKI operations should always be coupled with extensive monitoring (certificate transparency and other feeds) to provide defense in depth and catch broader vulnerabilities like CA misbehavior.
