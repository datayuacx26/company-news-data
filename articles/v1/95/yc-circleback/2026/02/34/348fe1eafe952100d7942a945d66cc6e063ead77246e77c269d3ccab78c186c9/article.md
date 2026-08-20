---
schema_version: "1.0.0"
document_id: "348fe1eafe952100d7942a945d66cc6e063ead77246e77c269d3ccab78c186c9"
company_key: "yc-circleback"
company: "Circleback"
source_id: "yc-circleback-news-import-b22aa3eada49"
canonical_url: "https://circleback.ai/blog/recording-consent-for-ai-meeting-notes"
published_at: "2026-02-24T00:00:00+00:00"
first_seen_at: "2026-07-24T01:33:40.934837+00:00"
fetched_at: "2026-07-28T21:57:40.692150+00:00"
content_hash: "sha256:5efe0a146e5f521b05f223487ca475a8928dc970a124801c833d4bcfe2e8eb84"
---

# Recording Consent for AI Meeting Notes: What You Need to Know

"Check if you're in a one-party or two-party consent state." That's the standard advice for anyone using AI meeting notes, and it's dangerously incomplete. Recording consent for AI meeting tools is more complex than a simple state-by-state map: it ignores cross-border calls, says nothing about EU member-state criminal codes, doesn't address whether a meeting bot counts as disclosure, and skips the industry-specific rules that override general consent law entirely. Two active class-action lawsuits against AI meeting tools suggest the legal system agrees: the simple framework isn't simple enough.


**This post is an informational overview of the recording consent landscape, not legal advice. Laws vary by jurisdiction and change frequently. Consult a qualified attorney for guidance specific to your organization, industry, and locations.**


## Key takeaways


- **Eleven US states require all-party consent** for recording: California, Connecticut, Florida, Illinois, Maryland, Massachusetts, Michigan, Montana, New Hampshire, Pennsylvania, and Washington. Cross-state calls should default to the most restrictive jurisdiction.
- **A visible meeting bot is not legally sufficient consent.** No jurisdiction treats bot presence in a participant list as informed agreement. You need explicit disclosure regardless of recording method.
- **GDPR is a data processing framework, not just a consent law.** Consent is one of six lawful bases — organizations may also rely on legitimate interest or contractual necessity, but each requires documented justification.
- **Two active class-action lawsuits** (Cruz v. Fireflies.AI, Brewer v. Otter.ai) are testing whether AI meeting tool providers or individual users bear responsibility for obtaining participant consent.
- **Industry-specific rules override general consent law.** Healthcare (HIPAA), financial services (FINRA/SEC), legal (attorney-client privilege), and education (FERPA) all impose additional requirements beyond state wiretapping statutes.
- **Three practices form a common baseline:** most organizations disclose before recording, obtain acknowledgment, and document their process. Consult legal counsel for your specific requirements.


## U.S. recording consent: beyond the one-party/two-party split


Federal wiretapping law ([18 U.S.C. § 2511](https://www.law.cornell.edu/uscode/text/18/2511) ) sets the baseline: at least one party to the conversation must consent to the recording. In most cases, if you're in the meeting and you consent to recording it, the federal standard is met.


States add their own requirements on top. Eleven states require all-party consent: California, Connecticut, Florida, Illinois, Maryland, Massachusetts, Michigan, Montana, New Hampshire, Pennsylvania, and Washington. In these states, every person in the conversation must agree to the recording, not just the person pressing record. Violations carry both civil liability and, in several states, criminal penalties.[The Reporters Committee for Freedom of the Press](https://www.rcfp.org/introduction-to-reporters-recording-guide/) maintains the most comprehensive state-by-state reference.


The two-category framework breaks down quickly once you examine the details. Connecticut requires "knowledge" of the recording rather than affirmative consent, a distinction that matters when deciding what disclosure actually looks like. Nevada requires all-party consent for phone and electronic communications (NRS 200.620) but applies a one-party standard for in-person conversations (NRS 200.650). Oregon flips a similar distinction: all-party consent for in-person conversations, but one-party consent for electronic communications under certain conditions. These aren't obscure edge cases. They're the jurisdictions where teams using AI meeting tools are most likely to get the analysis wrong by relying on a simple consent map.


Cross-state calls add another layer. When participants sit in different states, the prevailing legal guidance is to follow the most restrictive state's requirements. A call between someone in Texas (one-party) and someone in California (all-party) should be treated as an all-party consent situation. This is conservative but defensible, and it's the approach most corporate legal teams recommend. But it only works if someone on the team actually knows which states are in play — and for distributed teams, a single video call can easily span three or four jurisdictions. AI meeting tools add a layer to this problem because the tool itself may process or store the recording in yet another jurisdiction, potentially triggering data handling obligations separate from the recording consent question. The practical result is that teams using AI meeting notes across a distributed workforce need a consent process robust enough to cover the most restrictive scenario by default, rather than attempting jurisdiction-by-jurisdiction analysis for every call.


## International recording consent


If anyone on your call is in the EU, you need to think about GDPR. But the first thing to understand is that GDPR is not a consent-for-recording law in the way U.S. wiretapping statutes are. It's a data processing framework, and recording is one form of processing.


Under GDPR[Article 6](https://gdpr-info.eu/art-6-gdpr/) , consent is one of six lawful bases for processing personal data, not the only one. Organizations may also rely on legitimate interest, contractual necessity, or other bases depending on context. When consent is the chosen basis,[Article 7](https://gdpr-info.eu/art-7-gdpr/) requires it to be specific, informed, freely given, and unambiguous. Penalties under[Article 83](https://gdpr-info.eu/art-83-gdpr/) reach 4% of global annual revenue or €20 million, whichever is greater. The distinction between GDPR's lawful-basis framework and U.S. wiretapping consent matters because conflating them leads to the wrong compliance strategy. A company that treats GDPR as "just get everyone to click agree" misses the five other lawful bases that may be more appropriate for workplace recordings. A company that relies on legitimate interest without documenting the required balancing assessment is exposed to enforcement action. The correct approach depends on the recording context, the relationship between the parties, and whether participants can genuinely refuse without consequences.


The GDPR framework is a floor, not a ceiling. Individual EU member states layer their own recording-specific laws on top, and several are significantly stricter. Germany's § 201 StGB (Strafgesetzbuch) makes unauthorized recording of private speech a criminal offense punishable by up to three years' imprisonment, regardless of GDPR compliance. France's Article 226-1 of the Penal Code similarly criminalizes recording private conversations without consent, carrying up to one year's imprisonment and a €45,000 fine. These are not data protection provisions. They are criminal statutes that apply independently of any GDPR analysis, which means a company can be fully GDPR-compliant and still violate national criminal law.


Outside Europe, the landscape fragments further. Canada's federal privacy law, PIPEDA, requires knowledge and consent for collecting personal information, with provincial laws in Quebec, British Columbia, and Alberta adding further requirements. The UK, post-Brexit, operates under a framework substantially similar to GDPR, with "legitimate interest" providing a potentially viable basis for workplace recordings where proper assessments have been documented. Australia's Surveillance Devices Acts vary by state and territory, with some requiring all-party consent and others permitting one-party consent, creating a patchwork similar to the U.S. model.


The practical lesson: no single international rule governs meeting recording. If your team is distributed across borders, you need to map the specific requirements for each jurisdiction where participants are located, not just the jurisdiction where the company is headquartered.


## Does a meeting bot count as notice?


This is the question teams using AI notetakers ask most often: if the bot is right there in the participant list, labeled clearly, doesn't that serve as disclosure?


The short answer is no. No jurisdiction currently treats the visible presence of a recording bot in a meeting's participant list as legally sufficient notice or consent. Consent, under both U.S. state wiretapping laws and GDPR, requires informed agreement, not mere awareness. A participant must understand what is being recorded, how the recording will be used, who will have access, and how long it will be retained. Under GDPR, participants must also be informed of their rights regarding that data. A bot labeled "Circleback Notetaker" or "Otter.ai" in a participant list communicates that something is present. It does not communicate what that tool is doing with the audio, where the data is stored, whether it is used for model training, or how long it persists. The gap between "I can see something is here" and "I understand and agree to what it's doing" is the gap between awareness and consent. No court or regulator has accepted bot visibility as bridging it.


This means the[recording method](https://circleback.ai/blog/bot-vs-desktop-recording) doesn't change the consent obligation. Whether you use a bot that joins visibly or desktop recording that captures audio locally, you still need affirmative disclosure. The bot's visibility may make the conversation about consent more natural ("you can see our notetaker has joined; is everyone comfortable?"), but the visibility itself is not a substitute for actually obtaining it.


## AI meeting tools in court


Two active class-action lawsuits are testing the legal boundaries of AI meeting recording directly.


In *Cruz v. Fireflies.AI Corp.* (C.D. Ill., filed Dec. 2025), the plaintiff alleges that Fireflies.AI collected voiceprint biometrics from meeting participants without the informed written consent required by the Illinois Biometric Information Privacy Act (BIPA). The complaint asserts that the company's AI notetaker joined meetings, captured voice data, and processed it to create speaker-identifying voiceprints without satisfying BIPA's notice and consent requirements.


In *Brewer v. Otter.ai Inc.* (N.D. Cal., filed Aug. 2025), the claims are broader. The plaintiff brings allegations under the Electronic Communications Privacy Act (ECPA), the Computer Fraud and Abuse Act (CFAA), the California Invasion of Privacy Act (CIPA), the California Comprehensive Computer Data and Fraud Access Act, and California's Unfair Competition Law (UCL). The complaint alleges that Otter.ai's meeting assistant recorded conversations without proper authorization from all participants.


Neither case has been resolved. The allegations are not findings of liability, and both defendants may prevail. But the pattern these lawsuits represent matters regardless of outcome. Courts are being asked to decide three questions that don't have settled answers: whether AI meeting assistants must obtain consent from every meeting participant, not just the user who enabled the tool; whether voice data collected by these tools constitutes biometric information subject to stricter consent requirements under laws like Illinois BIPA; and whether the individual user who activated the tool bears responsibility for obtaining consent from other participants, or whether that obligation falls on the tool provider. The fact that these questions are being litigated creates uncertainty that affects every organization using AI meeting tools, not just the named defendants. Companies that wait for rulings before establishing consent processes are making a bet that the rulings will be favorable, which is a bet, not a compliance strategy.


## Industry-specific requirements


*The following is a general overview of how certain regulations intersect with meeting recording. It is not a compliance guide. Organizations in regulated industries should work with legal counsel familiar with their specific regulatory obligations.*


The lawsuits above are testing the general legal framework. In regulated industries, the picture gets stricter. Industry-specific rules can override or substantially complicate the baseline consent analysis, and "we follow our state's recording laws" is rarely a complete answer.


**Healthcare (HIPAA):** Recording telehealth sessions or clinical meetings involves protected health information (PHI). HIPAA requires a Business Associate Agreement (BAA) with any AI meeting tool processing PHI, explicit patient consent for recording, and storage that meets HIPAA security standards. Recording a telehealth appointment without meeting all three requirements isn't merely non-compliant. It's a potential violation of federal law with penalties exceeding $2 million per violation category per year (as adjusted for inflation).


**Financial services (FINRA/SEC):** Broker-dealers face a paradox. FINRA and SEC rules require archiving business communications, which creates a regulatory incentive to record. But recording creates its own compliance obligations around consent, data retention, and supervision. The archiving requirements are not optional: financial regulators have levied over $2 billion in fines for communication archiving violations since 2021, across more than 100 firms that failed to capture and retain communications on approved platforms. Any AI meeting tool used in a financial services context must integrate with the firm's existing compliance and archiving infrastructure.


**Other regulated contexts:** Attorney-client privilege means recording legal strategy meetings can waive the privilege if recordings are not properly secured or if third parties gain access. Educational settings involving student records fall under FERPA, which imposes its own notice and consent requirements distinct from state wiretapping law. Government meetings may be subject to open-meetings statutes that require recording or, conversely, prohibit it in executive sessions. The common thread across all regulated industries is that general consent analysis is necessary but not sufficient. Industry-specific rules layer on top, and they frequently impose stricter requirements than the baseline.


## What most organizations do in practice


*This section describes common practices we've observed across teams using AI meeting tools. It is not a substitute for legal review of your specific situation.*


The legal landscape is complex, but the operational response doesn't have to be. Regardless of jurisdiction, recording mode, or industry, three practices tend to form the baseline.


**Disclose before recording.** Many teams tell participants the meeting will be recorded and that AI will process the audio. Being specific tends to be more effective: "Our AI notetaker will record this meeting to generate a transcript and summary" versus "this call may be recorded." Disclosure can happen verbally at the start of the call, in the calendar invite, or via a message in the meeting chat.


**Get acknowledgment.** In all-party consent states and under GDPR (when consent is the lawful basis), affirmative agreement — not just absence of objection — is generally expected. "Does everyone consent to the recording?" followed by explicit confirmation is a common approach. In one-party consent jurisdictions, disclosure without explicit agreement may be legally sufficient, but many organizations obtain acknowledgment anyway to eliminate ambiguity.


**Document your process.** Many organizations maintain a recording policy that specifies what is recorded, how recordings are stored and retained, who can access them, and how participants can request deletion. Under GDPR, documentation of data processing activities is a regulatory requirement. In other jurisdictions, having a documented, consistently applied process is widely considered a best practice, particularly given active litigation in the space.


For teams using AI meeting tools, one additional step matters: review how your tool handles audio data. Where is it stored? How long is it retained? Is it used for model training? These questions feed directly into the disclosure and documentation obligations above, and the answers vary by provider. Understanding[how the AI pipeline processes your meeting audio](https://circleback.ai/blog/how-ai-meeting-notes-work) is part of meeting the consent obligation, not a separate technical question.


None of this eliminates legal risk entirely, and nothing in this post should be treated as a compliance checklist for your organization. With two active class actions and recording consent law still catching up to AI meeting tools, the legal landscape will keep shifting. Organizations that want to stay ahead of these changes should work with legal counsel to build a consent process that fits their specific jurisdictions, industries, and use cases.


*Circleback provides both bot-based and desktop recording with clear consent workflows built into each mode.*[See how it works.](https://circleback.ai/)


## Frequently asked questions


**Is it legal to record a meeting without telling anyone?** It depends on jurisdiction. Under U.S. federal law and in 39 states plus the District of Columbia, one-party consent applies, meaning you can legally record a conversation you're participating in without notifying others. However, eleven states (including California, Illinois, and Pennsylvania) require all-party consent, making undisclosed recording illegal. In the EU, both GDPR and member-state criminal codes (such as Germany's § 201 StGB) generally require informed consent or another lawful basis. Even where one-party consent applies, professional best practice is to disclose, particularly when using AI tools that process and store audio data.


**Do I need consent to use an AI meeting notetaker?** Yes, in most practical scenarios. Any AI meeting tool that records audio and processes it into transcripts, summaries, or action items is performing activities that consent and privacy laws govern. In all-party consent states and under GDPR, you need affirmative agreement from participants before recording begins. The pending lawsuits against Fireflies.AI (BIPA) and Otter.ai (ECPA, CIPA) are actively testing whether the user who enables the tool bears responsibility for obtaining consent from every participant. Until those questions are resolved by the courts, obtaining explicit consent from all participants is the safest approach regardless of jurisdiction.


**What states require all-party consent for recording?** Eleven U.S. states require all-party consent for recording conversations: California, Connecticut, Florida, Illinois, Maryland, Massachusetts, Michigan, Montana, New Hampshire, Pennsylvania, and Washington. Some of these states have important nuances that complicate the simple two-category framework: Connecticut technically requires "knowledge" rather than affirmative "consent," Nevada requires all-party consent for electronic communications but one-party for in-person conversations, and Oregon similarly distinguishes between the two. For cross-state calls where participants span multiple jurisdictions, the standard guidance is to apply the most restrictive participating state's requirements. The[Reporters Committee for Freedom of the Press](https://www.rcfp.org/introduction-to-reporters-recording-guide/) maintains the most current and comprehensive state-by-state reference.


**Does GDPR apply to recording meetings?** GDPR applies whenever you record a meeting involving EU participants or process recordings in the EU, regardless of where your organization is headquartered. Audio recordings constitute personal data under[Article 4](https://gdpr-info.eu/art-4-gdpr/) . However, consent is only one of six lawful bases for processing under Article 6. Organizations may also rely on legitimate interest (with a documented balancing assessment), contractual necessity, or other bases depending on context and the relationship with participants. Critically, GDPR compliance alone may not be sufficient: individual member states like Germany and France have criminal statutes governing unauthorized recording that apply independently of any data protection analysis.


**Does a meeting bot count as recording consent?** No. No jurisdiction currently treats the visible presence of a recording bot in a meeting's participant list as legally sufficient notice or consent. Consent requires informed agreement — participants must understand what is being recorded, how the recording will be used, who will have access, and how long it will be retained. A bot labeled "Notetaker" communicates that something is present, but it doesn't communicate what the tool does with the audio, where data is stored, or whether it's used for model training. Whether you use a[visible bot or desktop recording](https://circleback.ai/blog/bot-vs-desktop-recording) , you still need explicit disclosure. The bot may make the consent conversation more natural ("you can see our notetaker has joined — is everyone comfortable?"), but its presence alone doesn't satisfy the legal requirement.


**Can I record a meeting if someone objects?** If any participant objects to recording, the safest and most professional course of action is to stop recording or not begin. In all-party consent jurisdictions, continuing to record over an objection is illegal. In one-party consent jurisdictions, you may have the legal right to continue, but doing so over a stated objection creates relationship damage and potential liability if the legal landscape shifts. If you need documentation from the conversation, take notes manually or ask the objecting participant what accommodation would work for them. Preserving the relationship almost always matters more than preserving the transcript.
