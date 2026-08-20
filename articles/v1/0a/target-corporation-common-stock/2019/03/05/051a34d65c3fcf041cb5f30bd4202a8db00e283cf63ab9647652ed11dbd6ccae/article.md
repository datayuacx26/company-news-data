---
schema_version: "1.0.0"
document_id: "051a34d65c3fcf041cb5f30bd4202a8db00e283cf63ab9647652ed11dbd6ccae"
company_key: "target-corporation-common-stock"
company: "Target Corporation"
source_id: "target-corporation-common-stock-rss-2844690dfd24"
canonical_url: "https://tech.target.com/blog/target-finds-cross-site-scripting-in-microsoft-sharepoint"
published_at: "2019-03-15T05:00:00+00:00"
first_seen_at: "2026-07-20T03:31:39.462984+00:00"
fetched_at: "2026-07-28T21:06:00.870812+00:00"
content_hash: "sha256:f04724130d251f65c7e7454691c3cd9efc46de707827b8a96c43fe2597d7b031"
---

# Target Finds Cross-Site Scripting in Microsoft SharePoint

Cross-site scripting has been an OWASP Top 10 classic for more than a decade, but it still comes as a surprise to find it out in the wild, especially in a well-known product. During a recent penetration test, Target's Security Testing Services team found that Microsoft's SharePoint was vulnerable to a unique attack that, unlike typical cross-site scripting, could be exploited without any interaction from the victim user.


Some quick background: SharePoint is part of the Microsoft Office 365 product line, providing an online web interface for Outlook, Excel, Word and other Microsoft resources. This portal notifies users within their browsers about new emails, Lync/Skype messages and upcoming meetings. It was this notification system that enabled attackers to inject code into a victim's browsers by simply sending the victim an email.


Detection


This vulnerability was discovered by accident during a pentest of an unrelated application. At one point during the assessment, we performed an action that made the application send a notification email to the assessor containing a basic cross-site scripting payload. Soon after, the assessor’s SharePoint session was interrupted by the iconic alert(1) box:


After some digging, we found that around once a minute, the browser sends a GET request for the following URL:


```text
https://outlook.office365.com/owa/ev.owa2?ns=PendingRequest&ev=PendingNotificationRequest&UA=0&cid=[cid]&X-SuiteServiceProxyOrigin=https://[company].sharepoint.com
```


The SharePoint server responds with information about any new emails in some JSON data, which will be dynamically incorporated into the user’s open SharePoint page:


```text
HTTP/1.1 200 OK                       [TRUNCATED]                                  <script>[{"__type":"NewMailNotificationPayload:#Exchange","id":"NewMailNotification","Sender":"SenderInformation","Subject":"Email Subject","PreviewText":"Preview Text","ItemId":"[itemID]","ConversationId":"[conversationID]","IsClutter":false,"SenderSmtpEmailAddress":"johndoe@example.com","InferenceClassification":"Focused","EventType":"0"}]</script>
```


The


PreviewText


parameter contains the contents of the email but fails to sanitize potentially troublesome characters like


< > /


. SharePoint does properly escape the apostrophe


’


and doublequote


”


characters, though backticks made it through unescaped. Given this, an email containing the payload


</script><script>alert('hello')</script>


generates the following notification:


```text
<script>[{"__type":"NewMailNotificationPayload:#Exchange","id":"NewMailNotification","Sender":"SenderInformation","Subject":"Email Subject","PreviewText":"</script><script>alert('hello')</script>","ItemId":"[itemID]","ConversationId":"[conversationID]","IsClutter":false,"SenderSmtpEmailAddress":"johndoe@example.com","InferenceClassification":"Focused","EventType":"0"}]</script>
```


Note that the payload closes the existing open script tag, allowing our injected script to function properly:


Exploitation


This flaw could be exploited in any number of ways. We created this basic proof-of-concept attack when disclosing this vulnerability to Microsoft:


An attacker crafts an innocent-looking email by shrinking and recoloring the malicious code to look invisible to the reader:


If the victim is signed into SharePoint when they receive the email, the payload will open an alert box in the victim’s browser stating that their session has expired and they need to reauthenticate:


The payload will then redirect them to the attacker’s webpage, set up as a fake Microsoft login page:


To be successful, an attacker only needs a single employee who is signed into Sharepoint to receive the malicious email. Note that the victim user doesn't need to actually open the email for the attack to work - the notification alone triggers the exploit.


Conclusion


Target's Security Testing Services team responsibly disclosed the vulnerability to Microsoft. After completing our working proof of concept, we documented detailed steps and the exploit's requirements and sent them to Microsoft, following Microsoft's


[security vulnerability disclosure process](https://www.microsoft.com/en-us/msrc/faqs-report-an-issue?rtc=1) .


Email confirmation was given from Microsofot Security Response Center reporting the issue has been resolved. To fix the issue, a serializer was modified to properly encode script tags to prevent cross-site scripting payloads from executing from within the notifications in the SharePoint application.


By identifying this vulnerability and working with the Microsoft team to properly get the issue resolved, Target's Security Testing Services team was able to identify and eliminate Target's risk to the cross-site scripting attack vector within the Sharepoint application.
