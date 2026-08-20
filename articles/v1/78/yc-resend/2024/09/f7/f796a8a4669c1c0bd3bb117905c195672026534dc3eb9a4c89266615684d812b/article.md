---
schema_version: "1.0.0"
document_id: "f796a8a4669c1c0bd3bb117905c195672026534dc3eb9a4c89266615684d812b"
company_key: "yc-resend"
company: "Resend"
source_id: "yc-resend-rss-9474f2be6342"
canonical_url: "https://resend.com/blog/email-changes-coming-to-ios-18"
published_at: "2024-09-19T00:00:00+00:00"
first_seen_at: "2026-07-20T23:20:56.405079+00:00"
fetched_at: "2026-07-28T22:01:06.812214+00:00"
content_hash: "sha256:09521a27be965ec1330d7f22ef4d4a8c410a358e1792aee219e2c1a43bc1b17a"
---

# Email changes coming to iOS 18

When iOS 18.1 comes out in October, it will include Apple Intelligence for newer iPhones. Apple Mail for iPhones with Apple Intelligence will significantly change how it displays previews or preheader text when that feature is released.


## What is a preview text


Preview text is the first few sentences of an email you see when viewing your email from your inbox.


An example of a preview text


There's a trick to change what text is at the top of the email. A sender can put text that is not displayed when viewing the email via CSS at the top of the HTML. The email client will then display this text in the inbox.


At an implementation level, here's what that looks like:


```text
<  div style  =  "display: none; overflow: hidden; line-height: 1px; opacity: 0; max-height: 0; max-width: 0;"  >        Custom preview text goes here    <  /  div  >
```


## What changes with iOS 18


In iOS 18.1 with Apple Intelligence, Apple Mail will rewrite the preview text with a sentence or two summarizing the email. The summary will ignore the hidden text that the sender included in the message. When viewing a message, there'll also be an option to summarize the entire email into a paragraph.


The preview summary feature will also use an icon shared accross different iOS summary features denoting the use of Apple Intelligence. It looks a little like a backward return symbol (⏎). There'll also be an option for Apple Mail to highlight Priority emails, which Apple Intelligence has deemed important at that given moment.


Preview text in iOS 18.1


iPhones that will include Apple Mail summaries with Apple Intelligence:


Phone Apple Intelligence


iPhone 16 Pro Yes


iPhone 16 Yes


iPhone 15 Pro Yes


iPhone 15 No


iPhone 14 pro and older No


Here's the specific moment from WWDC when Apple announced these summary features.


## The future of preview text


Does this mean the end of the preview text trick? Not yet.


- Not every iPhone will support Apple Intelligence and this is Apple Mail only.
- Apple Intelligence will also start as US English only and will not be available in the EU for the release of iOS 18.1. Broad support for English outside the US and other languages will come later.


While Gmail and other email clients may introduce similar features in the future, senders should continue to include preview text for backward compatibility. However, it's important to remember that this text may not always be displayed in the inbox as intended, and email clients won't always display the email as expected.


## What about Email Categories and iOS 18?


You may also have heard Apple Mail is coming out with a categorization or tabs feature (Primary, Promotional, etc.).


Here's the specific moment from WWDC where they announce categorization features.


That's coming later this year and will have wider iPhone support than the summary features. We'll keep an eye on that as it gets closer to release and share what we learn.
