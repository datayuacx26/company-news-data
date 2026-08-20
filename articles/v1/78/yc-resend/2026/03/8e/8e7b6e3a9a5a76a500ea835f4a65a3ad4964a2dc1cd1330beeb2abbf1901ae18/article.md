---
schema_version: "1.0.0"
document_id: "8e7b6e3a9a5a76a500ea835f4a65a3ad4964a2dc1cd1330beeb2abbf1901ae18"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/introducing-dmarc-analyzer"
published_at: "2026-03-11T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T20:53:30.609843+00:00"
content_hash: "sha256:aa2bdae1e1dc00f4c65dc1fc68953bbc760c6f22b1a915def0a9baaa9cd048d7"
---

# Introducing DMARC Analyzer

DMARC is one of the most important protocols for email authentication. It protects your domain from spoofing, tells mailbox providers how to handle unauthenticated emails, and gives you reports about what's happening with every message sent from your domain.


## Introducing DMARC Analyzer


Today we're releasing[DMARC Analyzer](https://github.com/resend/resend-dmarc-analyzer) —a free, open-source tool that parses DMARC XML reports and turns them into something you can actually understand. No account required. No monthly fee.


## Capturing DMARC reports


When you[create your DMARC record](https://resend.com/docs/dashboard/domains/dmarc#1-add-a-txt-dmarc-record) , you can specify which reports to receive.


## Problem with DMARC reports


Receiving reports gives you visibility into your email authentication. The problem? The reports arrive as raw XML with Unix timestamps, nested tags, and raw IP addresses.


```text
<  feedback  >        <  report_metadata  >          <  org_name  >  google  .  com  <  /  org_name  >          <  email  >  noreply  -  dmarc  -  support@google  .  com  <  /  email  >          <  date_range  >            <  begin  >  1706745600  <  /  begin  >            <  end  >  1706831999  <  /  end  >          <  /  date_range  >        <  /  report_metadata  >        <  policy_published  >          <  domain  >  company  .  com  <  /  domain  >          <  adkim  >  r  <  /  adkim  >          <  aspf  >  r  <  /  aspf  >          <  p  >  quarantine  <  /  p  >          <  sp  >  reject  <  /  sp  >          <  pct  >  100  <  /  pct  >        <  /  policy_published  >        <  record  >          <  row  >            <  source_ip  >  203.0  .113  .45  <  /  source_ip  >            <  count  >  2410  <  /  count  >            <  policy_evaluated  >              <  disposition  >  none  <  /  disposition  >              <  dkim  >  pass  <  /  dkim  >              <  spf  >  fail  <  /  spf  >            <  /  policy_evaluated  >          <  /  row  >          <  identifiers  >            <  header_from  >  company  .  com  <  /  header_from  >          <  /  identifiers  >          <  auth_results  >            <  dkim  >              <  domain  >  company  .  com  <  /  domain  >              <  result  >  pass  <  /  result  >            <  /  dkim  >            <  spf  >              <  domain  >  mail  .  company  .  com  <  /  domain  >              <  result  >  fail  <  /  result  >            <  /  spf  >          <  /  auth_results  >        <  /  record  >     <  /  feedback  >
```


The reports are technically human-readable, but just barely.


We've written about[how to read a DMARC report](https://resend.com/blog/how-to-read-a-dmarc-report) and[how DMARC policies work](https://resend.com/blog/dmarc-policy-modes) . But knowing how to read XML doesn't make it any less tedious to do it every day.


## The cost of monitoring DMARC


An entire industry has grown around solving this problem, but many of them start at $30+/month, which is a significant cost for a glorified XML parser.


The consequence? For many teams, DMARC reports simply go unread. The data is there, but nobody looks at it. Ignoring this data opens a gap in your email security posture and deliverability.


## How to use DMARC Analyzer


We've built DMARC Analyzer to be easy to use and deploy. There are two ways to use it.


### 1. Web interface to check single reports


The fastest way to get started. Go to[checkdmarc.email](https://checkdmarc.email/) , paste your DMARC XML report (or upload the file), and get parsed results instantly.


You'll see your SPF and DKIM alignment results broken down by source IP, which senders are passing or failing authentication, and how receiving mail servers are handling your emails. No signup, no setup—just paste and go.


### 2. Self-hosted with automated reports


For ongoing monitoring, you can deploy your own instance. Connect it to[Resend Receiving](https://resend.com/docs/dashboard/receiving/introduction) to automatically ingest DMARC reports as they arrive, and get email digests summarizing your authentication results. You own the stack and the data.


Here's how it works:


1. DMARC aggregate reports arrive at an email address on your domain
2. [Resend captures the inbound email](https://resend.com/docs/dashboard/receiving/introduction)
3. The application parses the XML attachment and extracts SPF/DKIM results
4. The application emails you a styled digest email (built with[React Email](https://react.email/) ) with a clear summary of the report


No manual XML parsing. No monthly subscription. Your DMARC data, delivered to your inbox in a format that's actually useful.


## Built with the Resend stack


DMARC Analyzer is built with:


- **[React Email](https://react.email/)** for the digest email templates
- **[Resend Receiving](https://resend.com/docs/dashboard/receiving/introduction)** for automated inbound report ingestion
- **[Resend](https://resend.com/)** for sending the digest emails


The project also serves as a practical example of how these pieces fit together. If you're building an application that processes inbound email, the patterns in this codebase—webhook handling, attachment parsing, email rendering—are directly reusable.


## Get started


DMARC Analyzer is available now and completely free to use.


- **Try it now** —[checkdmarc.email](https://checkdmarc.email/)
- **View on GitHub** —[resend/resend-dmarc-analyzer](https://github.com/resend/resend-dmarc-analyzer)
- **Read the docs** —[DMARC Analyzer documentation](https://resend.com/docs/dmarc-analyzer)


If you're already collecting DMARC reports, paste one in and see what it looks like. If you haven't set up DMARC reporting yet, check our guide on[setting up DMARC](https://resend.com/docs/dashboard/domains/dmarc) to get started.
