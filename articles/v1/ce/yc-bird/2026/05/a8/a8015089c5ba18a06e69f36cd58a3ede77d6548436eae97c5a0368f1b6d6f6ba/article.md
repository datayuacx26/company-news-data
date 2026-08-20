---
schema_version: "1.0.0"
document_id: "a8015089c5ba18a06e69f36cd58a3ede77d6548436eae97c5a0368f1b6d6f6ba"
company_key: "yc-bird"
company: "Bird"
source_id: "yc-bird-news-import-1ae5a03d7866"
canonical_url: "https://bird.com/en-us/blog/smtp-vs-imap"
published_at: "2026-05-09T00:00:00+00:00"
first_seen_at: "2026-07-21T10:23:31.194838+00:00"
fetched_at: "2026-07-28T21:45:28.384377+00:00"
content_hash: "sha256:66ca7ccfb6f9bd0d6da6da05b12701e49b6e651fe1e777f57e54a6fb3f582dbe"
---

# SMTP vs. IMAP: What's the Difference?

SMTP and IMAP solve opposite halves of email. SMTP (Simple Mail Transfer Protocol) is how mail gets sent and relayed between servers. IMAP (Internet Message Access Protocol) is how a client retrieves and syncs mail that already arrived. One pushes messages out, the other pulls them in, and most mail setups use both.


## What does SMTP do?


SMTP moves a message from a sender toward its destination. Your mail client (or your application) hands the message to an SMTP server, which then relays it server to server until it reaches the recipient's mail provider. It is a push protocol focused entirely on transmission, with no concept of folders or reading. When you "send" an email, SMTP is the part doing the sending.


For an application sending receipts or password resets, SMTP is one of two valid paths: connect to an SMTP relay, or call an HTTP email API. Either way, you are using SMTP-style delivery, not IMAP.


## What does IMAP do?


IMAP retrieves mail from a server and keeps it in sync across devices. When you open an inbox on your phone and your laptop and see the same read and unread state, that is IMAP keeping both clients aligned with the copy on the server. Messages stay on the server; the client holds a synchronized view.


POP3 (Post Office Protocol) is the older retrieval option. It downloads messages to one device and traditionally removes them from the server, so it does not sync across devices the way IMAP does. If you want the same mailbox state everywhere, IMAP is the protocol that does it.


## How do SMTP and IMAP compare?


SMTP IMAP


Purpose Send and relay mail Retrieve and sync mail


Direction Push (outbound) Pull (inbound)


Default ports 587 (submission), 465 (implicit TLS), 25 (server-to-server relay) 143 (plain/STARTTLS), 993 (implicit TLS)


Model Transmission only Folder and state sync


Who uses it Mail clients and apps sending mail; mail servers relaying Mail clients reading an inbox


A note on ports: 587 is the standard submission port a client uses to hand a message to its server, 465 is the TLS-wrapped variant, and 25 is reserved for server-to-server relay (and is widely blocked for direct client use). On the IMAP side, 993 is the encrypted port you almost always want over plain 143.


## Which one does my app need?


If your application sends mail (transactional or otherwise), you want SMTP or an HTTP email API, never IMAP. IMAP is for reading an existing mailbox, so it does not help you deliver a message.


The practical choice for sending is between running SMTP yourself and calling an HTTP API. SMTP works everywhere and integrates with existing libraries, but you manage connection handling and credentials. An HTTP API skips the socket entirely, which matters in serverless environments. Bird's[email sending product](https://bird.com/en-us/products/email/sending) supports both, and the[sending email guide](https://bird.com/en-us/docs/guides/email/sending-email) walks through getting a first message out. If you are working in Node,[sending mail with Nodemailer](https://bird.com/en-us/blog/sending-emails-with-nodemailer) shows the SMTP path concretely.


What if you actually need to receive and process mail (handle replies, build a ticketing flow)? That is a different job from IMAP polling, and an[inbound email API](https://bird.com/en-us/blog/what-is-an-inbound-email-api) is usually the cleaner way to do it.


A quick way to keep the two straight: think about which direction the message is moving. Anything leaving your system toward a recipient is SMTP territory (or an HTTP send API). Anything you are reading out of an existing mailbox is IMAP territory. Apps almost always live on the sending side, because they generate mail rather than store an inbox a human browses. The moment you find yourself reaching for IMAP in application code, it is worth asking whether you actually want to send (SMTP) or to receive parsed mail (an inbound API), because raw IMAP polling is rarely the right tool for either.


## Frequently asked questions


### Can I use IMAP to send email?


No. IMAP only retrieves and organizes mail that already exists on a server. Sending requires SMTP or an HTTP email API. The two are designed for different directions of traffic.


### What is the difference between IMAP and POP3?


Both retrieve mail, but IMAP keeps messages on the server and syncs state across all your devices, while POP3 typically downloads messages to a single device and removes them from the server. IMAP is the better choice for multi-device access.


### Which SMTP port should I use?


Use 587 for client-to-server submission with STARTTLS, or 465 for an implicitly encrypted connection. Port 25 is meant for server-to-server relay and is commonly blocked for direct sending, so avoid it from application code.


For sending mail at scale, the[email product overview](https://bird.com/en-us/products/email/sending) and the[sending email guide](https://bird.com/en-us/docs/guides/email/sending-email) cover both the SMTP relay and the HTTP API in detail.
