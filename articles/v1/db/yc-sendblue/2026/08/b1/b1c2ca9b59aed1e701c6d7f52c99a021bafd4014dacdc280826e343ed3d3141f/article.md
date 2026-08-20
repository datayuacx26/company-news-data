---
schema_version: "1.0.0"
document_id: "b1c2ca9b59aed1e701c6d7f52c99a021bafd4014dacdc280826e343ed3d3141f"
company_key: "yc-sendblue"
company: "Sendblue"
source_id: "yc-sendblue-news-import-cbfb84d5bb49"
canonical_url: "https://www.sendblue.com/blog/sendblue-security-incident-update"
published_at: "2026-08-05T00:00:00+00:00"
first_seen_at: "2026-08-07T06:38:35.807311+00:00"
fetched_at: "2026-08-07T06:38:37.755907+00:00"
content_hash: "sha256:83e99b0a59350ad2c73eb4ba4710a695f8aaec93badb52a58ab716e41fa75dd9"
---

# Incident report for August 5, 2026

On August 1, 2026, Sendblue received reports that some contacts had received fraudulent SMS messages impersonating Sendblue customers and requesting payment through a suspicious link.


The fraudulent messages were not sent through Sendblue. They were sent from external Sinch toll-free numbers and directed recipients to a domain that is not owned or operated by Sendblue.


We are treating this as a security incident because the campaign targeted phone numbers associated with Sendblue customer messaging activity.


On July 31, 2026, an external security researcher reported a vulnerability in our Firebase/Firestore security rules. We immediately treated the issue as a SEV-0 incident and fixed the vulnerability that day.


Our investigation has identified unauthorized access to certain Firebase/Firestore conversation records on July 7 and July 10. Based on the evidence reviewed to date, the information involved may include message content, phone numbers, contact information, Sendblue line associations, Sendblue organization user names, and related messaging metadata.


For some customers, we have confirmed that records were accessed. Other customers had records that were accessible through the affected paths, but we have not confirmed that those records were retrieved. We are contacting affected customers directly and providing information specific to their accounts.


Our investigation remains active. We have preserved relevant evidence, engaged Oneleet, our third-party cybersecurity partner, and have begun reporting the incident to the relevant authorities. We are continuing to assess the full scope of the incident and will update this notice if our findings materially change.
