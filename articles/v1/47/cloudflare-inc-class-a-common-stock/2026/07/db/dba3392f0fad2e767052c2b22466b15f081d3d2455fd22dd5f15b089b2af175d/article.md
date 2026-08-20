---
schema_version: "1.0.0"
document_id: "dba3392f0fad2e767052c2b22466b15f081d3d2455fd22dd5f15b089b2af175d"
company_key: "cloudflare-inc-class-a-common-stock"
company: "Cloudflare Inc."
source_id: "cloudflare-inc-class-a-common-stock-rss-72eb55cabac1"
canonical_url: "https://blog.cloudflare.com/dnssec-nta-ede-33/"
published_at: "2026-07-14T13:00:00+00:00"
first_seen_at: "2026-07-20T03:32:14.182345+00:00"
fetched_at: "2026-07-28T21:50:17.978782+00:00"
content_hash: "sha256:190bd1eae2532d73c1c071e5bdee198c2c27f15898d4bfe1fd634401e5da8bfc"
---

# A broken DNSSEC rollover took down .al. Now 1.1.1.1 tells you when validation is bypassed

On July 3, 2026, the Albanian communications authority (AKEP), the operator of the` .al` country-code top-level domain (TLD) of Albania, attempted a DNSSEC key rollover. Something went wrong, resulting in DNSSEC validation failures. Any validating DNS resolver receiving these signatures was required by the DNSSEC specification to reject them and return errors to clients. That includes[1.1.1.1](https://www.cloudflare.com/learning/dns/what-is-1.1.1.1/) , the public DNS resolver operated by Cloudflare.


The` .al` TLD is the online home of Albanian government services, banks, and media; it ranks[#191 on Cloudflare Radar's TLD ranking](https://radar.cloudflare.com/tlds/al?dateRange=7d) . Anyone trying to visit those sites, using a validating resolver, found them unreachable during the incident. The failure had the potential to affect every` .al` domain, regardless of where it was hosted or which authoritative nameservers served it.


Just two months earlier, a similar incident struck` .de` , the TLD of Germany. As we described in[our blog post on the incident](https://blog.cloudflare.com/de-tld-outage-dnssec/) , our response was to install a Negative Trust Anchor (NTA) for` .de` , temporarily suspending DNSSEC validation in 1.1.1.1 to keep domains reachable while the registry resolved the issue. We did the same for .` al` .


NTAs restore resolution, but silently. A client receiving a response served under an NTA has no way to tell, from the response alone, that DNSSEC validation was bypassed, leaving it unable to distinguish a legitimate answer from a spoofed one. For the` .al` incident, 1.1.1.1 addressed that gap for the first time, returning a new Extended DNS Error (EDE) code alongside every affected response to signal that the answer was not DNSSEC-validated due to the presence of an NTA.


The graph below shows the SERVFAIL and NOERROR rates for` .al` queries on 1.1.1.1 throughout July 3. The SERVFAIL rate climbs as cached records expire and resolvers are forced to revalidate. It drops sharply when the NTA is applied at 17:15 UTC, restoring resolution.


### What happened to` .al`


We discussed how DNSSEC works in more detail in[our prior blog post](https://blog.cloudflare.com/de-tld-outage-dnssec/#how-dnssec-works) . A brief recap:


DNSSEC builds a chain of trust from the root zone down to individual domain names. The root zone holds a Delegation Signer (DS) record for each signed TLD, a fingerprint of that TLD's DNSKEY. A resolver verifying` .al` checks that the DNSKEY served by` .al` 's nameservers matches the DS record in the root. If it does, the resolver trusts that DNS responses from` .al` 's nameservers are authentic. The same pattern repeats one level down:` .al` holds DS records for its signed child zones, each with a matching DNSKEY. A break anywhere in that chain, such as a DS record pointing to a key that no longer exists, causes validation to fail for everything below it.


Before the incident, the root zone held a DS record matching the DNSKEY served by the` .al` nameservers, as illustrated below.


At around 14:15 UTC, the` .al` operator published a new DNSKEY and stopped serving the old one. The DS record in the root zone still pointed to the old DNSKEY (id=26319), so any resolver attempting to validate` .al` responses found no matching key and failed.


At roughly 17:00 UTC, the` .al` operator removed the new DNSKEY without restoring the old one. The zone now had no DNSKEY records at all, while the DS record in the root still pointed to id=26319, and resolution continued to fail.


At roughly 19:15 UTC, the` .al` operator removed the DS record from the root zone. Without a DS record, resolvers no longer expected DNSSEC validation for` .al` , and resolution was restored, though the entire TLD was now unsigned.


As of publishing,` .al` remains unsigned. The DS record has not been restored to the root zone by the` .al` operators. Without a DS record, every` .al` domain is unable to use DNSSEC protections.


### Why Negative Trust Anchors are used


Having a broken DNSSEC configuration can be painful, especially when it impacts an entire TLD at once. As we covered in our` .de`[incident blog](https://blog.cloudflare.com/de-tld-outage-dnssec/#negative-trust-anchors) , recursive DNS operators can install a Negative Trust Anchor (NTA) as defined in[RFC 7646](https://datatracker.ietf.org/doc/html/rfc7646) , which tells a resolver to treat a zone as unsigned and bypass validation. Before installing the NTA, we attempted to reach the` .al` operator directly and posted on the[DNS-OARC Mattermost](https://www.dns-oarc.net/oarc/services/chat) to alert the community. We received no response, in part because the operator's contact addresses were themselves under` .al` , making them unreachable during the outage.


We applied the NTA for` .al` and rolled it out to all 1.1.1.1 users by 17:15 UTC, roughly three hours after the chain broke.


The tradeoff is the same as it was for` .de` : a Negative Trust Anchor suspends DNSSEC validation, which means` .al` domains were no longer protected against DNS spoofing for the duration. We judged this acceptable for the same reason: the failure was public, confirmed, and affecting every validating resolver equally.


The Negative Trust Anchor was removed the following day, once the` .al` operator had removed the DS record from the root zone. With no DS record present, resolvers no longer expected DNSSEC for` .al` and the NTA was no longer needed.


### The problem with Negative Trust Anchors


Installing a Negative Trust Anchor is an aggressive measure. We suspend DNSSEC validation to keep domains reachable, accepting that responses are no longer cryptographically verified for the duration. Users get answers instead of SERVFAIL, but those answers carry no DNSSEC guarantee.


What makes this harder is that, up until now, nothing in the DNS response signalled this to the client; a response served under an NTA looked identical to a fully validated one. RFC 7646 acknowledges this gap and recommends that operators publicly disclose which NTAs they have in place, but that disclosure is out-of-band. For both the` .de` and` .al` incidents we published status pages, but a status page requires the user to go looking. An application, a monitoring tool, or a user querying 1.1.1.1 had no way to tell, from the response alone, that DNSSEC validation was bypassed.


### Bringing transparency to Negative Trust Anchors


Extended DNS Error (EDE) codes, defined in[RFC 8914](https://datatracker.ietf.org/doc/html/rfc8914) , allow resolvers to include additional context alongside any DNS response, whether that is an error or a successful answer. Babak Farrokhi at Quad9 proposed an Internet-Draft to signal the presence of a Negative Trust Anchor directly in the DNS response, using a new EDE code:[Disclosure of Negative Trust Anchors in DNS Responses](https://datatracker.ietf.org/doc/draft-farrokhi-dnsop-ede-nta/) . We joined as co-authors, and 1.1.1.1 now implements it.


During the` .al` incident, any query for a` .al` name returned both the answer and the new EDE code while the Negative Trust Anchor was installed. Here is what that looked like:


```text
$ kdig @1.1.1.1 google.al
;; ->>HEADER<<- opcode: QUERY; status: NOERROR; id: 32848
;; Flags: qr rd ra; QUERY: 1; ANSWER: 1; AUTHORITY: 0; ADDITIONAL: 1


;; EDNS PSEUDOSECTION:
;; Version: 0; flags: ; UDP size: 1232 B; ext-rcode: NOERROR
;; EDE: 9 (DNSKEY Missing): 'no SEP matching the DS found for al.'
;; EDE: 33 (Negative Trust Anchor): 'a Negative Trust Anchor has been applied for this query (see RFC 7646)'


;; ANSWER SECTION:
google.al.              300    IN    A    142.251.142.196
```


The response is a NOERROR with a valid answer:` google.al` resolves, but two EDE codes accompany it.` EDE 9 (DNSKEY Missing)` surfaces the underlying DNSSEC failure: the chain of trust was broken and validation failed.` EDE 33 (Negative Trust Anchor)` signals that 1.1.1.1 applied a Negative Trust Anchor and served the response anyway. Together they give clients and operators full visibility into what happened: the answer is real, but it was not DNSSEC-validated.


1.1.1.1 returns EDE 33 on any response generated while an NTA is active, regardless of whether the query itself would have failed DNSSEC validation. A query for a domain that does not use DNSSEC at all will still carry EDE 33 if it falls under an active NTA. This is intentional: the NTA covers the entire zone, and transparency applies equally to every response served under it. This also resolves an issue we flagged in our` .de` blog, where 1.1.1.1 incorrectly returned` EDE 22 (No Reachable Authority)` instead of surfacing the underlying DNSSEC error. During the` .al` incident, 1.1.1.1 correctly returned` EDE 9 (DNSKEY Missing)` alongside EDE 33.


The Internet-Draft is an individual submission and EDE 33 has been[assigned by the Internet Assigned Numbers Authority (IANA)](https://www.iana.org/assignments/dns-parameters/dns-parameters.xhtml#extended-dns-error-codes) . Thanks to our co-author, Babak Farrokhi at Quad9, the[kdig](https://www.knot-dns.cz/docs/latest/html/man_kdig.html) tool from the Knot project[now recognizes EDE 33 by name](https://github.com/CZ-NIC/knot/commit/1b053bcfe17eaa4f008d589d6ec0ea53145e22e4) , and[a pull request for Unbound](https://github.com/NLnetLabs/unbound/pull/1470) is under review. We hope other resolver implementations will follow. The Internet-Draft has been submitted to the[Internet Engineering Task Force (IETF) DNSOP Working Group](https://datatracker.ietf.org/wg/dnsop/about/) , and will be discussed in the DNSOP Working Group at the IETF meeting, taking place in Vienna from July 18 to July 24.


### Closing the gap


TLD-level DNSSEC failures are rare, but when they happen they affect every domain underneath the affected TLD simultaneously, and every validating resolver equally. The` .al` incident, following closely behind` .de` , shows that Negative Trust Anchors are a necessary operational tool, but one that has, until now, been invisible to the users they affect.


EDE 33 closes a gap that RFC 7646 left open. A response served under a Negative Trust Anchor now says so directly, giving operators, monitoring tools, and users the information they need to understand what the resolver did and why.


The Internet-Draft is available at the[IETF datatracker](https://datatracker.ietf.org/doc/draft-farrokhi-dnsop-ede-nta/) . If you have thoughts on it, the[IETF DNSOP mailing list](https://mailarchive.ietf.org/arch/browse/dnsop/) is the right place to share them.


If you want to learn more about how DNSSEC works, visit our page[How does DNSSEC work?](https://www.cloudflare.com/en-gb/learning/dns/dnssec/how-dnssec-works/) And you can always follow real-time DNS trends and TLD data on[Cloudflare Radar](https://radar.cloudflare.com/tlds/al?dateStart=2026-07-03&dateEnd=2026-07-03) .
