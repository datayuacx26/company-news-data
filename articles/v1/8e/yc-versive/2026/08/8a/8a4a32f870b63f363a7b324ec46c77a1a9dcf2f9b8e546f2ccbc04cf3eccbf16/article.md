---
schema_version: "1.0.0"
document_id: "8a4a32f870b63f363a7b324ec46c77a1a9dcf2f9b8e546f2ccbc04cf3eccbf16"
company_key: "yc-versive"
company: "Versive"
source_id: "yc-versive-news-import-a5070f4382ee"
canonical_url: "https://getversive.com/blog/august-2026-study-memory"
published_at: "2026-08-20T00:00:00+00:00"
first_seen_at: "2026-08-19T04:23:58.049058+00:00"
fetched_at: "2026-08-20T00:00:01.194919+00:00"
content_hash: "sha256:261275fe7ade116fc041b18e2e634132628d85bd06044aaa1d47a23686a6f1b7"
---

# Introducing Memory: an interviewer that learns from every interview

[Back to updates](https://www.getversive.com/blog)


August 20, 2026 •


Product Updates


### Introducing Memory: an interviewer that learns from every interview


A new beta setting that lets the AI interviewer learn from completed interviews in a study, so follow-up questions get sharper as responses come in.


---


A human moderator's twentieth interview in a study is better than their first. They know which threads keep coming up, which follow-ups actually get people talking, and which questions have already been answered ten times. Until now, an AI interviewer started every conversation from zero.


**Memory** changes that. It's a new per-study setting, available now in beta: turn it on, and the AI interviewer learns from the completed interviews in your study, so later interviews probe deeper on what matters.


### How it works


After each completed interview, Versive extracts small, typed observations from the transcript: emerging findings, follow-up questions that worked well, threads participants opened but nobody has explored, and guidance about what to do differently. Those observations roll up into a compact study-level memory.


When the next participant starts, the interviewer gets a short briefing built from that memory. In practice, that means it will:


- **Pursue open threads** : if earlier participants hinted at something nobody has unpacked, the interviewer digs in when it comes up again
- **Pressure-test emerging findings** : patterns get confirmed or challenged with new participants instead of sitting unexamined
- **Reuse questions that work** : follow-ups that got rich answers earlier get adapted and asked again
- **Stop re-probing settled ground** : once something is well covered, the interviewer spends its follow-ups elsewhere


Memory works in both text and voice interviews, and everything happens automatically in the background as responses come in.


### Careful by design


An interviewer that learns is only useful if it learns responsibly, so Memory is built with some deliberate epistemics:


- **Findings stay tentative until they've earned it.** A pattern is only treated as established after multiple participants confirm it, and at least one of those confirmations has to be volunteered unprompted, so the interviewer can't talk participants into agreeing with its own hypothesis.
- **Disagreement is a signal, not noise.** When participants contradict an earlier finding, it gets marked contested and moves to the top of the briefing, so the interviewer probes to resolve the disagreement instead of papering over it.
- **Participants never see each other's answers.** Memory shapes what the interviewer asks, never what it reveals: it will not share or attribute anything a previous participant said.


Memory is scoped to a single study, never leaves your organization, and is never exposed to participants.


### Turning it on


You'll find the **Memory** toggle in your study settings under AI Interviewer. It's off by default while in beta; flip it on for studies where you expect meaningful volume, and the interviewer will start learning from the first completed response. We'd love to hear what it surfaces for you.


---


### Get in touch


We're continuing to build features that help you learn faster. If you have any questions or feature requests, reach out to us at[\[email protected\]](https://www.getversive.com/cdn-cgi/l/email-protection#84ecedc4e3e1f0f2e1f6f7edf2e1aae7ebe9) . Want to give it a try?[Start a free trial](https://www.getversive.com/signup) and start running research in minutes.


---


Eric Li


, Co-Founder, Versive
