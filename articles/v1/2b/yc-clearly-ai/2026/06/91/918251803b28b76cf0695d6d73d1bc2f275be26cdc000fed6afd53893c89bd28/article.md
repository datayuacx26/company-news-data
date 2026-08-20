---
schema_version: "1.0.0"
document_id: "918251803b28b76cf0695d6d73d1bc2f275be26cdc000fed6afd53893c89bd28"
company_key: "yc-clearly-ai"
company: "Clearly AI"
source_id: "yc-clearly-ai-news-import-066e8c806377"
canonical_url: "https://clearly-ai.com/blog/good-data-scientists-and-bad-outcomes-what-s-the-difference-between-security-and-privacy-work"
published_at: "2026-06-26T00:00:00+00:00"
first_seen_at: "2026-07-23T05:49:00.156409+00:00"
fetched_at: "2026-07-28T21:42:09.561502+00:00"
content_hash: "sha256:b647ddf2cc344e2ea8818aaa5e4547e501b45993b11a8059023c3ab45851501e"
---

# Good data scientists and bad outcomes, part 1: What's the difference between security and privacy work?

*Note: This 2-part blog has been lightly adapted from a talk I gave at B-Sides SF 2026. The theme that year was musicals, so there are occasional easter eggs for the theater kids in here.*


Before I joined Clearly AI I spent five years as a principal privacy engineer at Amazon. Before that, five years running an applied machine learning research lab at Symantec, building security tools for everything from malware to DLP and spearphishing. From the outside, or either side of the fence, data security and privacy work seem like they're very similar. At some level they are. Both teams want to protect data. Both teams want a data inventory, access controls, encryption at rest and in transit, and so on. However, multiple times in my career, I've watched security teams take over privacy functions, and then struggle to deliver results. In this blog, I'll talk about why, and the fundamental differences in objectives, tools, and mindset between the two disciplines, so that you can be great at both.


Since privacy is often heavily overlapped with compliance, I'm going to remind you that I am not a lawyer, and I'm especially not *your* lawyer. Please go talk to your lawyer when you put this into practice. Privacy legal folks are often delightful people, which is good, because you will need to work with them regularly. (That’s the first difference right there.)


## Privacy in a nutshell (and all that jazz)


If you're coming in from security, here are a couple of core privacy concepts that you may not yet know.


**Personal data is not (just) PII.** I'll come back to this because it's the single most expensive misunderstanding in the field, but in short, personal data is a vastly bigger category than the "personally identifiable information" checklist that your DLP scanner detects.


**The right to deletion and correction, and the right to know.** Users can ask you at any time to delete their data, fix it, or tell them what you have. "We can't actually find all of this person's data" is not a great defense.


**Purpose-of-processing limitations.** If you didn't already tell the customer you were going to do a thing with their data, you can't decide to do it now. The phone number you collected saying it was only for security purposes[is not kosher to use for ads targeting](https://www.ftc.gov/news-events/news/press-releases/2022/05/ftc-charges-twitter-deceptively-using-account-security-data-sell-targeted-ads) . Purpose is sticky, it travels with the data.


**Consent.** My favorite way to describe consent to an engineering audience is that it's like Chaos Monkey for your data pipelines. Consent can be absent, partial, or withdrawn at any moment, and your systems have to respond correctly when it changes. A user who withdraws consent is the privacy equivalent of an instance that just got killed at random. If your data pipeline can't handle it gracefully, you have a design problem, and maybe also a whopping fine.


With the basics out of the way, let’s do some comparison of the two disciplines.


## Security and privacy: what does a bad day look like?


Security, broadly, is about protecting data from people who shouldn't have it or want to endanger it. (The good old CIA triad.) When security fails, you get outages, breaches, ransomware, and intellectual property theft.


Privacy is about the appropriate use, retention, and deletion of data. When privacy fails, you get failures of deletion, failures to fulfill data subject access requests, failures of consent, and failures of purpose limitation. You get children's data sitting in an ML model that was never supposed to see it.


In both cases, you're going to have reputational loss and potential fines. But privacy fines are on average much, much, bigger.


## Who's the bad guy?


If you've spent any time in security, you know what a bad guy is, and they’re, well, bad. You’re defending against a hoodie-wearing hacker, a ransomware crew, or maybe a malicious insider. Security is built to keep the bad guys out and the good data in, and it’s pretty clear who is who.


But your worst nightmare on the privacy side is a data scientist, one with the best of intentions. They're the most likely to perform data re-identification, to over-retain and under-catalog, and violate consent and purpose of processing. And they’re doing it because they really, really, want to help the business and don’t understand the consequences. Privacy almost never has a clear cut bad guy.


## Data handling differences. "No me diga!"


Security: “Confidentiality is privacy.” Privacy: “Appropriate use, retention, and deletion of data is privacy”.


Security: "Keep the red data in the red zone.” (Access control) Privacy: “Don’t give child data to the ads targeting system.” (Filtering)


Security: “Protect data integrity and make lots of backups." Privacy: "Delete all this data *everywhere* , including backups (unless it's a security log, under litigation hold, used for tax and accounting...)"


In most cases, privacy is handling a messier problem, with more edge cases. Security leaders that are used to being able to easily measure what “correct” is may struggle with a domain that requires them to embrace nuance and uncertainty.


## Hold up, I need data semantics?!


If you thought data inventory for security was hard, privacy is even worse. Not only do you need to know what data you have in each data store, and what data classification it is, you need to know about the semantics of the data, at a row/record and column/field level. You also need to know the purposes storage and compute are used for, so you can assess whether a given record or field should end up in that system, and whether you should delete data out of it. You have to find a way to build (or simulate) fine-grained, role-based, access control and filtering that lines up with the law.


Here’s a (denormalized) example of the kinds of information you need, and why. Let’s say we want data to determine the users we want to target for an ad.


-


Alice and Dhiren are minors, so their records get filtered. We need to know there’s an age field.


-


Carlos did not give consent, so that record is out. We need to know about the consent field.


-


The phone number was collected for security purposes only, toss it. We need to either have propagated, or be able to look up, the purposes we collected that data for.


-


And our legal team decided that the facial ID we used is biometrics, which can’t be used in some jurisdictions, so we have to catalog THAT, and then filter out the field for Emilie-Jean, based on her locale.


Out of the 5 records x 8 fields we started with, we have two records left, one with 7 fields, one with 6. And the only reason we followed the law is because we captured data semantics at a granular level.


## "But I just follow the law and I'm good, right?"


Yes! And.


GDPR, CCPA, etc. are not a controls framework. They're more like a compiler spec, with a lot of undefined behavior until case law defines it. And the privacy "controls frameworks" that do exist are, to put it gently, not great. Some of them are designed around US government needs, which doesn't help if you're a consumer tech company. The NIST Privacy Framework barely even addresses consent and purpose limitation, which, ironically, are things regulators care deeply about.


Depending on what you're doing, there may be multiple other laws you need to contend with. Sarbanes-Oxley and HIPAA both interact in interesting ways with GPDR and its clones due to retention requirements and a few other things.


And then there are consent decrees. If you have made the FTC exceptionally wroth with you, you may wind up with what is effectively a custom-built regulation, just for you, with its own bespoke requirements and its own auditors. In some cases, it might even[follow a single person between employers](https://www.ftc.gov/news-events/news/press-releases/2022/10/ftc-takes-action-against-drizly-its-ceo-james-cory-rellas-security-failures-exposed-data-25-million) . There is no framework that can help you in those cases.


You can't outsource your privacy program's design to a generic checklist, just like you can’t run NIST 800-53 out of the box. You have to reason about your specific data, your specific purposes, your specific legal obligations, and then build controls to match. And even if you've done all of that, you can still be tried in the court of public opinion for not respecting and protecting your customers.


## The scanner will save me (narrator: it did not)


Find the sensitive stuff with a scanner, tag it, lock it down. This works reasonably well for security because the things you're hunting often have recognizable shapes. A Social Security number looks like a Social Security number. A credit card number has a checksum. Malware has some characteristic entropy patterns.


In privacy, you usually aren't looking for an SSN. You're looking for the UUID you assigned to each customer. And that UUID looks exactly like every other UUID in your system, including orders, sessions, devices, and the document ID of the cafeteria menu. There is no regex for "this opaque identifier happens to be a person."


Once you do find it, for extra credit, you now need to determine whether that UUID belongs to a person who has consented to whatever you're about to do. And find all the linked session UUIDs if you want to delete a user’s data. And figure out whether that integer between 40 and 220 is a heart rate, in which case it's probably health data (with all the extra obligations that entails), or whether it's the number of minutes us-east-1 was down this week, in which case it's an operational metric, not personal data, and nobody cares (at least not on the privacy team).


## LLMs will save me! (narrator: they also did not)


Obviously, I work on a product with LLMs at the core, and they can be powerful security/privacy tools, BUT. If you didn’t capture it at data ingest, an LLM likely cannot re-create it. If your developer uses names like “fid”, “dq”, and “feature 1”, the LLM can’t read their mind to infer semantics (any more than you can). And most importantly, LLMs cannot, should not, make legally fraught decisions like “is this time stamp personal data? Or a business technical secret?” Call your lawyer. Did I mention you're going to want to be friends with them? Getting semantics wrong can be a very expensive mistake.


## Speaking of scanners, PII does not mean what you think it means


The term "PII" is comforting. It implies a finite list. Name, SSN, email, maybe a phone number. It's pithy, easy to say, and horribly misleading. PII doesn't even appear in most privacy laws. GDPR, CCPA, etc. instead talk about identifiable personal data. And personal data often doesn't even *contain* PII.


For example, GDPR: “Personal data are any information which are related to an identified or identifiable natural person. The data subjects are identifiable if they can be directly *or indirectly* identified.”


Your email, your full name, your YouTube channel, are all direct identifiers because they’re public. Anything unique that's linked to a direct identifier becomes an indirect identifier. That might mean internal user IDs, serial numbers, VINs, and order IDs. From there, anything linkable to an identifier is probably personal data. Your shoe size. The make, model, and mileage of your car. The number of times you clicked "I am not a robot." That data can be 5 levels of indirection deep, doesn't matter. It's still personal data as long as you can walk the pointers back.


So please, if you take only one thing away from this Part 1 blog, I beg you to stop saying, and thinking about, PII, and start thinking about personal data.


(Again: not legal advice. Consult your lawyer when deciding what’s personal data.)


In Part 2, we'll cover how to put all this into practice.
