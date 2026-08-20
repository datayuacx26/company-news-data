---
schema_version: "1.0.0"
document_id: "1c76446872fb462681575f61a15b9b16a9724fc066aa41258b8296f0896936ec"
company_key: "yc-ai-2"
company: "&AI"
source_id: "yc-ai-2-rss-536318f0f92f"
canonical_url: "https://www.tryandai.com/blog/enanta-pfizer-provisional-priority-date-written-description"
published_at: "2026-07-01T00:00:00+00:00"
first_seen_at: "2026-07-20T23:19:52.502979+00:00"
fetched_at: "2026-07-28T20:47:42.945239+00:00"
content_hash: "sha256:c4668ee0e1fc0f45cc3239ff939cc895e37bdc6c7b8b2d7386fe553fc18eff06"
---

# Enanta v. Pfizer: When a Provisional Can't Carry Priority

[Back to insights](https://www.tryandai.com/blog)


# Enanta v. Pfizer: When a Provisional Can't Carry Priority


July 1, 2026


[Caleb Harris](https://www.linkedin.com/in/caleb-m-harris/)


On June 23, the Federal Circuit decided Enanta Pharmaceuticals, Inc. v. Pfizer Inc. (Lourie, J.). The case shows how a single character in a provisional application can decide patent litigation. The court held every claim of Enanta's U.S. Patent No. 11,358,953, which covers compounds for inhibiting coronavirus replication, invalid as anticipated, because the provisional did not support the patent's claimed priority date. Enanta tried to write the difference off as a typographical error, and the court did not accept it.


If you litigate validity or counsel on priority, the decision is worth reading closely. It is also the specific failure mode our newest features are designed to catch.


### Key takeaways


- The Federal Circuit invalidated all claims of Enanta's U.S. Patent 11,358,953 as anticipated, holding that its 2020 provisional (disclosing a C2–C12 alkyl substituent) gave no §112 written-description support for the patent's broader C1–C12 claim.
- Because the claims could not keep the provisional's priority date, Pfizer's April 2021 nirmatrelvir (Paxlovid) disclosure anticipated them.
- Priority is tested claim by claim against what each ancestor application actually describes—a patent can list an early filing date while individual claims are entitled only to a later one.
- The "C2 was a typo" argument failed: error-correction doctrines (§251 reissue, judicial correction) don't reach a provisional, and the alleged error was subject to reasonable debate.
- The holding is narrow—whether disclosing "2" carbons supports a "1"-carbon claim—not a broad genus/species pronouncement.


### What actually happened


The whole dispute turned on one chemical notation. Enanta's July 2020 provisional (the '048 provisional) disclosed an alkyl substituent of two to twelve carbons: —NHC(O)—C2-C12-alkyl. The '953 patent, which issued from a non-provisional filed November 9, 2021, expanded that substituent to —NHC(O)—C1-C12-alkyl, reaching a one-carbon group the provisional never recited. The timing of that expansion mattered. Pfizer's nirmatrelvir, the active compound in Paxlovid, carries a one-carbon —NHC(O)—C1-alkyl group, and Pfizer disclosed it publicly on April 6, 2021, right between the provisional and the non-provisional filing.


Enanta argued the "C2" in the provisional was a typo that should have read "C1." Reviewing the question fresh, the Federal Circuit applied 35 U.S.C. § 112 directly. The rule from Lockwood and § 120 is straightforward: a patent earns the benefit of an earlier filing date only if every application in the chain describes the later-claimed invention well enough that a skilled artisan would conclude the inventor possessed it. On this record, a provisional disclosing two-to-twelve carbons did not convey possession of the one-carbon group, so the '953 patent's —NHC(O)—C1-alkyl had no support in it. The court put it plainly: "we respect applicants' statements in their specification that they invented what was specifically disclosed in the '048 provisional, but similarly we conclude that they did not invent what they did not disclose."


Two parts of the opinion are worth flagging. The first is how narrow the holding is. The court said so itself, distinguishing Ariad and In re Ruschig and framing the question as whether "2" supports "1." This is not a broad statement about genus and species claims.


The court also rejected Enanta's fallback that the C2 was a fixable typo. The issued-patent correction doctrines Enanta invoked, reissue under § 251 (In re Oda) and judicial correction (Novo Industries), did not save the provisional, and the alleged error was subject to reasonable debate in any event.


On the written-description question itself, the court used an analogy. Disclosing ethanol, a two-carbon alcohol, says nothing about methanol, a one-carbon alcohol that is toxic. The two compounds differ by one carbon and are not the same invention.


Without the provisional's priority date, Pfizer's April 2021 disclosure anticipated the claims. Enanta did not dispute that, so the priority ruling decided the whole case at summary judgment, without needing a trial. The Federal Circuit affirmed.


### The part litigators should sit with


Priority belongs to the claims as much as to the patent. The law tests it claim by claim, against what each ancestor application actually describes. A patent can list a 2020 filing date while individual claims are entitled only to 2021, once you run the written-description analysis against the provisional. In Enanta, one unsupported substituent invalidated every asserted claim, so the court did not have to analyze the claims separately, but that claim-level principle is what made the result possible.


None of this is visible from the cover sheet. Finding the gap the way Pfizer did means pulling the provisional, reading the disclosure limitation by limitation, and asking whether a skilled artisan would see possession of the claimed scope. On a large family with several provisionals, that is days of work, and it is the same work that determines whether an anticipation theory built on intervening art will hold.


### How &AI surfaces this


Two features bring the Enanta analysis to the start of a matter, before it surfaces in an expert report.


**Provisional written-support charting.** As &AI processes a patent's family, it finds eligible provisional applications, pulls their specification documents, and, when provisional charting is enabled, generates § 112 written-description support charts comparing each provisional's disclosure against the target patent's claims. The charts appear in your normal chart views as Provisional Support – {application number}, citation-backed and scored like any other chart. You can read down each chart to see, limitation by limitation, where the provisional supports a claim and where it does not. That is the C2-versus-C1 question, answered with citations.


**Claim-level priority dates.** &AI tracks a priority date on each claim, not just on the patent. Every claim inherits the patent's date by default. Where the support charts show that an ancestor application does not carry a particular claim, you set an override date on that claim. The result is a claim list that reflects which claims hold the early date and which fall back to a later one, exposing them to intervening art.


### Why it matters


A claim that loses its provisional date is exposed to everything published in the interval, which is how Pfizer won. If you are defending priority, the same charts show where your own support is thin before opposing counsel finds it. Either way, the analysis that carried the Enanta parties through summary judgment is something you can now run early in a matter.


To see &AI run the provisional support and claim-level priority analysis on one of your own patents,[book a demo](https://www.tryandai.com/demo) . Bring a family with a provisional in the chain.


Source: Enanta Pharmaceuticals, Inc. v. Pfizer Inc., No. 25-1427 (Fed. Cir. June 23, 2026) (Lourie, J., joined by Bryson and Chen, JJ.). This post is for general information and is not legal advice.


&AI's agent identifying the C2-vs-C1 written-description gap in the '048 provisional — with citations to the substituent list and the affected claims.


## Frequently asked questions


Why was Enanta's patent held invalid?


The '953 patent claimed a one-carbon (C1) alkyl substituent its 2020 provisional never disclosed (the provisional recited C2-C12). Without § 112 written-description support, the patent couldn't claim the provisional's priority date, and Pfizer's April 2021 nirmatrelvir disclosure anticipated the claims.


Wasn't the C2 just a typo?


Enanta argued exactly that. The Federal Circuit held that the error-correction doctrines (§ 251 reissue under In re Oda, judicial correction under Novo Industries) don't reach a provisional application, and the supposed error was "subject to reasonable debate" regardless.


What's the legal standard for claiming a provisional's priority date?


Under § 112 and Lockwood, every application in the chain must describe the later-claimed invention in enough detail that a person of ordinary skill would conclude the inventor possessed it as of the earlier filing date.


Is a priority date determined per claim or per patent?


As a matter of law it is tested claim by claim, against what each ancestor application actually describes. Different claims in the same patent can hold different priority dates.


How does &AI help find this risk?


It auto-generates § 112 "Provisional Support" charts comparing eligible provisional specifications against the target patent's claims, and it tracks a priority date on each claim (inherited from the patent by default, overridable per claim) so you can flag the claims exposed to intervening art.


## Related insights


[How Patent Litigators Draft Submission-Ready Claim Charts in &AI &AI Chart Settings supports 65 formatting controls — limitation labels, Bluebook citations, screenshots, cell boilerplate, page layout, and more. The goal: from export to filing, with no edits in Word. May 11, 2026](https://www.tryandai.com/blog/how-patent-litigators-draft-submission-ready-claim-charts)[Novo Nordisk Targets Hims & Hers’ Compounded Semaglutide Offerings in Patent Lawsuit Novo Nordisk's suit against Hims signals a new phase in the fight over compounded GLP-1 drugs. February 18, 2026](https://www.tryandai.com/blog/novo-nordisk-sues-hims-for-patent-infringement-over-compounded-semaglutide)
