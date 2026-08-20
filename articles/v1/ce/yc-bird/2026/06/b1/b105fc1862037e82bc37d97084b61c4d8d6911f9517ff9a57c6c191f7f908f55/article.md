---
schema_version: "1.0.0"
document_id: "b105fc1862037e82bc37d97084b61c4d8d6911f9517ff9a57c6c191f7f908f55"
company_key: "yc-bird"
company: "Bird"
source_id: "yc-bird-news-import-1ae5a03d7866"
canonical_url: "https://bird.com/en-us/blog/how-does-dmarc-work"
published_at: "2026-06-16T00:00:00+00:00"
first_seen_at: "2026-07-21T10:23:31.194838+00:00"
fetched_at: "2026-07-28T21:42:41.254879+00:00"
content_hash: "sha256:d131c4603e53c53cb1d7b0f03a6ea9d589587daa6f0bf2f4d8cb74efe713af21"
---

# How Does DMARC Work?

DMARC works by adding two things on top of SPF and DKIM: an alignment check that ties authentication to the domain your recipients actually see, and a reporting loop that tells you what's happening. When a receiving server gets a message claiming to be from your domain, it runs a short, well-defined sequence to decide what to do with it.


## What happens when a receiver gets your email?


Here's the sequence a mailbox provider runs, start to finish:


1. **Read the From domain.** The server pulls the domain out of the visible From address (the technical name is the RFC5322.From). This is the identity DMARC protects, because it's the one your recipient sees.
2. **Look up your DMARC record.** It queries DNS for` _dmarc.yourdomain.com` to find your published policy and reporting addresses. No record, no DMARC evaluation.
3. **Check SPF.** It verifies the message came from a server your[SPF record](https://bird.com/en-us/blog/what-is-an-spf-record) authorizes, and notes which domain SPF authenticated.
4. **Check DKIM.** It validates the[DKIM signature](https://bird.com/en-us/blog/spf-vs-dkim-vs-dmarc) and notes the signing domain.
5. **Test alignment.** It compares the SPF and DKIM domains against the From domain. This is the step that makes DMARC more than the sum of its parts.
6. **Apply your policy.** If the message passed (either path aligned), it's delivered normally. If it failed, the receiver follows your` p=` instruction: nothing for monitor mode, quarantine, or reject.
7. **Record it for reporting.** The outcome gets added to the aggregate report that lands in your mailbox later.


## What is alignment, and why does it matter?


Alignment is the heart of DMARC, so it's worth slowing down on. SPF and DKIM each authenticate a domain, but not necessarily the one your recipient sees. SPF checks the Return-Path (the bounce address), and DKIM checks whatever domain signed the message. A spammer can pass both for a domain they control while still showing your brand in the From line.


DMARC closes that by requiring the authenticated domain to match your From domain. SPF alignment means the Return-Path domain lines up with the From domain. DKIM alignment means the signing domain lines up with the From domain. The match can be relaxed (the organizational domain is the same, so` mail.example.com` aligns with` example.com` ) or strict (an exact match). Relaxed is the default.


## How much has to pass?


Just one path. A message passes DMARC if either SPF passes and aligns, or DKIM passes and aligns. You don't need both. This is deliberately forgiving, because mail takes messy routes through the internet and one mechanism often survives where the other breaks.


Path Domains compared Passes DMARC when


SPF Return-Path and From SPF passes and the domains align


DKIM Signing domain and From DKIM passes and the domains align


Either Both of the above At least one passes and aligns


DKIM alignment is the one to lean on, because it tends to survive forwarding where SPF doesn't. That's also why forwarding is a common source of confusing report data, covered in[how to fix DMARC failures](https://bird.com/en-us/blog/how-to-fix-dmarc-failures) .


## What does the receiver do with a failure?


Whatever your policy tells it to, within reason. Your` p=` value is a request, and most major providers honor it:


- ` p=none` : do nothing different, just report. This is monitor mode.
- ` p=quarantine` : treat failing mail as suspicious, usually the spam folder.
- ` p=reject` : refuse the message outright.


The policy choice is yours to ramp over time, which is the subject of[what is a DMARC policy](https://bird.com/en-us/blog/what-is-a-dmarc-policy) . The reporting half of the loop is what makes that ramp safe: you see exactly who's passing and failing before you tighten. Decoding those reports is covered in[how to read a DMARC report](https://bird.com/en-us/blog/how-to-read-a-dmarc-report) .


## Where does this leave you?


DMARC works because it checks the one identity that matters, the visible domain, and lets either authentication path carry the message. Publish a record, let the reports show you the full picture of who sends as you, and tighten the policy when the data says it's safe. If you want the conceptual map of all three protocols,[SPF vs DKIM vs DMARC](https://bird.com/en-us/blog/spf-vs-dkim-vs-dmarc) lays them side by side, and the[authentication guide](https://bird.com/en-us/docs/guides/email/dkim-spf-dmarc) covers the Bird specifics.
