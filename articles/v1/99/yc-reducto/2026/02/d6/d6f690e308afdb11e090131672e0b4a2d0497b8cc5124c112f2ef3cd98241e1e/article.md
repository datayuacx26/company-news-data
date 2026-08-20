---
schema_version: "1.0.0"
document_id: "d6f690e308afdb11e090131672e0b4a2d0497b8cc5124c112f2ef3cd98241e1e"
company_key: "yc-reducto"
company: "Reducto"
source_id: "yc-reducto-news-import-9027c8a6c8d0"
canonical_url: "https://reducto.ai/blog/vanta-reducto-case-study"
published_at: "2026-02-20T12:00:00+00:00"
first_seen_at: "2026-07-23T23:11:50.013054+00:00"
fetched_at: "2026-07-28T22:19:31.715486+00:00"
content_hash: "sha256:53814ddfcfc95b93619a6fcd6d8fee624fb6cbbb0aeb4fa1d0bcca7fd0e59bad"
---

# How Vanta is Building AI-Native Compliance with Reducto

Compliance is one of the most manual, document-heavy processes in B2B software, and every company needs it. For early-stage startups, certifications like SOC 2 and ISO 27001 aren't just checkboxes: they're business enablers that unlock enterprise deals. For larger companies, maintaining strong compliance frameworks is essential to protecting customer data. As regulations evolve and digital footprints expand, getting this right becomes both a greater responsibility and a competitive advantage.


[Vanta](https://www.vanta.com/) set out to modernize the compliance landscape. Rather than only offering templates or static checklists, Vanta is building AI-native compliance tools that do the work for customers: drafting policies, capturing evidence, and answering security questionnaires, all while keeping humans in control of critical decisions.


> "We can empower people to do their job better by removing the busy work and making them responsible for the decisions. Compliance is a key factor in building user trust," says Ignacio Andreu, Head of Vanta AI.


But AI-powered compliance has a prerequisite: accurate document understanding. Policies, questionnaires, evidence screenshots, SOC 2 reports all flow through documents. In 15 months, Vanta grew its AI organization from 3 to over 20 engineers, releasing powerful new products to reason and act on documents at scale.


### **Finding & Choosing Reducto**


Before[Reducto](https://reducto.ai/) , Vanta first tried AWS Textract for document processing. It worked to begin with, but the team started to notice the large range of files users uploaded: PowerPoint decks filled with screenshots of spreadsheets, scanned documents with poor resolution, and a mix of formats that Textract simply couldn't handle. Quality was inconsistent, processing was slow, and costs kept climbing.


Then someone on the team heard about Reducto, a new company focused on document intelligence. Ignacio's team decided to run a test.


They put Reducto head-to-head against Textract and other PDF libraries, running the same documents through each system. "We ran a side-by-side comparison. Reducto's quality was higher, *and* it was faster. It was a no-brainer to switch," Ignacio recalls.


The technical performance mattered, but so did the partnership. The Reducto team was direct about capabilities and actively shaped their roadmap around Vanta's ideas. That responsiveness became just as valuable as the API itself during the migration.


### **Questionnaire Automation: From Hours to Minutes**


One of Vanta's most popular features tackles a pain point every B2B company faces: security questionnaires. When a prospect evaluates your product, they send a spreadsheet or PDF with dozens of questions about your security posture and compliance controls.


Answering these manually is tedious. Teams dig through SOC 2 reports, internal policies, and previous questionnaires to find answers, then copy-paste responses into the new format.


Vanta automates it. Customers upload the questionnaire alongside supporting documents – Reducto then parses the contextual evidence and extracts all the questions that need to be answered, enabling Vanta's AI to intelligently answer each question.


But generating answers was only half the solution. Without a way to create the final deliverable, customers still faced manual copy-paste work. Vanta needed to close the loop by turning AI-generated answers into a completed document that preserved the original's exact formatting.


Reducto added document filling to their roadmap after understanding the value, leading to a design partnership. The collaboration moved fast: **prototype in days, production within a month.**


> "It was a very nice win-win. Reducto built a killer feature that's very unique and very key for us as well," says Ignacio.


Now Vanta delivers a complete work artifact: upload questionnaire, AI generates answers (using the parse/extract endpoints), download the filled document ready to send (using the edit endpoint).


### **Evidence Evaluation: The Platform Behind the Platform**


The second use case is even more interesting: Vanta's Evidence Evaluation system. It validates every piece of compliance evidence uploaded, checking screenshots for completeness, verifying formatting, and ensuring nothing is missing before it goes to an auditor.


> "That feature isn't only for end customers. It's something we use internally for a lot of other features. It's a feature for the other agents to have a feedback loop," Ignacio explains.


Vanta's web agent captures compliance screenshots and sends them to the Evidence Evaluator, which identifies gaps. The agent helps iterate and give feedback until evidence is complete. The same system validates policy drafts, checking that all required sections align with compliance requirements.


### **The Roadmap Ahead**


What sets the Reducto partnership apart isn't just the technology. It's the responsiveness.


> "The team is extremely responsive. Every time someone runs into an issue, they address it or suggest a solution quickly. Being able to have that really quick feedback loop makes a difference," Ignacio says.


Looking ahead, Vanta is building increasingly sophisticated workflows and agents. Compliance operates as a graph of dependencies: policies connect to controls, controls link to evidence, evidence validates tests. When a customer updates one paragraph in a policy, it can cascade across their entire compliance program. Vanta's AI agent will soon propose those interconnected changes automatically, hopefully streamlining the complex compliance web.


The partnership continues to evolve. Vanta and Reducto are now exploring extended editing capabilities, such as maintaining specific formatting and branding which is important to enterprises. As Vanta evolves, Reducto is proud to work alongside and be the ingestion team powering what comes next.


---


***If you want to try Reducto on your own documents, either visit***[Studio](https://studio.reducto.ai/) ***to sign up for free, or***[request a demo](https://reducto.ai/contact?utm_source=website&utm_medium=blog) ***.***
