---
schema_version: "1.0.0"
document_id: "e74cd9278041d635c2b8e5c1dee874e20413cf23a6eb673d0ec7facd34dbcfea"
company_key: "yc-bird"
company: "Bird"
source_id: "yc-bird-news-import-1ae5a03d7866"
canonical_url: "https://bird.com/en-us/blog/sending-emails-with-nodemailer"
published_at: "2026-05-03T00:00:00+00:00"
first_seen_at: "2026-07-21T10:23:31.194838+00:00"
fetched_at: "2026-07-28T21:25:33.541420+00:00"
content_hash: "sha256:52f8c6eac3377d93d2d8552d5076db628c73c22ec643dda7f50356219116da71"
---

# Sending Emails With Nodemailer

Nodemailer is the standard library for sending email from a Node.js server. You create a transport pointed at an SMTP server, then call` sendMail` with the message fields. It handles connection pooling, attachments, and HTML bodies. The code below is the minimum you need to send a real message, plus the testing trick most people miss.


## How do you install and configure Nodemailer?


Install it from npm:


```text
npm   install   nodemailer
```


A transport holds your SMTP connection details. You point it at a host and port, then pass credentials under` auth` :


```text
import   nodemailer   from   "  nodemailer  "  ;


const   transporter   =   nodemailer  .  createTransport  ({
host  :   "  smtp.example.com  "  ,
port  :   587  ,
secure  :   false  ,   // true for port 465, false for 587 (STARTTLS)
auth  :   {
user  :   process  .  env  .  SMTP_USER  ,
pass  :   process  .  env  .  SMTP_PASS  ,
},
});
```


Port 587 with` secure: false` uses STARTTLS, which upgrades the connection to TLS after the handshake. Port 465 wants` secure: true` . Keep the username and password in environment variables, never in the source.


## How do you send a message?


Call` sendMail` with the envelope and content. It returns a promise, so wrap it in` async` /` await` with a try/catch so a failed send does not crash the process:


```text
async   function   main  ()   {
try   {
const   info   =   await   transporter  .  sendMail  ({
from  :   '  "Your App" <you@yourdomain.com>  '  ,
to  :   "  recipient@example.org  "  ,
subject  :   "  Your receipt  "  ,
text  :   "  Thanks for your order. This is the plain-text version.  "  ,
html  :   "  <p>Thanks for your order.</p>  "  ,
});
console  .  log  (  "  Sent:  "  ,   info  .  messageId  );
}   catch   (  err  )   {
console  .  error  (  "  Send failed:  "  ,   err  );
}
}


main  ();
```


Always send both` text` and` html` . Some clients render the plain-text part, and a message with no text alternative tends to score worse with spam filters.


## How do you add attachments?


Pass an` attachments` array. Each entry can read from a path, a buffer, or a stream:


```text
await   transporter  .  sendMail  ({
from  :   "  you@yourdomain.com  "  ,
to  :   "  recipient@example.org  "  ,
subject  :   "  Your invoice  "  ,
text  :   "  Invoice attached.  "  ,
attachments  :   [
{
filename  :   "  invoice.pdf  "  ,
path  :   "  ./invoices/invoice-1042.pdf  "  ,
},
{
filename  :   "  note.txt  "  ,
content  :   "  Generated at runtime, no file needed.  "  ,
},
],
});
```


## How do you test without sending real mail?


Nodemailer ships with[Ethereal](https://ethereal.email/) , a fake SMTP service that captures messages instead of delivering them. You get a preview URL for each send, which is ideal for local development:


```text
const   testAccount   =   await   nodemailer  .  createTestAccount  ();


const   transporter   =   nodemailer  .  createTransport  ({
host  :   "  smtp.ethereal.email  "  ,
port  :   587  ,
secure  :   false  ,
auth  :   {
user  :   testAccount  .  user  ,
pass  :   testAccount  .  pass  ,
},
});


const   info   =   await   transporter  .  sendMail  ({
from  :   "  you@yourdomain.com  "  ,
to  :   "  test@example.org  "  ,
subject  :   "  Preview only  "  ,
text  :   "  This never leaves Ethereal.  "  ,
});


console  .  log  (  "  Preview:  "  ,   nodemailer  .  getTestMessageUrl  (  info  ));
```


Ethereal is for inspecting what you built. It tells you nothing about whether a real inbox would accept the message.


## Send it with Bird


SMTP works, but running it in production means operating a mail server (or renting one), warming IPs, and watching reputation yourself. An HTTP API hands that off. There is no SMTP socket to keep open, and the provider manages deliverability infrastructure. With the[Bird email API](https://bird.com/en-us/products/email/sending) the same send is a single call:


```text
import   {   BirdClient   }   from   "  @messagebird/sdk  "  ;


const   bird   =   new   BirdClient  ({   apiKey  :   process  .  env  .  BIRD_API_KEY  !   });


const   {   data  ,   error   }   =   await   bird  .  email
.  send  ({
from  :   "  you@yourdomain.com  "  ,
to  :   [  "  delivered@bird.dev  "  ],
subject  :   "  Hello from Node  "  ,
html  :   "  <p>It works.</p>  "  ,
})
.  safe  ();
```


The` delivered@bird.dev` sandbox address always accepts mail, so you can confirm the wiring before you point at a real recipient. Serverless platforms are the clearest case for this approach, since their function runtimes often cannot hold open SMTP connections at all. See[how to send email on Vercel](https://bird.com/en-us/blog/how-to-send-email-on-vercel) for that path, and the broader[sending email with JavaScript](https://bird.com/en-us/blog/sending-email-with-javascript) overview for where email code belongs in your stack.


## FAQ


### Does Nodemailer work in the browser?


No. Nodemailer is a server-side library and depends on Node's networking modules. Browsers cannot open SMTP connections, and putting credentials in client code would expose them. Send from a backend instead.


### Why send both text and HTML?


A multipart message lets each client pick the part it can render, and the plain-text alternative improves how spam filters read the message. Skipping` text` is a common reason for[emails landing in spam](https://bird.com/en-us/blog/why-are-my-emails-going-to-spam) .


### Should I use SMTP or an HTTP API?


SMTP is fine for low volume and internal tools. For production sending at scale, an HTTP API removes the server you would otherwise operate and folds in[deliverability](https://bird.com/en-us/products/email/deliverability) handling. Many teams use Nodemailer in development and an API in production.


Nodemailer is a solid choice for talking SMTP from Node. When you would rather not run the mail server behind it, read the[sending email guide](https://bird.com/en-us/docs/guides/email/sending-email) to see the API alternative end to end.
