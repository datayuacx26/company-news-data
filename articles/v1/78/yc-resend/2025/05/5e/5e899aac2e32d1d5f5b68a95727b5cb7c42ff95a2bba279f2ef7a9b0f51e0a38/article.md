---
schema_version: "1.0.0"
document_id: "5e899aac2e32d1d5f5b68a95727b5cb7c42ff95a2bba279f2ef7a9b0f51e0a38"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/why-your-emails-are-going-to-spam"
published_at: "2025-05-21T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:57:40.062421+00:00"
content_hash: "sha256:558b057f183072f388d900d3f81cf70c51534f4a2d0761092f177501d40b6810"
---

# Why Your Emails are Going to Spam

Email communication isn't so different from traditional mail delivery.


Imagine inbox providers (such as Gmail, Yahoo, Outlook, Proton, QQ, etc.) as mailbox concierge services that offer a mailbox and manage delivery to your digital doorstep. Where does Resend fit in? We're your digital postal carrier, dropping off emails to your inbox providers.


## What are the best practices for email delivery?


Just as physical mail follows standards for addressing envelopes, email has specific best practices to ensure delivery:


1. **Contain clear sender info:** Emails should include a recognizable 'From' address, a clear subject line, and valid company contact details.
2. **Include an unsubscribe option:** The[CAN-SPAM Act](https://www.ftc.gov/business-guidance/resources/can-spam-act-compliance-guide-business) mandates you include an unsubscribe option for business emails.
3. **Monitor for prohibited content:** Like a postal service screening for prohibited items, Resend monitors email content to comply with regulations and our[Acceptable Use Policy](https://resend.com/legal/acceptable-use) .


## Why are emails marked as spam?


Emails are marked as spam when inbox providers detect signs that the message is unwanted, suspicious, or low-quality.


### 1. Sender reputation


Inbox providers evaluate the sender’s reputation based on past behavior.


There’s a big difference between receiving a trusted letter from the post office versus getting thousands of unsolicited flyers from a pizza shop down the street. The latter feels intrusive, and that’s exactly how inbox providers view emails with high complaint rates or poor sending practices.


- **Spam Complaints:** Too many users manually mark emails as "spam." No one wants mail that they didn’t sign up for. Keep your complaints under **0.08%.**
- **Bounce Rates:** Sending emails to invalid addresses repeatedly? That's like ringing the wrong doorbell over and over! Inbox providers notice this behavior. Resend requires keeping bounce rates below **4%** .
- **Sending Habits:** Sudden surges in email volume, jumping from 100 to 10,000 overnight, raise alarm bells. Steady sending habits are the way to go!


Inbox providers constantly evaluate and reevaluate. It's not a one-change-fix-all mentality, but requires active maintenance. If the seed of suspicion is planted, it takes a while to rebuild trust.


Resend manages a high-reputation sender pool to ensure your email reaches the inbox as much as possible.


### 2. Sender verification


Authenticate your emails. It helps receivers identify who’s responsible for the message, confirm that the sending IP is authorized, and ensure the message hasn’t been altered in transit.


When inbox providers receive an email, they check the sender's authentication DNS records. If the records are not found or are incorrect, inbox providers may mark the email as spam, reject it, or degrade your sender reputation.


Here are the three records you should set up for every sender:


- **SPF (Sender Policy Framework):** A list of authorized IPs for your domain's email, helping receivers see that you’ve taken responsibility for your sending infrastructure.
- **DKIM (DomainKeys Identified Mail):** A digital signature that helps verify your emails haven’t been tampered with.
- **DMARC (Domain-based Message Authentication):** Gives inbox providers instructions on handling suspicious emails that fail authentication or authenticate with different domains than your own. DMARC compliance has become a standard for most large inbox providers like Google, Yahoo, Apple, and Microsoft.


Need more help? View our[domain setup guide](https://resend.com/docs/dashboard/domains/introduction) .


### 3. Content relevance


Inbox providers carefully review your emails. Here are a few things to keep in mind:


- **Maintain Consistency:** Just like you wouldn’t expect to receive a bill in the mail that’s actually an appliance ad, your email’s subject line and content should reflect its purpose. Misleading or mismatched messaging can erode trust and trigger spam filters.
- **Align Links with Your Domain:** Ensure that all URLs and redirects in your email align with your sender domain. Unexpected or mismatched links can raise red flags with inbox providers.


## How can you improve your email deliverability?


Resend offers internal tools, but you can also use third-party tools.


### Resend deliverability insights


Resend offers[email insights](https://resend.com/docs/dashboard/emails/deliverability-insights) for each email to help you improve email deliverability.


### Third-party tools


Inbox providers have their own filters, rules, and algorithms to manage spam. They don’t share their criteria openly to protect themselves and spammers from bypassing their rules. Here are a few tools that can help you improve your email deliverability.


**1. Google Postmaster Tools**


[Google Postmaster Tools](https://gmail.com/postmaster/) is a free tool offered by Google that provides insights on your sender reputation. It is a fantastic way to see how an inbox provider is perceiving your email, as long as you send enough mail to anonymize the data.


Get help[setting up Google Postmaster Tool](https://support.google.com/a/answer/9981691?sjid=4594872399309427672-NA&visit_id=638259770782293948-1913697299&rd=2) .


**2. Email mailbox messages**


Google provides feedback for individual emails as well, unlike some providers, like Outlook. On every spam email, Google includes a grey box with feedback on why this particular email landed in the spam folder.


**3. Yahoo! Sender Hub**


Yahoo! offers a service called[Sender Hub](https://senders.yahooinc.com/) . It helps high-volume senders identify opportunities to improve how their emails are received and gives you insight into complaints reported by Yahoo.


**4. Google Safe Browsing**


When adding links to your emails, use[Google Safe Browsing](https://transparencyreport.google.com/safe-browsing/search?hl=en) service to test them first. The tool checks URLs against a database of known malicious sites, helping protect you from linking to compromised domains.


**5. Spamhaus**


[Spamhaus](https://check.spamhaus.org/) is a respected real-time blackhole list provider used by mailboxes for email filtering.


If your sending domain or URLs in your message are listed by Spamhaus, you’ll have email deliverability issues. It’s good practice to regularly check your domain, IP addresses, or email URLs in Spamhaus to ensure they don’t appear on their blocklists.


## How to recover your email reputation after a dip


1. **Audit your authentication** settings (SPF, DKIM, and DMARC).
2. **Clean your list** by removing bounced, inactive, or unengaged users.
3. **Review and test your content** for spam triggers, formatting issues, or mismatched links.
4. **Keep an eye on your domain reputation** using tools like Google Postmaster Tools.
5. **Slowly ramp up volume** once your engagement metrics improve — don’t rush it.


[Reach out to us](https://resend.com/contact) if you need a hand! Our team includes deliverability experts who can guide you through the process.
