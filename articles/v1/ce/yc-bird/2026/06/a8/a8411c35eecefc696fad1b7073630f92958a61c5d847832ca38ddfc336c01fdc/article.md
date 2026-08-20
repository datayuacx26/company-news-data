---
schema_version: "1.0.0"
document_id: "a8411c35eecefc696fad1b7073630f92958a61c5d847832ca38ddfc336c01fdc"
company_key: "yc-bird"
company: "Bird"
source_id: "yc-bird-news-import-1ae5a03d7866"
canonical_url: "https://bird.com/en-us/blog/spf-vs-dkim-vs-dmarc"
published_at: "2026-06-22T00:00:00+00:00"
first_seen_at: "2026-07-21T10:23:31.194838+00:00"
fetched_at: "2026-07-28T21:43:28.836467+00:00"
content_hash: "sha256:522904e269f3aa787839835ee581cb3b2f63867e96ce57a7d929e7251b205165"
---

# SPF vs DKIM vs DMARC: What's the Difference?

SPF, DKIM, and DMARC are three email authentication standards that solve different parts of the same problem: proving an email really comes from the domain it claims. SPF authorizes which servers can send for you, DKIM cryptographically signs your messages, and DMARC ties both back to the domain your recipients see while reporting on what it finds. You want all three.


## What does each one actually check?


Short version first, then the detail.


SPF DKIM DMARC


**Stands for** Sender Policy Framework DomainKeys Identified Mail Domain-based Message Authentication, Reporting & Conformance


**Checks** Is this server allowed to send for the domain? Was this message signed and unaltered? Does authentication align with the visible From domain?


**Mechanism** DNS list of authorized servers Cryptographic signature plus a public key in DNS Policy, alignment, and reporting published in DNS


**Protects** The Return-Path domain Message integrity and the signing domain The visible From domain (what users read)


**On its own** Breaks on forwarding Survives forwarding Needs SPF or DKIM underneath it


## How does SPF work?


SPF (Sender Policy Framework) is a DNS record listing the servers allowed to send mail for your domain. When a message arrives, the receiver checks the sending server's IP against that list. Pass means the server was authorized; fail means it wasn't. SPF's weak spot is forwarding: when mail is forwarded, the forwarding server isn't on your list, so SPF breaks even though the mail is legitimate. The deeper dive is in[what is an SPF record](https://bird.com/en-us/blog/what-is-an-spf-record) .


## How does DKIM work?


DKIM (DomainKeys Identified Mail) adds a cryptographic signature to each message's headers. Your server signs outgoing mail with a private key, and receivers verify it against a public key you publish in DNS. If the signature checks out, the message wasn't altered in transit and genuinely came from a domain holding the key. DKIM survives forwarding better than SPF, because the signature travels with the message rather than depending on the sending IP.


## How does DMARC tie them together?


Here's the gap SPF and DKIM leave open: both authenticate a domain, but not necessarily the one in the From address your recipient reads. SPF checks the Return-Path; DKIM checks the signing domain. A scammer can pass either for a domain they own while still displaying your brand in the From line.


DMARC fixes that with alignment. It requires the SPF or DKIM domain to match your visible From domain, and a message passes DMARC if either path passes and aligns. Then it adds reporting, so you see every source sending as you. The mechanics are in[how DMARC works](https://bird.com/en-us/blog/how-does-dmarc-work) , and the policy you publish is covered in[what is a DMARC policy](https://bird.com/en-us/blog/what-is-a-dmarc-policy) .


## Do you need all three?


Yes, and they're meant to stack. SPF and DKIM are the authentication; DMARC is the policy and visibility layer that makes them meaningful for the domain people actually see. Publishing DMARC without SPF and DKIM underneath gives receivers nothing to align against, so the order is SPF and DKIM first, then DMARC. That sequence is exactly what[how to set up DMARC](https://bird.com/en-us/blog/how-to-set-up-dmarc) walks through.


A way to hold it in your head: SPF vouches for the server, DKIM vouches for the message, and DMARC vouches for the From address your reader actually sees (and writes you a report about it). Each one covers a blind spot the others have.


## Where do you start?


If you're starting from zero, get SPF and DKIM published and verified, then add a[DMARC record](https://bird.com/en-us/blog/what-is-a-dmarc-record) in monitor mode and watch the reports. If you send through Bird, the DKIM and return-path records you publish during domain setup give you SPF alignment without an apex SPF record, so DMARC is the main thing left. The[SPF, DKIM, and DMARC guide](https://bird.com/en-us/docs/guides/email/dkim-spf-dmarc) has the specifics, and whether you're obligated to enforce is covered in[do I need DMARC](https://bird.com/en-us/blog/do-i-need-dmarc) .
